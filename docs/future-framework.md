# The Future Framework

## Overview

Intellipunk implements **machine-readable policy + tools** that transform philosophical principles into executable infrastructure.

This framework makes Intellipunk inspectable, forkable, and verifiable at the protocol level.

## Core Idea

Move from documents to interfaces. From "trust us" to "verify this". From manifestos to mechanisms.

## Components

### 1. llms.txt - LLM-Optimized Summary

**File**: `/llms.txt`

**Purpose**: Provide AI agents with a clear, structured introduction to Intellipunk when they encounter the project.

**Format**: Markdown document following [llms.txt standard](https://llmstxt.org/)

**Contains**:
- Core position and four-threat framework
- Key principles and canonical terms
- Links to primary resources
- Usage constraints and voice guidelines
- Machine-readable interface pointers

**Use cases**:
- AI assistants researching Intellipunk
- AI code assistants working in Intellipunk repos
- Automated documentation tools
- Web crawlers optimizing for AI search

---

### 2. Manifesto MCP Server - Queryable Principles

**Spec**: `/adr/001-manifesto-mcp-server.md`

**Purpose**: Expose Intellipunk documentation as structured, queryable resources using the Model Context Protocol.

**Architecture**:
```
Resources (read-only):
  - manifesto://sections/{section}
  - theses://{id}
  - glossary://term/{name}
  - threats://{threat}

Tools (actions):
  - validate_terminology(text)
  - check_mechanism(claim)
  - suggest_thesis(topic)
  - compliance_check(config)

Prompts (templates):
  - review_for_intellipunk
  - design_guild_node
  - energy_audit
```

**Protocol**: MCP 1.0 (JSON-RPC 2.0 over stdio/HTTP)

**Use cases**:
- AI agents query Intellipunk principles during development
- Tools validate content against canonical glossary
- IDEs provide Intellipunk-aware autocomplete
- CI/CD pipelines check compliance before merge

**Status**: Proposed (ADR-001)

---

### 3. Compliance Pack Endpoint - Verifiable Receipts

**Spec**: `/adr/002-compliance-pack-endpoint.md`

**Purpose**: Provide machine-readable, cryptographically verifiable evidence that a guild node or service adheres to Intellipunk principles.

**Endpoint**: `GET /.well-known/intellipunk-compliance`

**Schema** provides:
- **Provenance**: Who operates this? What hardware? What software?
- **Licensing**: What models? What data policies? What usage terms?
- **Safety**: What constraints? What isolation? What telemetry?
- **Energy**: What power sources? What consumption? What receipts?
- **Verifiable Compute**: What attestation? What audit logs?
- **Governance**: How are decisions made? How to dispute?
- **Interoperability**: What APIs? What export formats?
- **Compliance Assertions**: Claims mapped to evidence for each threat

**Validation Levels**:
1. Self-Attested
2. Cryptographically Signed
3. Hardware-Attested (TPM/TEE)
4. Continuously Audited

**Use cases**:
- Users compare guild nodes before choosing
- Federation networks verify peer compliance
- Auditors validate renewable energy claims
- Marketplaces filter by Intellipunk principles

**Status**: Proposed (ADR-002)

---

## How They Work Together

```
User/Agent Query Flow:

1. Discover Intellipunk via llms.txt
   → Understand: "What is this? What principles?"

2. Query MCP server for details
   → "What does 'computational feudalism' mean?"
   → "Which thesis addresses energy concerns?"
   → "Does this text use canonical terminology?"

3. Validate guild node via compliance pack
   → "Does this node provide verifiable energy receipts?"
   → "Is the hardware community-owned?"
   → "Can I export my data?"

4. Make informed decision
   → "This node aligns with my Intellipunk priorities"
   → "I can verify their renewable energy claims"
   → "I have a clear exit path"
```

```
Guild Node Operator Flow:

1. Read Intellipunk docs (manifesto, theses, glossary)

2. Design node architecture
   → Use MCP server to validate design decisions
   → Query: "Does this comply with anti-lock-in principles?"

3. Implement infrastructure
   → Self-host models (avoid computational feudalism)
   → Deploy edge inference (avoid extractive clouds)
   → Install energy meters (avoid greenwashing)
   → Enable audit logs (avoid epistemic gatekeeping)

4. Generate compliance pack
   → Document hardware, software, governance
   → Provide energy receipts
   → Sign with operator key
   → Serve at /.well-known/intellipunk-compliance

5. Publish and operate
   → Users discover via compliance pack
   → MCP server helps users validate claims
   → Community monitors via audit endpoints
```

## Why This Matters

### From Documents to Infrastructure

Traditional manifestos are **static text**:
- Humans read and interpret
- Compliance is subjective
- Verification requires trust
- No programmatic validation

Intellipunk's future framework is **executable policy**:
- AIs query directly via MCP
- Compliance is measurable via receipts
- Verification uses cryptography
- Automated validation possible

### Enables New Capabilities

1. **AI-Native Governance**
   - Agents can query "What would Intellipunk do?"
   - Tools enforce principles at development time
   - CI/CD prevents anti-pattern merges

2. **Verifiable Claims**
   - "We're renewable-first" → Show energy receipts
   - "We're community-owned" → Show governance docs
   - "We're anti-lock-in" → Show export API

3. **Comparison Shopping**
   - Compare 10 guild nodes in 10 seconds
   - Filter by compliance level
   - Sort by renewable percentage

4. **Fork-First Ecosystem**
   - Fork MCP server with custom principles
   - Fork compliance pack schema for new use cases
   - Fork guild node implementations

5. **Transparent Accountability**
   - Public audit logs
   - Cryptographic receipts
   - Community verification

## Implementation Status

### Completed
- ✅ llms.txt specification written
- ✅ ADR-001 (MCP Server) proposed
- ✅ ADR-002 (Compliance Pack) proposed

### Next Steps
1. **Community review** of ADRs (Q1 2026)
2. **MCP server MVP** - Parse docs, expose resources (Q1 2026)
3. **Compliance pack schema v1.0** finalized (Q1 2026)
4. **Reference implementations** in TypeScript/Go (Q2 2026)
5. **First guild node** publishes compliance pack (Q2 2026)
6. **Validator tools** for compliance pack checking (Q2 2026)
7. **MCP + Compliance integration** (Q3 2026)

## Design Principles

This framework itself follows Intellipunk principles:

- **Portability**: Uses open standards (llms.txt, MCP, JSON Schema)
- **Receipts over Trust**: Compliance pack provides cryptographic evidence
- **Forkability**: All components are open source, schema is versioned
- **Resilience**: No central registry required, distributed verification

## Open Questions

1. **Who governs schema evolution?**
   - Proposal: Community governance via GitHub proposals

2. **How to prevent compliance theater?**
   - Proposal: Multiple validation levels, community auditing, reputation

3. **Should compliance be required?**
   - Answer: No. Intellipunk doesn't gatekeep. Compliance pack enables *informed choice*.

4. **What about privacy vs. transparency?**
   - Proposal: Tiered disclosure (summary vs. detailed), redaction for opsec

## Success Criteria

The future framework succeeds when:

- **10+ guild nodes** publish compliance packs
- **AI agents** routinely query MCP server for Intellipunk guidance
- **Users** make node selections based on verified compliance
- **Other projects** fork schemas for their own principles
- **Compliance theater** is detectable via audit endpoints
- **Energy claims** are cryptographically verifiable

## Philosophical Alignment

This framework embodies Intellipunk's core commitments:

| Principle | Implementation |
|-----------|----------------|
| Inspectable | Compliance pack reveals all operational details |
| Forkable | Open schemas, versioned specs, anyone can modify |
| Optional | No central enforcement, user choice enabled |
| Verifiable | Cryptographic receipts, hardware attestation |
| Local-first | Well-known URIs, no central registry required |
| Energy realism | Receipts from signed energy meters |
| Anti-lock-in | Standard protocols, export formats specified |

## For Developers

**To implement llms.txt**:
- Copy `/llms.txt` to your Intellipunk project root
- Customize for your specific implementation
- Update resource links

**To integrate MCP server**:
- Wait for Phase 1 release (Q1 2026)
- Install via npm/pip: `intellipunk-mcp-server`
- Configure with path to your docs
- Connect from any MCP-compatible client

**To publish compliance pack**:
- Generate JSON following schema in ADR-002
- Sign with your node operator key
- Serve at `/.well-known/intellipunk-compliance`
- Optionally register with discovery services

**To validate compliance**:
- Fetch compliance pack from node
- Verify JSON schema
- Check cryptographic signatures
- Query audit endpoints
- Assign trust level

## Resources

- **Standards**:
  - [llms.txt specification](https://llmstxt.org/)
  - [Model Context Protocol](https://modelcontextprotocol.io/specification/2025-06-18)
  - [RFC 8615: Well-Known URIs](https://tools.ietf.org/html/rfc8615)

- **Intellipunk Documentation**:
  - [Manifesto](manifesto.md)
  - [Theses](theses.md)
  - [Glossary](glossary.md)
  - [AGENTS.md](../AGENTS.md)

- **ADRs**:
  - [ADR-001: Manifesto MCP Server](../adr/001-manifesto-mcp-server.md)
  - [ADR-002: Compliance Pack Endpoint](../adr/002-compliance-pack-endpoint.md)

---

**This document is a living specification. Propose changes via pull request.**

**Last updated**: 2026-01-20
**Status**: Active development
