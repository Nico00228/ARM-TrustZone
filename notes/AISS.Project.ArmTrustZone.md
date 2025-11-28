# Processor
	- TrustZone enables a secure state, the only way to get the privileges is via exception and note that the Firmware/Secure Monitor is the only component at EL3. The bit that control on which security state it is the processor is the `SCR_EL3.NS` bit that if it is 0 is secure state, else non-secure ![image.png](../assets/image_1764064179700_0.png)
	  id:: 69257b02-5ce4-4c45-bedf-c8361ba29c6f
	- In order to switch between these security states, this bit has to be changed but the thing here is that it has to be performed at EL3, so an exception that requests a secure state via FIQ or SMC, it will give the work to to the Secure Monitor that will change the bit and save the non-secure registers state and restore the secure registers state. So from there another exception can decrease the privileges in the secure state in order to access services or applications in the secure world. Note that a possible secure monitor is Trusted Firmware
	- The memory management is divided by secure and non-secure world, with two translation regime and they are indicated with a prefix as `NS.EL1:address` or `S.EL1:address`. This is the translation from virtual to physical, so both states can access their own translation regime
	- For the physical addresses instead the secure state can access the physical memory, but the vice-versa is prohibited. To identify to which state they are associated a prefix is present: `SP:address` or `NP:address`
	- In the caches if the physical address that has been translated is a secure one can be hit only if the request has been performed by the secure state, instead if it is non-secure both from secure and non-secure
	- The TLB is used to store the previous translations but as the cache also here it is present the security state and the exception level the translation has occurred, so when there is an invalidation of TLB entries the entries affected are the one on which the security state matches the current state. The only exception is on EL3 that has a particular behavior because it is upon the software to decide ![image.png](../assets/image_1764065827063_0.png)
	  id:: 692580b6-060e-4cea-a0e2-75ef40c3828c
	- The SMC (Secure Monitor Call) can be called to access firmware functionalities or a service so a SMC dispatcher is needed to dispatch the requests
	- The virtualization has been introduced in the processors from Armv8.4-A due to the fact that specific services required specific trusted kernels and also that to follow the least privilege schema some firmware services had to been moved into a lower exception level ![image.png](../assets/image_1764066106672_0.png){:height 189, :width 718}
	- # System architecture
		- ![image.png](../assets/image_1764066177880_0.png)
		- We do not have only the processor but multiple devices, as we said the divison of the physical memory we have to address how we guarantee these accesses with the bus. The bus is built so that it grant access to devices that are in a possible state depending on the request and os we can divide these devices on how they are implemented:
			- TrustZone aware if the devices use the bus transaction to guarantee security (GIC)
			- Non-TrustZone aware if the device is only secure or non-secure (on-chip memory, timer)
		- To enforce the isolation of the devices it is possible to define the behavior of each completer:
			- Secure
			- Non-secure
			- Boot time configurable
			- TrustZone aware
		- This mechanism is good if the completer is present only in one address space or if it is trust aware, instead if we have more partition a TZASC is feasible to use ![image.png](../assets/image_1764066988941_0.png)
		- There are also other devices that can request access to the bus and can be divided into aware and non. For the firsts they implement on their own the mechanism to request secure spaces, instead for the latter we can have different possibilities:
			- Design time tie-off, tying off the specific signal to a specific address a device has to access
			- Configurable logic, at boot time there is an override to grant access to a specific address
			- SMMU, like an MMU. It includes the NS bit so it can control which physical address is accessed 6
			  ![image.png](../assets/image_1764067449614_0.png)
		- There are also other type of processors, like the M versions (low level) that for v8 do not implement TrustZone or also the R versions (Real time), so they are seen as like other bus requester non aware
		- The interrupts in the system are divided into:
			- Group 0, secure interrupts as FIQ, for Firmware management
			- Secure Group 1, secure interrupts as FIQ or IRQ, for EL1/EL2 management (software)
			- Non-secure Group 1, non-secure interrupts as IRQ or FIQ
		- To handle interrupts this is how they are generated in base of the security state ![image.png](../assets/image_1764067980106_0.png)
		- There is the funcionality to debug however it has to be controlled in a way that after the manufacturer sell the item not all the functionalities can be debugged such as the security state, so there are signals that control these possibilities and to disable is to blowing the fuses. The signals are `DBGEN` controls external debug in both security state and the `SPIDEN` instead external debug in secure state. Another possibility is to have an authentication module
		- There are other devices like OTP, non-volatile counter and trusted RAM and ROM (on chip)
		- Arm Base System Architecture is the hardware architecture that system software can rely on, including boot, installation and run generic OS and hypervisor
	- # Software architecture
		- ![image.png](../assets/image_1764075038318_0.png)
		- From the previous image it is immediate to see that the software that request the secure service it has to be authenticated, according to the security that has to put in place for the specific service
		- There is also the problem of the scheduling because the processor can have only a state at time. Calling EL3 services these in general are blocking, however an untrusted software can block the change of state, but this has only a lack of availability not confidentiality. The software stack could be designed to give also availability and an example is the GIC that can grant priority to the security interrupts
		- An example of trusted kernel is OP-TEE and here there is the schema on how it is implemented in the TrustZone architecture
		  ![image.png](../assets/image_1764075797574_0.png)
		  We can see that there is a driver in the Rich OS useful to interact with the main kernel, also there is an API interface built by GlobalPlatform API standards in both secure and non-secure state. GlobalPlatform API standards are standards supported by different TEEs. `tee-supplicant` is used to deploy services that TEE support but requires rich OS interaction
		- The hypervisor does not want that a VM has access to all the firmware services, like for example power management so it implement a  SMC trap that is used to block the SMCs and emulate the firmware internally the hypervisor or forward the request to the real firmware
		- The boot flow is the one that guarantee that each component loaded before are trusted referred as the chain of trust, it is important to check the boot flow. In order:
			- First stage boot ROM (on chip), it is the one that cannot be changed and it is used only to load and check the second boot stage that is from a flash memory that will load the TEE and UEFI that will load the OS or hypervisor
		- If fail the boot checks? There are different actions that can be performed like for example if the second boot image is compromised, it is fatal and so the device cannot boot, or the TEE is compromised so the device could be booted however the TEE is not available. Instead if the rich OS is compromised it could be booted but with limit functionalities like for example without access to the TEE
		- In order to have guidelines to construct the trusted boot flow in a trustZone architecture there are the Trusted Board Boot Requirements
		- Trusted Firmware is an open source project sponsored by Arm that is used as firmware/secure monitor that can handle SMC dispatching them between firmware ones and TEE ones and also connect to specific IP architecture 
		  ![image.png](../assets/image_1764077092687_0.png)
	- # Usage examples
		- Encryption of the file system using only the OTP key to encrypt the key used to encrypt the filesystem, using an on-chip trusted RAM to check and decrypt the key used in a flash controller with crypto support
		- Over the air firmware update
		  ![image.png](../assets/image_1764077322701_0.png)
	- # Questions
		- **What are the security states and the physical addresses spaces in the ARM architecture?**
			- The security states are two:
				- Non-secure state
				- Secure-state
			- And the physical addresses are also two, the non-secure ones that can be seen by both security states and the secure ones seen only by the secure state
		- **For each Exception Level what determines whether the processor is in secure state or non-secure?**
			- It is determined by the register present at exception level 3, by the NS bit, called SCR_EL3.NS
		- **While in non-secure state, can software access the secure physical address space?**
			- No, it can be done the vice-versa, to access the secure physical address space the software has to be perform a request via exception to the secure state
		- **Can an access to `SP:0x80000` hit on cache line containing `NP:0x80000`?**
			- No, they are mapped as two distinct caches with different addresses
		- **What do The Arm Base System Architecture (BSA) and Platform Security Boot Guide provide guidance on?**
			- The BSA provides guidance on the hardware specification to run software on the ARM processor, instead the one about boot provides guidance on how it has to be performed the boot flow to be compliant to the ARM guidelines
		- **What is the purpose of a TRustZone Address Space Controller (TZASC)?**
			- It enables the partition in secure and non-secure memory
-