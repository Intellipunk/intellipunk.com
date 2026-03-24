# CONTRIBUTING.md — Working with Intellipunk

## Project Context
Intellipunk is a speculative framework exploring decentralized intelligence in a world where compute, energy, latency, and attention are scarce and priced. The project challenges AI empires and centralized compute monopolies through concrete mechanisms rather than aspirational rhetoric.

## Primary Directive
When working on this project, prioritize:
1. **Clarity over inspiration** — mechanisms beat adjectives
2. **Falsifiability** — claims must be testable or labeled as speculative
3. **Canon consistency** — check against existing docs before proposing changes

## Canon Hierarchy
Reference these in order of authority:
1. `docs/manifesto.md` — core worldview and principles
2. `docs/theses.md` — specific propositions
3. `docs/glossary.md` — term definitions (use these exactly)
4. `adr/*` — architectural decisions
5. `AGENTS.md` — execution contract and style guide

## Key Constraints
### What Intellipunk IS
- Decentralized intelligence infrastructure
- Energy realism and grid constraints
- Anti-monopoly compute architectures
- Scarcity-aware AI systems

### What Intellipunk IS NOT
- Generic solarpunk/hopepunk aesthetics
- Utopian tech optimism without mechanisms
- Moralistic or inspirational content
- Vague "better future" rhetoric

## Terminology (must use consistently)
- **Intellipunk** — the framework/movement, not "intellectualism punk"
- **Compute monopolies** — not "Big Tech" or "AI companies"
- **Grid capture** — when centralized actors control energy access
- **Extractive clouds** — centralized compute that extracts value from edges

Check `docs/glossary.md` for full definitions before introducing new terms.

## Writing Style
```
✓ Short sentences. Concrete nouns. Active verbs.
✓ Mechanisms: "proof-of-work creates energy markets"
✗ Adjectives: "reimagine", "empower", "transform"
✗ Vague futures: "a world where..."
```

## Workflow Pattern
1. **Read canon first** — especially manifesto, theses, glossary
2. **Identify conflicts** — if your idea contradicts canon, flag it explicitly
3. **Propose patch** — show how to reconcile or update canon
4. **Smallest change** — don't restructure unless necessary
5. **Draft with structure** — use existing heading patterns

## Common Tasks

### Adding Content
- **Manifesto sections**: 3–7 bullet theses per heading
- **Theses**: single falsifiable claim + 2–4 sentence explanation
- **Glossary entries**: term + 1-sentence definition + optional 2–3 sentence expansion

### Editing Content
- Preserve file structure and heading hierarchy
- Mark speculative claims explicitly
- Add citations for factual claims about real entities
- Check for terminology drift against glossary

### Quality Checklist
Before finalizing any change:
- [ ] Does each key claim imply a concrete mechanism?
- [ ] Are all terms used consistently with glossary?
- [ ] Is tone concrete and non-moralistic?
- [ ] Are factual claims cited or labeled as working theories?
- [ ] Does this avoid generic punk aesthetics without substance?

## Red Flags
If you see yourself about to:
- Use "reimagine", "empower", "unlock", "transform"
- Make utopian claims without mechanisms
- Introduce new terminology without defining it
- Add inspirational or motivational language
- Suggest major restructuring without clear need

**Stop and reconsider the approach.**

## Output Formats

### Manifesto Section
```markdown
## [Heading]
- Mechanism-based claim
- Concrete consequence
- Testable prediction
- (3–7 bullets total)
```

### Thesis Entry
```markdown
### [Thesis Title]
Single falsifiable claim. Mechanism explanation in 2–4 sentences.
Evidence or label as speculative.
```

### Glossary Entry
```markdown
**Term**: One-sentence definition.

Optional: 2–3 sentences expanding on usage, context, or implications.
```

## Working with the Repository
- Main branch: `main`
- Current worktree: `romantic-varahamihira`
- Preserve existing directory structure
- Follow git commit conventions in the project
- Don't create new top-level files without discussion

## When Uncertain
1. Check canon documents
2. Reference AGENTS.md protocol
3. Ask for clarification rather than inventing
4. Propose options rather than making assumptions

---

*This document guides contributors and AI assistants working on Intellipunk. For the execution contract, see AGENTS.md.*
