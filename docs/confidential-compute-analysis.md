# Confidential Computing: Intellipunk Analysis

## Overview

Confidential computing protects data in-use through hardware-based Trusted Execution Environments (TEEs). This technology is highly relevant to Intellipunk's verifiable compute and anti-monopoly goals—but it carries centralization risks that must be addressed.

## What is Confidential Computing?

[Confidential computing](https://confidentialcomputing.io/wp-content/uploads/sites/10/2023/03/CCC_outreach_whitepaper_updated_November_2022.pdf) uses hardware to create isolated execution environments where:
- Code executes in plaintext inside the TEE
- Data appears encrypted to everything outside the TEE
- Platform security processors manage access control
- [Remote attestation](https://confidentialcomputing.io/2023/04/06/why-is-attestation-required-for-confidential-computing/) proves what code is running

### Key Technologies (2026)

**AMD SEV-SNP** (Secure Encrypted Virtualization - Secure Nested Paging)
- Encrypts VM memory at runtime
- Better scalability (more VMs per socket)
- [General availability on Azure](https://learn.microsoft.com/en-us/azure/confidential-computing/overview-azure-products)

**Intel TDX** (Trust Domain Extensions)
- Memory encryption with hardware isolation
- 1-2% lower overhead on memory-bound workloads
- [Preview on Azure, GA on Google Cloud C3](https://dl.acm.org/doi/10.1145/3700418)

**NVIDIA GPU TEEs**
- [Confidential AI inference on H100 GPUs](https://next.redhat.com/2025/10/23/enhancing-ai-inference-security-with-confidential-computing-a-path-to-private-data-inference-with-proprietary-llms/)
- Protects both private data and proprietary models
- Preview on Google Cloud A3 machines

## Intellipunk Alignment: What Fits

### 1. Verifiable Compute
TEE [remote attestation](https://edera.dev/stories/remote-attestation-in-confidential-computing-explained) provides cryptographic proof:
- What code/model executed
- On what hardware platform
- With what security posture
- At what time

**Intellipunk application**: Guild nodes can prove they ran specified models with no backdoors.

### 2. Privacy-Preserving Inference
Confidential VMs protect:
- User queries from cloud provider
- Model weights from infrastructure operator
- Computation from privileged software

**Intellipunk application**: Local-first compute with hardware guarantees, even when edge nodes share infrastructure.

### 3. Open Source Tooling
[Confidential Computing Consortium](https://confidentialcomputing.io/projects/current-projects/) supports open source projects:
- **Confidential Containers**: Cloud-native TEE workloads
- **Enarx**: Platform abstraction for TEEs
- **Keystone**: RISC-V TEE framework (open hardware)
- **dstack**: Developer-friendly TEE SDK

**Intellipunk application**: Self-hosted guild nodes can use open runtimes instead of proprietary clouds.

## Intellipunk Conflict: What Doesn't Fit

### 1. Centralized Trust Roots

**Problem**: [Existing TEE attestation relies on centralized trust](https://arxiv.org/html/2402.08908v1):
- Single provisioned secret key per hardware vendor
- Centralized verifier (Intel, AMD, cloud provider)
- If root-of-trust is compromised, entire system fails

**Intellipunk violation**: Creates **computational feudalism**—you must trust AMD/Intel/NVIDIA to verify your compute.

**Quote from research**: *"This closed and centralized chain of trust is fragile since only one part of the chain being compromised will directly cause the failure of entire attestations."*

### 2. Hardware Manufacturer Gatekeeping

**Problem**: TEE requires specific CPUs/GPUs:
- AMD SEV-SNP, Intel TDX, NVIDIA H100 TEE
- Only available on expensive, latest-generation hardware
- Manufacturer controls firmware updates and attestation keys

**Intellipunk violation**: Creates **energy chokepoints** and locks out lower-cost, older hardware.

### 3. Cloud Provider Mediation

**Problem**: Most TEE deployments are cloud-only:
- [Azure confidential VMs](https://learn.microsoft.com/en-us/azure/confidential-computing/overview)
- [Google Confidential Space](https://cloud.google.com/confidential-computing/docs/confidential-computing-overview)
- Cloud provider mediates attestation and key management

**Intellipunk violation**: **Extractive clouds** still control access, pricing, and policy.

### 4. Opacity and Proprietary Firmware

**Problem**: TEE firmware is often closed-source:
- [54% of TEE CVEs come from firmware vulnerabilities](https://dl.acm.org/doi/10.1145/3700418)
- Attestation chains rely on vendor-signed certificates
- Limited auditability of actual security guarantees

**Intellipunk violation**: **Epistemic gatekeeping**—can't inspect what you're trusting.

## Working Theory: Decentralized Attestation

Recent research proposes decentralized alternatives to centralized trust roots.

### Approach 1: Multi-Party Computation (MPC) Root of Trust
[Phala Network's decentralized RoT](https://phala.com/posts/understanding-the-role-of-rootoftrust-in-tee-and-phalas-decentralized-approach):
- Use [Secure Multi-Party Computation](https://github.com/Phala-Network/phala-docs/blob/main/dstack/design-documents/decentralized-root-of-trust.md) to distribute root secrets
- No single entity can compromise the RoT
- Trust distributed across multiple independent nodes

**Mechanism**: Instead of trusting AMD's provisioning key, trust is split among N parties where M-of-N must agree.

### Approach 2: Blockchain-Based Verification
["Teamwork Makes TEE Work" proposal](https://arxiv.org/html/2402.08908v1):
- Smart contracts verify attestations on-chain
- Multiple independent verifiers (not single vendor)
- Public audit trail of attestation results
- Physically Unclonable Functions (PUF) as intrinsic RoT

**Mechanism**: Attestation results go to public blockchain; anyone can verify the verification.

### Approach 3: Open Hardware TEE
[FOSDEM 2026 on open firmware](https://fosdem.org/2026/schedule/event/LKWQL7-open_source_firmware_for_high_assurance_confidential_infrastructure/):
- RISC-V Keystone project provides open TEE
- Firmware is auditable
- No vendor lock-in to x86 ecosystems

**Mechanism**: Use open ISA (RISC-V) + open firmware + community attestation.

## Intellipunk Design Constraints

If guild nodes use confidential computing, these constraints apply:

### Required
1. **Open attestation verification**: Anyone can verify attestation quotes, not just vendor
2. **Auditability**: Firmware source available, reproducible builds
3. **Multiple trust anchors**: Support AMD SEV, Intel TDX, RISC-V Keystone, etc.
4. **Graceful degradation**: TEE is enhancement, not requirement
5. **Self-hostable**: Must work on bare metal, not just cloud VMs

### Preferred
1. **Decentralized RoT**: MPC-based or blockchain-verified attestation
2. **Open hardware**: RISC-V over x86 when feasible
3. **Public attestation logs**: Guild node publishes attestation quotes publicly
4. **Community verification**: Other guild nodes can cross-check attestations

### Rejected
1. **Cloud-only TEE**: No Azure/GCP-mediated attestation as sole option
2. **Vendor-locked attestation**: Must support multiple verifiers
3. **Closed firmware**: Proprietary TEE firmware without open alternative
4. **Single trust root**: One key to rule them all

## Compliance Pack Integration

The [compliance pack endpoint](../adr/002-compliance-pack-endpoint.md) should include:

```json
{
  "verifiable_compute": {
    "attestation_method": "tee_with_decentralized_verification",
    "technologies": [
      {
        "type": "amd_sev_snp",
        "version": "1.51",
        "firmware_hash": "sha256:...",
        "firmware_source": "https://github.com/amd/sev-snp-firmware",
        "trust_model": "vendor_root_of_trust",
        "decentralized_verification": {
          "enabled": true,
          "blockchain": "ethereum_sepolia",
          "verification_contract": "0x...",
          "attestation_log": "https://guild.example.org/attestations/"
        }
      }
    ],
    "fallback": {
      "without_tee": true,
      "transparency_mechanism": "public_container_logs",
      "note": "TEE enhances but doesn't replace transparency"
    }
  }
}
```

**Key additions**:
- `firmware_source`: Link to auditable firmware
- `trust_model`: Explicit declaration (vendor, decentralized, open-hardware)
- `decentralized_verification`: Optional blockchain/MPC verification
- `fallback`: Guild nodes must work without TEE

## Staged Adoption Strategy

### Phase 1: Transparency Without TEE (Now)
- Public container images with checksums
- Audit logs of all inferences
- No hardware requirements
- **Trust model**: Reputation + audit trails

### Phase 2: Vendor TEE with Open Verification (Q2-Q3 2026)
- Support AMD SEV-SNP / Intel TDX on self-hosted nodes
- Publish attestation quotes publicly
- Community tools to verify quotes independently
- **Trust model**: Hardware RoT + public verification

### Phase 3: Decentralized Attestation (2027)
- Integrate MPC-based root of trust (Phala dstack model)
- Smart contract attestation verification
- Cross-guild-node attestation checks
- **Trust model**: Distributed trust, no single vendor

### Phase 4: Open Hardware TEE (2028+)
- RISC-V Keystone or similar open TEE
- Fully auditable firmware
- Community-controlled attestation
- **Trust model**: Open hardware + open firmware + community governance

## Threat Model: TEE Compromise Scenarios

### Scenario 1: Vendor Key Extraction
**Attack**: Adversary extracts AMD/Intel provisioning keys from hardware.
**Impact**: Can forge attestations for any node claiming that vendor's TEE.
**Mitigation**: Decentralized verification (blockchain-based) catches forged attestations if honest verifiers exist.

### Scenario 2: Firmware Backdoor
**Attack**: Vendor or nation-state inserts backdoor in TEE firmware.
**Impact**: TEE provides false security; attacker reads "confidential" data.
**Mitigation**: Open firmware + reproducible builds + community audits. Phase 4 (open hardware) eliminates this entirely.

### Scenario 3: Cloud Provider Interception
**Attack**: Cloud provider exploits hypervisor to extract data from TEE.
**Impact**: Confidentiality violated despite TEE.
**Mitigation**: Self-hosted guild nodes on bare metal bypass this attack surface. Cloud-hosted nodes explicitly label trust assumptions.

### Scenario 4: Side-Channel Attacks
**Attack**: Spectre/Meltdown-style attacks leak data from TEE.
**Impact**: TEE isolation breached via microarchitecture.
**Mitigation**: No silver bullet. Firmware updates, microcode patches, architectural changes. Public disclosure of vulnerabilities essential.

## Recommendations for Intellipunk

### DO
- ✅ Support TEE as **optional enhancement**, not requirement
- ✅ Require public attestation quote publication
- ✅ Provide tools for community attestation verification
- ✅ Document trust model explicitly (vendor RoT vs. decentralized)
- ✅ Prefer open-source TEE runtimes ([Enarx](https://enarx.dev/), [Confidential Containers](https://github.com/confidential-containers))
- ✅ Plan for decentralized attestation (Phase 3)
- ✅ Support graceful degradation (guild nodes work without TEE)

### DON'T
- ❌ Mandate cloud-only TEE deployments
- ❌ Require latest-generation expensive hardware
- ❌ Accept vendor attestation as sole source of truth
- ❌ Hide firmware/attestation details
- ❌ Create single point of failure in attestation chain
- ❌ Claim "perfect security" from TEE alone

## Glossary Additions

Propose adding these terms to Intellipunk glossary:

**Confidential computing**: Hardware-based protection of data in-use via Trusted Execution Environments. Useful for verifiable compute but carries centralization risks via vendor-controlled attestation.

**Trusted Execution Environment (TEE)**: Hardware-enforced isolated environment for code execution. Provides confidentiality and integrity for running code. Not a substitute for transparency and auditability.

**Remote attestation**: Cryptographic proof that specific code is running on specific hardware with specific security properties. Essential for verifiable compute; must be decentralized to avoid vendor gatekeeping.

**Root of Trust (RoT)**: The hardware/firmware foundation for attestation chains. Centralized RoTs (vendor keys) create single points of failure. Decentralized RoTs (MPC, blockchain) distribute trust.

## Open Questions

1. **Should Intellipunk guild nodes mandate TEE?**
   - **Proposal**: No. Optional for enhanced privacy, not required for compliance.

2. **What attestation verification is "good enough"?**
   - **Proposal**: Phase 2 (vendor TEE + public quotes) acceptable if decentralized verification (Phase 3) is on roadmap.

3. **How to handle TEE vulnerabilities?**
   - **Proposal**: Public disclosure via compliance pack, fallback to non-TEE transparency.

4. **Can we trust open-source TEE firmware?**
   - **Working theory**: More trustworthy than closed, but still requires community auditing and reproducible builds.

## References

### Confidential Computing Overview
- [Confidential Computing Consortium White Paper](https://confidentialcomputing.io/wp-content/uploads/sites/10/2023/03/CCC_outreach_whitepaper_updated_November_2022.pdf)
- [Microsoft Azure Confidential Computing](https://learn.microsoft.com/en-us/azure/confidential-computing/overview)
- [Google Cloud Confidential Computing](https://cloud.google.com/confidential-computing/docs/confidential-computing-overview)

### TEE Technologies
- [AMD SEV-SNP vs Intel TDX Analysis](https://dl.acm.org/doi/10.1145/3700418)
- [Confidential AI Inference with TEE](https://next.redhat.com/2025/10/23/enhancing-ai-inference-security-with-confidential-computing-a-path-to-private-data-inference-with-proprietary-llms/)
- [NVIDIA GPU TEE Overview](https://blogs.nvidia.com/blog/what-is-confidential-computing/)

### Attestation and Trust
- [Why Attestation is Required](https://confidentialcomputing.io/2023/04/06/why-is-attestation-required-for-confidential-computing/)
- [Remote Attestation Explained](https://edera.dev/stories/remote-attestation-in-confidential-computing-explained)
- [Attestation Mechanisms Demystified](https://arxiv.org/pdf/2206.03780)

### Decentralized Approaches
- [Teamwork Makes TEE Work: Decentralized Attestation](https://arxiv.org/html/2402.08908v1)
- [Phala Network Decentralized Root of Trust](https://phala.com/posts/understanding-the-role-of-rootoftrust-in-tee-and-phalas-decentralized-approach)
- [Flashbots on Decentralized Root of Trust](https://collective.flashbots.net/t/early-thoughts-on-decentralized-root-of-trust/3868)

### Open Source Projects
- [Confidential Computing Consortium Projects](https://confidentialcomputing.io/projects/current-projects/)
- [Confidential Containers (CNCF)](https://www.cncf.io/projects/confidential-containers/)
- [FOSDEM 2026 Confidential Computing Track](https://fosdem.org/2026/schedule/track/confidential-computing/)

---

**Status**: Analysis document
**Last updated**: 2026-01-20
**Next steps**: Community discussion on TEE adoption strategy, ADR for decentralized attestation
