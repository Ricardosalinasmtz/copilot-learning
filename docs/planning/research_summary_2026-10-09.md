# Weekly Research Summary — 2026-10-09

## Version Status

| Metric | Baseline (state) | Live (`code --version`) | Change |
|--------|------------------|-------------------------|--------|
| VSCode | 1.139.1 | 1.139.1 | ✅ No change |
| Copilot | built-in | built-in | No change |

**VSCode version unchanged** (1.139.1, commit `04c0d99f4fb0d8afe6ce4f0c58e31e183ac3e4b1`). No new version features this week; no entry appended to `state/version_history.json`, and `currentVersion` in `state/progress.json` is left as-is.

## Current Learning Progress

- **Current Phase (state):** Phase 2 (VSCode Integration) — in progress (state also tracks Phase 3 as in progress; delivery interleaves by priority)
- **Lessons delivered:** 16 (last: 2026-10-08, Multi-File Edits)
- **Queue Before This Research:** 10 topics
- **Topics Added This Cycle:** 3 (queue holds ≥ 5, so the 3-topic fill applies)
- **New Queue Size:** 13 topics

## Topics Added to Queue (3)

All three target **Phase 3 curriculum gaps** identified in `00_master_strategy.md`: enterprise features, team collaboration, and Copilot for documentation — the three curriculum items with no completed lesson or queue entry.

| # | Topic | Source | Difficulty | Section | Priority |
|---|-------|--------|------------|---------|----------|
| 1 | **GitHub Copilot Policies for Enterprises and Organizations (AI Controls)** | https://docs.github.com/en/copilot/concepts/enterprise/policies | advanced | phase_3_advanced | 1 |
| 2 | **Custom Agents (Agent Profiles for Shared, Team-Specific Workflows)** | https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-custom-agents | advanced | phase_3_advanced | 1 |
| 3 | **Prompt Files for Generating Documentation (e.g., /document-api for OpenAPI specs)** | https://code.visualstudio.com/docs/agent-customization/prompt-files | intermediate | phase_3_advanced | 2 |

### Rationale

1. **Copilot Policies (AI Controls)** — Fills the Phase 3 "Enterprise features" curriculum item. Covers how policies gate features/agents/models, where they live (enterprise AI controls tab / org settings), enterprise vs org scoping, "let organizations decide" behavior, and least/most-restrictive conflict resolution for multi-license users. Directly relevant: the user's subscription is **Copilot Business (Enterprise)**, so this is the governance surface they will actually encounter. Priority 1 (uncovered curriculum item).

2. **Custom Agents (Agent Profiles)** — Fills the Phase 3 "Team collaboration patterns" curriculum item (with a supporting angle on "enterprise features"). Agent profiles are Markdown + YAML frontmatter defining name/description/prompt/tools, configurable at repository, organization, and enterprise level (`.github/agents/`, org `.github`/`.github-private` repos). One definition, shared across cloud agent, IDEs, Copilot app, and CLI — the natural "teammate" primitive for a team, and distinct from the completed Custom Instructions lesson (per-surface instruction injection vs reusable agent definitions). Priority 1 (uncovered curriculum item).

3. **Prompt Files for Documentation Generation** — Fills the Phase 3 "Copilot for documentation" curriculum item. Prompt files (public preview) are saved reusable prompts in `.github/prompts` invoked as `/slash-commands` in VS Code; the canonical example is `/document-api`, which generates OpenAPI 3.0 specs from endpoint code. Scoped to one lesson: what prompt files are, how to save/parameterize them, and the documentation-generation use case. Source is the VS Code page (GitHub's own docs link there for IDE mechanics). Priority 2 (uncovered curriculum item, narrower scope than the other two).

## Source Verification

Every recorded URL was fetched live today and confirmed to return **HTTP 200 with no redirect** (final URL == requested URL):

- `https://docs.github.com/en/copilot` — docs index (fetched; restructured paths confirmed live)
- `https://docs.github.com/en/copilot/concepts/enterprise/policies` — 200, no redirect
- `https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-custom-agents` — 200, no redirect
- `https://code.visualstudio.com/docs/agent-customization/prompt-files` — 200, no redirect

**Docs restructure note:** VS Code moved its Copilot customization namespace again — `/docs/copilot/customization/*` now redirects to `/docs/agent-customization/*` (verified live). The legacy queue entries still point at `/docs/agents/overview`, which resolves cleanly (no redirect) and is therefore left untouched. The new prompt-files entry records the redirected-to URL, per the no-redirect policy.

## Duplication Check

All 3 candidates verified against the 10 existing queue entries and all completedTopics (Phase 1: 6, Phase 2: 7, Phase 3: 3). No overlaps:
- Policies vs "Copilot Code Review with Agentic Capabilities" (completed) — code review is a feature; policies is the org/enterprise governance layer.
- Custom Agents vs "Custom Instructions" (completed) — agent profiles are reusable, shareable, multi-surface agent definitions; custom instructions are per-surface behavioral guidance.
- Prompt Files vs "Slash Commands Overview" (completed) — that lesson covered built-in chat slash commands; prompt files are user-authored custom prompts, a distinct mechanism.

## Actions Taken

- ✅ `code --version` checked: 1.139.1 — matches baseline, no version change, `state/version_history.json` untouched
- ✅ `state/progress.json` updated: 3 topics appended to queue (10 → 13), `lastUpdated` set; existing entries and their `addedAt` values preserved in order
- ✅ All 3 source URLs fetched and verified (200, no redirect)
- ✅ Research summary written to `docs/planning/research_summary_2026-10-09.md`
- ✅ Ready for git commit

---
*Research performed 2026-10-09 14:30 Europe/Rome. Sources: docs.github.com/en/copilot (index + 3 pages), code.visualstudio.com/docs (2 pages probed, 1 verified), local `code --version`.*
