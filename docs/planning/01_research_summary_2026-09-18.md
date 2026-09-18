# Weekly Research Summary — 2026-09-18

## Version Status

| Component | Version | Status |
|-----------|---------|--------|
| VSCode | 1.136.1 | ✅ No change since 2026-09-11 |
| Copilot | built-in | ✅ No change |
| Subscription | GitHub Copilot Business (Enterprise) | ✅ Unchanged |

No version changes detected. Queue expansion driven by curriculum review and documentation deep-dive.

## Topics Added to Queue

**8 new topics added** (queue grew from 2 to 10 items)

### Phase 2 (VSCode Integration) — 1 topic

| # | Topic | Difficulty | Priority | Source |
|---|-------|-----------|----------|--------|
| 3 | Copilot in Integrated Terminal | Intermediate | 5 (urgent for Phase 2) | [copilot-cli docs](https://docs.github.com/en/copilot/how-tos/copilot-cli) |

**Rationale:** Phase 2 curriculum calls for "Copilot in integrated terminal." Copilot CLI is the native terminal Copilot experience. It covers launching, tool use, session management, and VSCode integration.

### Phase 3 (Advanced Workflows) — 4 topics

| # | Topic | Difficulty | Priority | Source |
|---|-------|-----------|----------|--------|
| 4 | Prompt Engineering for Copilot Chat | Intermediate | 4 | [prompt-engineering docs](https://docs.github.com/en/copilot/concepts/prompting/prompt-engineering) |
| 5 | Custom Instructions (Personal, Repo, Org) | Intermediate | 5 | [response-customization docs](https://docs.github.com/en/copilot/concepts/prompting/response-customization) |
| 6 | Copilot Memory | Advanced | 3 | [copilot-memory docs](https://docs.github.com/en/copilot/concepts/agents/copilot-memory) |
| 7 | Copilot Spaces | Intermediate | 4 | [spaces docs](https://docs.github.com/en/copilot/concepts/context/spaces) |

**Rationale:** 
- Prompt engineering is the bridge between using Copilot and using it well; covers start-general-then-specific, examples, breaking tasks down, avoiding ambiguity, and keeping history relevant.
- Custom instructions are a major Enterprise feature — three tiers (personal, repository, organization) with precedence rules. Fits Phase 3 curriculum perfectly.
- Copilot Memory is a newer feature (public preview) that stores repo-level facts and user preferences with 28-day auto-expiry. Valuable for enterprise context management.
- Copilot Spaces lets users organize context (repos, code, issues, files) into shareable bundles. Supports team knowledge sharing.

### Phase 4 (Deep Customization) — 3 topics

| # | Topic | Difficulty | Priority | Source |
|---|-------|-----------|----------|--------|
| 8 | Agent Hooks | Advanced | 2 (foundational) | [hooks docs](https://docs.github.com/en/copilot/concepts/agents/hooks) |
| 9 | Model Context Protocol (MCP) | Advanced | 2 (foundational) | [mcp docs](https://docs.github.com/en/copilot/concepts/context/mcp) |

**Rationale:**
- Hooks enable custom shell commands at agent lifecycle events (sessionStart, preToolUse, postToolUse, etc.). Critical for enterprise security, audit logging, and compliance automation.
- MCP is an open standard for connecting LLMs to external data sources and tools. GitHub provides an official MCP server with rich toolsets (issues, PRs, code scanning). Foundational for Phase 4 ecosystem topics.

## Queue Summary

| Status | Count |
|--------|-------|
| Existing (from last research) | 2 |
| Newly added | 8 |
| **Total queued** | **10** |

## Curriculum Coverage Map

| Phase | Planned Topics | Covered | In Queue |
|-------|---------------|---------|----------|
| Phase 1 (Core) | 6 items | 5 | 1 (NL to Code) + 1 (Chat Modes) |
| Phase 2 (VSCode) | 6 items | 0 | 1 (Terminal) — plus existing |
| Phase 3 (Advanced) | 6 items | 0 | 4 (Prompt Eng, Custom Instructions, Memory, Spaces) |
| Phase 4 (Deep) | 4 items | 0 | 2 (Hooks, MCP) |

## Notes

- Both existing queue topics (NL to Code, Chat Modes) have broken/404 URLs. Recommend verifying source URLs on next research cycle.
- Copilot CLI documentation is extensive (30+ sub-pages); consider whether to treat it as one lesson or split into 2 (basic usage + automation).
- Enterprise-relevant topics (Custom Instructions, Hooks, MCP) are high-priority given the user's Copilot Business subscription.
- Version history unchanged — no `version_history.json` update needed.
