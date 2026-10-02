# Weekly Research Summary — 2026-10-02

## Version Status

| Metric | Previous | Current | Change |
|--------|----------|---------|--------|
| VSCode | 1.136.1 | **1.139.1** | ✅ Version bump detected |
| Copilot | built-in | built-in | No change |

**VSCode 1.136.1 → 1.139.1** — Multiple new features surfaced in this update, including GitHub Agentic Workflows, dynamic workflows, Copilot CLI `/fleet` parallel execution, `/research` command, BYOK support, code review agentic capabilities, and CLI extensions.

**Version history updated:** `state/version_history.json`

## Current Learning Progress

- **Current Phase:** Phase 3 (Advanced Workflows) — *in progress*
- **Topics Completed:** 8 across all phases (Phase 1: 6, Phase 2: 3, Phase 3: 2)
- **Queue Before This Research:** 2 topics
- **Topics Added This Cycle:** 8 topics
- **New Queue Size:** 10 topics

## Topics Added to Queue (8)

### Priority 1 — New Version Features (Critical)

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 1 | **Copilot Code Review with Agentic Capabilities** | [docs](https://docs.github.com/en/copilot/concepts/agents/code-review) | Advanced | 3 | Phase 3 |

**Rationale:** Full project context gathering, cloud agent handoff for automated fix PRs, Lite vs Balanced effort levels, agentic capabilities with GitHub Actions runners. Enterprise policy controls for unlicensed users. **New version feature — highest priority.**

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 2 | **GitHub Agentic Workflows in GitHub Actions** | [docs](https://docs.github.com/en/copilot/concepts/agents/about-github-agentic-workflows) | Advanced | 3 | Phase 3 |

**Rationale:** AI-powered repository automations in markdown (not YAML), natural language instructions with frontmatter guardrails, safe-outputs pattern, multi-engine support (Copilot, Claude, Codex, Gemini), AIC billing. Public preview. **New version feature — highest priority.**

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 3 | **Dynamic Workflows in Copilot** | [docs](https://docs.github.com/en/copilot/concepts/agents/dynamic-workflows) | Advanced | 3 | Phase 3 |

**Rationale:** Reusable multi-step processes defined in code, combining agents/tools/APIs/parallel execution, checkpointing for human review, extension-based delivery. Contrasts with autopilot and /fleet. Public preview. **New version feature — highest priority.**

### Priority 2 — CLI Deep Dive & Enterprise

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 4 | **Copilot CLI Deep Dive** | [docs](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli) | Intermediate | 3 | Phase 3 |

**Rationale:** Interactive vs programmatic modes, plan mode (Shift+Tab), sandboxing (local/cloud), computer use on macOS/Windows, remote control from GitHub.com. Foundation for understanding CLI capabilities.

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 5 | **Copilot CLI /fleet Command** | [docs](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet) | Advanced | 2 | Phase 4 |

**Rationale:** Parallel subagent execution for complex tasks, context isolation per subagent, custom agent specialization, model selection per subtask, credit implications. Contrasts with dynamic workflows (defined process vs copilot-decided).

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 6 | **Bring Your Own Key (BYOK)** | [docs](https://docs.github.com/en/copilot/concepts/models/bring-your-own-key) | Advanced | 3 | Phase 4 |

**Rationale:** Local BYOK (VS Code, JetBrains, Xcode, CLI, SDK) for air-gapped/self-hosted, Enterprise BYOK (server-side custom models for all users), policy controls. Cost savings and compliance. **New version feature.**

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 7 | **Copilot CLI Extensions** | [docs](https://docs/github.com/en/copilot/concepts/agents/copilot-cli/about-cli-extensions) | Advanced | 3 | Phase 4 |

**Rationale:** Custom tools and slash commands via Node.js, project/user/plugin scopes, /extensions mode (Load&Augment/Load Only/Disabled), Copilot-managed scaffolding. Experimental feature. Complements existing "Agent Hooks" topic.

### Priority 3 — Utility

| # | Topic | Source | Difficulty | Est. Lessons | Phase |
|---|-------|--------|------------|--------------|-------|
| 8 | **Copilot CLI /research Command** | [docs](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/research) | Intermediate | 2 | Phase 4 |

**Rationale:** Deep research agent with citations, report output as Markdown, gist sharing, cross-repository and web search, query-type adaptation. Practical for the user's background (ECG signal processing, AI development research).

## Phase Mapping Summary

| Phase | Status | Topics Completed | Queue (New) |
|-------|--------|-----------------|-------------|
| Phase 1 — Core | Completed | 6 | — |
| Phase 2 — VSCode Integration | Completed | 3 | — |
| Phase 3 — Advanced | In Progress | 2 | **3** (code review, agentic workflows, dynamic workflows) |
| Phase 4 — Deep | Locked | 0 | **5** (CLI deep dive, /fleet, /research, BYOK, CLI extensions) |

## Key Observations

1. **Version bump is significant** — VSCode 1.136.1 → 1.139.1 introduces 3 major new concepts (Agentic Workflows, Dynamic Workflows, Code Review agentic capabilities) that directly map to Phase 3 curriculum.

2. **Phase 3 curriculum gap filled** — The existing Phase 3 topics (Custom Instructions, Copilot Spaces) were well-chosen. The 3 new Priority 1 topics complete the "Advanced Workflows" picture by covering automated review, repo automation, and reusable workflows.

3. **Phase 4 becomes rich** — 5 deep topics ready to unlock when Phase 3 completes. CLI ecosystem is a major theme: CLI basics → /fleet → /research → Extensions → BYOK.

4. **No queue duplicates** — All 8 topics verified against existing queue (Agent Hooks, MCP) and completed topics. No overlaps.

## Actions Taken

- ✅ VSCode version updated: `1.136.1` → `1.139.1`
- ✅ `state/version_history.json` appended with version change entry
- ✅ `state/progress.json` updated: currentVersion, lastUpdated, queue (8 new topics)
- ✅ Research summary written to `docs/planning/01_research_summary_2026-10-02.md`
- ✅ Ready for git commit

---
*Research performed 2026-10-02 12:30 Europe/Rome. Sources: docs.github.com/en/copilot (8 pages fetched), local `code --version`.*
