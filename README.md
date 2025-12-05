# ARM TrustZone
## Open questions
- Use CCA or TrustZone?
- Study the implementation of the third mode/world?
## Useful links
- [Trusted Firmware](https://www.trustedfirmware.org/)
- [ARM CCA](https://www.arm.com/architecture/security-features/arm-confidential-compute-architecture) (see key resources section to access several guides)
- [ARM developer RME learning resource for armv9-A](https://developer.arm.com/documentation/den0126/0102)
- [KVM CCA](https://www.cs.columbia.edu/~nieh/pubs/osdi2022_cca.pdf) (paper from Columbia)
- [Veraison](https://github.com/veraison)
- [ARM Learning Path - CCA Attestation and Veraison](https://learn.arm.com/learning-paths/servers-and-cloud-computing/cca-veraison/)
- [ARM Developer AArch64 Exception Model](https://developer.arm.com/documentation/102412/0103)
- [RME System Architecture](https://developer.arm.com/documentation/den0129/ba) (additional reading section may contains other interesting sources)
- [ARM A-Profile refernces](https://www.arm.com/architecture/learn-the-architecture/a-profile)
- [ARM architecture security features](https://www.arm.com/architecture/security-features) (very high level content, maybe it will be useful for some images to put into the slides)
- [CCA Architecture](https://developer.arm.com/documentation/den0125/400)
- [Draft - Arm's Confidential Compute Architecture Reference Attestation Token](https://datatracker.ietf.org/doc/draft-ffm-rats-cca-token/)
- [TrustZone and CAA comparison](https://fengweiz.github.io/paper/sok-seed24-slides.pdf)
- [Resources from ARM Developer Blog](https://developer.arm.com/search#numberOfResults=48&q=confidential%20computing%20architecture&f-navigationhierarchiescontenttype=Forum%20Thread,Blog%20Post)
- [Run an end-to-end Attestation Flow with Arm CCA](https://learn.arm.com/learning-paths/servers-and-cloud-computing/cca-essentials/)
- [Paper - Formal Verification of CAA](https://ieeexplore.ieee.org/document/10373038)
- [Presentation - ARM Attestation](https://static.linaro.org/connect/armcca/presentations/CCATechEvent-210623-SF.pdf)
- [OpenCCA to simulate CCA](https://ieeexplore.ieee.org/document/11129595)

## Summary
- TrustZone vs CCA focusing on problems of TrustZone and how they are patched with CCA
- RME architecture
- Attestation/Remote attestation
- Software and firmware

## Presentation Summary
- why TEE
- TrustZone introduction (and attestation?)
- trustzone downsides (not isolation between applications and not a root state)
- background knowledge (mpe, gpt, exception model...)
- Arm CCA motivations (repeat shortly)
- companies involved (arm, linaro, huawei, CCC)
- Arm CCA architecture hardware (rmi, hes)
- Arm CCA architecture software (rmm, secure monitor, hes sw)
- TrustZone vs CCA (features and downsides) (pro vs cons)
- attestation trustzone vs cca (maybe do some introduction on turstzone before)
- summary and use case (namely, scenarios in which CCA is better)
- future direction (personal considerations?)

## Logistic
- Max 40 pages for Lioy, the imprtant thing is to have material and do not do a long write without material

## Slide Format
- Font Size: 20 
- Slide Format: A4 
- Font: one of the sans serif font
- 10-15 minutes for each person, more or less 1 slide per minute (?)
- pictures are appreciated if big enough
- deliverable: Latex report + presentation or longer presentation (add doi of documents)

