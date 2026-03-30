# Use Architecture Decision Records

- Status: accepted
- Date: 2026-03-30

## Context and Problem Statement

We need a way to document and communicate significant architectural decisions made throughout the lifecycle of this project.

## Decision Drivers

- Decisions should be documented close to the code
- Historical context for past decisions should be easy to find
- The format should be lightweight and developer-friendly

## Considered Options

- Architecture Decision Records (ADRs) in Markdown
- Wiki-based documentation
- No formal documentation

## Decision Outcome

Chosen option: "Architecture Decision Records (ADRs) in Markdown", because it keeps decision records versioned alongside the code, is easy to write and review in pull requests, and integrates with Backstage via the ADR plugin.

### Consequences

- All significant architectural decisions will be documented as numbered ADR files in `docs/adr/`
- ADRs follow the MADR (Markdown Architectural Decision Records) format
- ADRs are viewable in Backstage via the ADR plugin
