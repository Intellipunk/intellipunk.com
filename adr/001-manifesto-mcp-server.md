# ADR-001: Manifesto MCP Server Architecture

## Status
Proposed

## Context

The future framework requires implementing Intellipunk as **machine-readable policy + tools**. A key component is a Model Context Protocol (MCP) server that exposes Intellipunk's manifesto, theses, and glossary as queryable interfaces for AI agents and tools.

MCP is an open standard (donated to the Linux Foundation's Agentic AI Foundation) that enables AI systems to integrate with external data sources and tools through a standardized protocol.

## Decision

We will build an MCP server that exposes Intellipunk documentation as structured, queryable resources.

### Server Capabilities

The Intellipunk MCP server provides three primary resource types:

#### 1. Resources (Read-Only Data)
- `manifesto://sections/{section}` - Query specific manifesto sections
- `theses://all` - All 25 theses
- `theses://{id}` - Individual thesis by ID
- `glossary://terms` - All canonical terms
- `glossary://term/{name}` - Specific term definition
- `principles://all` - Core principles summary
- `threats://all` - Four-threat framework
- `threats://{threat}` - Specific threat details

#### 2. Tools (Actions)
- `validate_terminology(text)` - Check text against glossary for consistency
- `check_mechanism(claim)` - Verify claim includes concrete mechanism
- `suggest_thesis(topic)` - Find relevant theses for a topic
- `compliance_check(config)` - Validate configuration against Intellipunk principles

#### 3. Prompts (Templates)
- `review_for_intellipunk` - Template for reviewing content against Intellipunk voice
- `design_guild_node` - Template for designing a compliant guild node
- `energy_audit` - Template for energy receipt validation

### Technical Architecture

```
intellipunk-mcp/
├── src/
│   ├── server.ts           # Main MCP server implementation
│   ├── resources/
│   │   ├── manifesto.ts    # Manifesto resource handlers
│   │   ├── theses.ts       # Theses resource handlers
│   │   └── glossary.ts     # Glossary resource handlers
│   ├── tools/
│   │   ├── validate.ts     # Terminology validation
│   │   ├── mechanism.ts    # Mechanism checking
│   │   └── compliance.ts   # Compliance verification
│   ├── prompts/
│   │   └── templates.ts    # Prompt templates
│   └── parsers/
│       └── markdown.ts     # Parse markdown docs to structured data
├── data/
│   └── cache/              # Cached parsed documentation
├── package.json
├── tsconfig.json
└── README.md
```

### Data Model

```typescript
interface Thesis {
  id: string;
  title: string;
  statement: string;
  mechanism?: string;
  evidence?: string;
  tags: string[];
}

interface GlossaryTerm {
  term: string;
  definition: string;
  category: 'core' | 'antagonism' | 'infrastructure' | 'governance' | 'operational';
  relatedTerms: string[];
  examples?: string[];
}

interface ManifestoSection {
  id: string;
  title: string;
  content: string;
  subsections?: ManifestoSection[];
}

interface Threat {
  name: string;
  mechanism: string;
  response: string[];
  status: string; // Coverage in current docs
}
```

### Protocol Implementation

Uses MCP specification (https://modelcontextprotocol.io/specification/2025-06-18):

- **Transport**: stdio for local processes, HTTP + SSE for remote
- **Message Format**: JSON-RPC 2.0
- **SDKs**: TypeScript (primary), Python (future)

### Example Interactions

#### Query a thesis
```json
{
  "method": "resources/read",
  "params": {
    "uri": "theses://4"
  }
}
```

Response:
```json
{
  "contents": [{
    "uri": "theses://4",
    "mimeType": "application/json",
    "text": "{\"id\":\"4\",\"statement\":\"The 'punk' is refusal: against closed systems, coerced subscriptions, model feudalism, and trillion-dollar permissioned intelligence.\",\"tags\":[\"position\",\"antagonism\"]}"
  }]
}
```

#### Validate terminology
```json
{
  "method": "tools/call",
  "params": {
    "name": "validate_terminology",
    "arguments": {
      "text": "We need to decentralize everything and create a utopian future"
    }
  }
}
```

Response:
```json
{
  "content": [{
    "type": "text",
    "text": "Non-canonical terms detected:\n- 'decentralize everything' (avoid - Intellipunk focuses on specific threats)\n- 'utopian' (avoid - requires concrete mechanisms)\nSuggested: 'We address computational feudalism through local autonomy and verifiable compute'"
  }]
}
```

## Implementation Phases

### Phase 1: Core Resources (MVP)
- Parse existing markdown docs (manifesto, theses, glossary)
- Expose via MCP resources
- Basic stdio transport
- Documentation and examples

### Phase 2: Validation Tools
- Terminology validator
- Mechanism checker
- Thesis suggester

### Phase 3: Compliance Tools
- Full compliance checker
- Configuration validator
- Integration with compliance pack endpoint

### Phase 4: Advanced Features
- HTTP transport for remote access
- Versioning support (track manifesto changes)
- Proposal system (governance via PRs)
- Real-time updates from repository

## Consequences

### Positive
- **Machine-readable policy**: AI agents can query Intellipunk principles programmatically
- **Consistency enforcement**: Tools can validate content against canonical definitions
- **Executable governance**: Changes to manifesto become queryable versions
- **Portability**: Standard MCP protocol enables wide tool integration
- **Forkability**: Anyone can run their own MCP server with modified principles

### Negative
- **Maintenance burden**: Docs and MCP server must stay synchronized
- **Complexity**: Adds infrastructure beyond static documentation
- **Initial adoption**: Requires MCP-compatible clients to realize value

### Mitigations
- Automated parsing keeps server synchronized with markdown docs
- Start with minimal viable server (Phase 1)
- Provide clear examples for common MCP clients
- Document fallback to static llms.txt for non-MCP tools

## Alternatives Considered

### 1. Static API (REST)
- Simpler to implement
- But: Not standardized, requires custom client code
- MCP provides standard protocol and existing clients

### 2. GraphQL API
- Flexible querying
- But: Overhead for simple document access
- MCP resources sufficient for current needs

### 3. Documentation Only
- Simplest approach
- But: Doesn't enable machine-readable policy goal
- Misses opportunity for tool integration

## References

- [Model Context Protocol Specification](https://modelcontextprotocol.io/specification/2025-06-18)
- [Anthropic MCP Introduction](https://www.anthropic.com/news/model-context-protocol)
- [llms.txt standard](https://llmstxt.org/)
- Future Framework (Dynalist notes)

## Timeline

- **Q1 2026**: ADR approval, Phase 1 implementation
- **Q2 2026**: Phase 2 (validation tools)
- **Q3 2026**: Phase 3 (compliance integration)
- **Q4 2026**: Phase 4 (advanced features)

## Success Metrics

- MCP server successfully queries all manifesto sections
- Terminology validator catches non-canonical usage
- At least one external tool integrates with server
- Community forks server for custom principles
- Compliance pack endpoint references MCP resources

---

**Decision makers**: [To be determined]
**Date**: 2026-01-20
**Approved**: [Pending]
