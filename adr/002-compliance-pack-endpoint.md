# ADR-002: Compliance Pack Endpoint Specification

## Status
Proposed

## Context

Intellipunk's future framework requires moving from "trust-me" claims to **verifiable artifacts**. The compliance pack endpoint provides machine-readable evidence that a guild node, service, or system adheres to Intellipunk principles.

Core principle: **Receipts over trust**.

This endpoint answers:
- What model/code actually ran?
- On what inputs?
- Under what constraints?
- With what energy profile?
- Under which governance rules?

## Decision

We will define a standard compliance pack endpoint that guild nodes and Intellipunk-compatible services can implement to provide cryptographic and auditable proof of their operation.

### Endpoint Structure

```
GET /.well-known/intellipunk-compliance
```

Returns a JSON document conforming to the Intellipunk Compliance Pack schema.

### Compliance Pack Schema

```json
{
  "version": "1.0.0",
  "node_id": "guild-node-sf-coop-001",
  "timestamp": "2026-01-20T15:30:00Z",
  "provenance": {
    "operator": {
      "name": "San Francisco AI Co-op",
      "governance_model": "cooperative",
      "legal_entity": "California Cooperative Corporation",
      "contact": "ops@sfaicoop.org"
    },
    "hardware": {
      "location": "San Francisco, CA, USA",
      "compute_spec": {
        "gpus": [
          {
            "model": "NVIDIA H100",
            "count": 4,
            "memory_gb": 80
          }
        ],
        "cpu": "AMD EPYC 9654",
        "ram_gb": 512
      },
      "ownership": "community-owned",
      "purchase_date": "2025-06-15"
    },
    "software": {
      "runtime": "vllm 0.4.2",
      "os": "Ubuntu 24.04 LTS",
      "container_runtime": "Docker 25.0.1"
    }
  },
  "licensing": {
    "models": [
      {
        "name": "llama-3.1-70b",
        "license": "Llama 3.1 Community License",
        "weights_source": "https://huggingface.co/meta-llama/Llama-3.1-70B",
        "checksum": "sha256:abc123...",
        "modification_policy": "forkable_with_attribution"
      }
    ],
    "data_policy": {
      "collection": "opt-in_only",
      "retention": "30_days_max",
      "minimization": true,
      "export_available": true,
      "deletion_on_request": true
    },
    "usage_terms": {
      "api_license": "Apache 2.0",
      "rate_limits": {
        "free_tier": "1000_tokens_per_day",
        "paid_tier": "pay_per_token"
      },
      "exit_policy": "full_state_export_available"
    }
  },
  "safety_constraints": {
    "content_filtering": {
      "enabled": true,
      "policy": "community_standards_v1.2",
      "appeals_process": "governance_board_review"
    },
    "isolation": {
      "multi_tenancy": false,
      "workload_isolation": "per_request_container"
    },
    "telemetry": {
      "logging_level": "aggregate_only",
      "pii_collection": false,
      "metrics_endpoint": "https://metrics.sfaicoop.org/public"
    }
  },
  "energy_profile": {
    "power_source": {
      "grid_mix": {
        "solar": 0.45,
        "wind": 0.25,
        "hydro": 0.15,
        "natural_gas": 0.10,
        "other": 0.05
      },
      "onsite_generation": {
        "solar_kw": 50,
        "battery_kwh": 100
      },
      "carbon_intensity_g_co2_per_kwh": 180
    },
    "consumption": {
      "idle_watts": 800,
      "peak_watts": 3200,
      "average_watts_last_30d": 1500
    },
    "receipts": {
      "energy_meter_id": "PGE-SM-94103-7821",
      "attestation_method": "cryptographic_meter_signature",
      "last_reading": {
        "timestamp": "2026-01-20T15:00:00Z",
        "kwh_consumed": 36.2,
        "signature": "0x..."
      }
    },
    "heat_management": {
      "waste_heat_recovery": true,
      "use_case": "building_hvac"
    }
  },
  "verifiable_compute": {
    "attestation_method": "tpm_2.0",
    "runtime_evidence": {
      "type": "trusted_execution_environment",
      "technology": "AMD SEV-SNP",
      "measurement": "sha256:def456..."
    },
    "audit_log_endpoint": "https://audit.sfaicoop.org/logs",
    "reproducibility": {
      "container_image": "docker://sfaicoop/llama-serve:v2.1.0",
      "config_hash": "sha256:789abc...",
      "deterministic_inference": false  // Note: LLMs are stochastic
    }
  },
  "governance": {
    "decision_making": "one_member_one_vote",
    "policy_change_process": "https://sfaicoop.org/governance/amendment",
    "dispute_resolution": "binding_arbitration",
    "transparency_reports": "https://sfaicoop.org/transparency/quarterly",
    "community_forum": "https://forum.sfaicoop.org"
  },
  "interoperability": {
    "api_standards": ["openai_compatible", "mcp_1.0"],
    "export_formats": ["json", "parquet", "sqlite"],
    "federation_protocol": "activitypub_extensions",
    "identity_portability": "did_based"
  },
  "compliance_assertions": {
    "computational_feudalism": {
      "claim": "self_hosted_with_exit_path",
      "evidence": [
        "community_owned_hardware",
        "full_state_export_api",
        "open_source_runtime"
      ]
    },
    "extractive_clouds": {
      "claim": "local_first_with_minimal_data",
      "evidence": [
        "edge_inference_available",
        "opt_in_data_collection",
        "30_day_retention_max"
      ]
    },
    "epistemic_gatekeeping": {
      "claim": "transparent_and_auditable",
      "evidence": [
        "public_audit_logs",
        "content_policy_published",
        "appeals_process_documented"
      ]
    },
    "energy_chokepoints": {
      "claim": "renewable_first_with_receipts",
      "evidence": [
        "70_percent_renewable_mix",
        "onsite_solar_storage",
        "cryptographic_energy_receipts"
      ]
    }
  },
  "signatures": {
    "operator_signature": {
      "pubkey": "0x...",
      "signature": "0x...",
      "algorithm": "ed25519"
    },
    "hardware_attestation": {
      "tpm_quote": "0x...",
      "pcr_values": {...}
    }
  }
}
```

### Validation Levels

Compliance packs can be validated at different trust levels:

#### Level 1: Self-Attested
- Operator provides JSON document
- No cryptographic verification
- Suitable for: Community nodes, low-stakes usage

#### Level 2: Cryptographically Signed
- Operator signs document with known key
- Verifies operator identity
- Suitable for: Federated networks, public services

#### Level 3: Hardware-Attested
- TPM/TEE provides runtime measurements
- Energy meters provide signed readings
- Suitable for: High-stakes applications, financial settlements

#### Level 4: Continuously Audited
- Third-party auditor verifies claims
- Real-time monitoring endpoints
- Suitable for: Critical infrastructure, regulated environments

### Discovery

Compliance packs are discoverable via:

1. **Well-known URI**: `GET /.well-known/intellipunk-compliance`
2. **DNS TXT record**: `_intellipunk-compliance.example.org`
3. **MCP resource**: `intellipunk-mcp://compliance/{node_id}`
4. **Registry**: Central registry of compliant nodes (optional, not required)

### Versioning

Schema follows semantic versioning:
- **Major**: Breaking changes to required fields
- **Minor**: New optional fields, new constraint types
- **Patch**: Clarifications, documentation updates

Current version: `1.0.0`

## Implementation Guide

### For Guild Nodes

1. Generate compliance pack JSON
2. Sign with node operator key
3. Serve at `/.well-known/intellipunk-compliance`
4. Update on significant changes (governance, energy mix, etc.)
5. Optionally register with discovery services

### For Validators

1. Fetch compliance pack from well-known URI
2. Verify JSON schema conformance
3. Check cryptographic signatures
4. Validate assertions against evidence
5. Query audit endpoints for real-time verification
6. Assign trust level (1-4)

### For Users

1. Query compliance pack before using service
2. Verify alignment with personal Intellipunk priorities
3. Compare multiple nodes for best fit
4. Monitor for changes over time
5. Report violations to community/governance

## Consequences

### Positive
- **Verifiable claims**: Moves from "trust us" to "verify this"
- **Comparison**: Users can compare nodes objectively
- **Accountability**: Public commitments are harder to break
- **Forkability**: Clear documentation of how a node operates
- **Energy transparency**: Real consumption data, not marketing

### Negative
- **Disclosure burden**: Operators must reveal operational details
- **Privacy concerns**: Some details (location, capacity) reveal strategic info
- **Gaming risk**: Operators might optimize for compliance metrics over substance
- **Maintenance**: Keeping compliance pack current requires automation

### Mitigations
- Allow granularity levels (detailed vs. summary)
- Support redaction for sensitive operational security
- Community review of schemas to prevent metric gaming
- Provide tools to auto-generate compliance packs from infrastructure configs

## Relationship to Other Components

### MCP Server Integration
The Manifesto MCP Server (ADR-001) can:
- Validate compliance packs against Intellipunk principles
- Suggest improvements for partial compliance
- Compare multiple nodes
- Generate human-readable reports

### llms.txt Integration
llms.txt points to compliance pack:
```markdown
## Compliance
- Compliance pack: /.well-known/intellipunk-compliance
- Validation level: Level 3 (Hardware-Attested)
```

## Alternatives Considered

### 1. Self-Reported Survey
- Simple Google Form or checklist
- But: Not machine-readable, not verifiable, trust-based

### 2. Blockchain-Based Registry
- Immutable record of compliance
- But: Adds complexity, energy cost, centralization risk
- Compliance pack can optionally anchor to blockchain

### 3. Manual Audits Only
- Trusted third parties review nodes
- But: Doesn't scale, expensive, introduces gatekeepers
- Compliance pack enables automated pre-screening

## Open Questions

1. **Who defines acceptable thresholds?** (e.g., "renewable-first" = >50% or >70%?)
   - Proposal: Community governance sets recommended ranges, not hard requirements

2. **How to handle trade secrets?** (e.g., proprietary efficiency improvements)
   - Proposal: Allow "attestation without disclosure" via zero-knowledge proofs (future)

3. **What happens on non-compliance?**
   - Proposal: No central enforcement; community reputation and federation policies

4. **Should there be a compliance registry?**
   - Proposal: Optional registries can exist, but discovery via well-known URI is primary

## Timeline

- **Q1 2026**: Schema v1.0 finalized
- **Q2 2026**: Reference implementation (Go/Rust)
- **Q2 2026**: Validator tools and examples
- **Q3 2026**: Integration with MCP server
- **Q4 2026**: Community adoption, schema v1.1 based on feedback

## Success Metrics

- 10+ guild nodes publish compliance packs
- Validator tools used by at least 100 users
- Schema adopted by non-Intellipunk projects (shows generality)
- Energy receipts enable verifiable renewable claims
- Users report compliance pack influenced node selection

## References

- [RFC 8615: Well-Known URIs](https://tools.ietf.org/html/rfc8615)
- [Trusted Execution Environments](https://en.wikipedia.org/wiki/Trusted_execution_environment)
- [TPM 2.0 Specification](https://trustedcomputinggroup.org/resource/tpm-library-specification/)
- Intellipunk Theses (docs/theses.md)
- Future Framework (Dynalist notes)

---

**Decision makers**: [To be determined]
**Date**: 2026-01-20
**Approved**: [Pending]
