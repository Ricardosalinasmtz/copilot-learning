# Weekly Research Summary — 2026-11-13

## Version Status: UNCHANGED

- **Live (`code --version`):** VS Code **1.144.0** (commit 2c33b50b16c472efe58357d1d2a8a5414bc23fbf, x64)
- **Baseline (`state/progress.json` `currentVersion.vscode`):** 1.144.0
- **Action:** none — no version history entry, no baseline change.
- Note: VS Code **1.145** (released 2026-11-04) is the newest stable upstream. This machine has not been upgraded, so the 1.145 highlights recorded last week in `state/version_history.json` (remote agent host browsing, remote canvases, mixed-harness nested sessions, cron schedules for local automations, agent verbosity, pinned canvases) remain queue candidates for when the machine upgrades.

## Topics Added (3)

Per the queue rule, the queue held 5 undelivered items (≥ 5), so this run adds exactly 3. This week's mix: one current-phase curriculum gap (phase_3 "PR review workflows / team collaboration") plus two items flagged for tracking in the 2026-11-06 research summary. Every source URL was fetched on 2026-11-13 and confirmed to return HTTP 200 with the final URL matching the requested URL.

1. **Delegating Work to Copilot from GitHub (Coding Agent PR Workflows: Issues, @copilot Mentions, Assignments, PR Handoff)**
   - Source: https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-***-agents (fetched, 200, final URL matches)
   - Section: phase_3_advanced · Difficulty: beginner · Priority: 2
   - Rationale: fills the Phase 3 "PR review workflows / team collaboration" curriculum gap — the thinnest remaining area (code *review* was taught 2026-10-02, but the coding-agent side of PR workflows — kicking off agent tasks from issues, @copilot mentions and assignments, tracking and reviewing agent PRs — has not been a lesson). This page is also a fresh anchor: the old `concepts/agents/coding-agent` URL now silently redirects to `concepts/copilot-surfaces/copilot-on-github`, confirming the docs restructure; the new "About agents" concepts page is the current home.

2. **Cloud Automations in the VS Code Agents Window (Scheduled and Event-Triggered Cloud Runs on GitHub Infrastructure)**
   - Source: https://code.visualstudio.com/docs/agents/run/automations (fetched, 200, final URL matches)
   - Section: phase_2_vscode_integration · Difficulty: intermediate · Priority: 2
   - Rationale: 1.144.0 feature (Preview) explicitly flagged "not queued" in last week's summary. Distinct from the 2026-10-16 lesson, which covered automations as defined on GitHub.com (`docs.github.com/.../about-automations`): this lesson is about creating and managing cloud automations directly in the VS Code Agents window — the `chat.automations.cloud.enabled` setting, the Create Automation button, agent-created automations, shared team automations, per-automation tool restrictions and filters, and the default of no allowed tools for new cloud automations.

3. **Agent Host Protocol v1.0 (Open Agent Session Protocol and Remote Agent Hosts)**
   - Source: https://code.visualstudio.com/docs/agents/concepts/agent-host (fetched, 200, final URL matches)
   - Section: phase_4_deep · Difficulty: advanced · Priority: 3
   - Rationale: also flagged as a "later week" candidate in 2026-11-06. AHP v1.0 shipped with 1.144.0 (open, extensible protocol; websocket remote agent hosts via "Agents: Add Remote Agent Host..."; client libraries in six languages). Deliberately scoped to the protocol itself — what it is, remote host registration, and building custom clients/hosts — as a natural Phase 4 deep-dive following the already-taught Agent Harnesses (2026-10-23) and Agent Management (2026-11-05) lessons; the AHP website and multi-client continuity story stay out of scope for a single lesson.

No new topic duplicates the queue or any phase's `completedTopics`.

## Queue Length

- Before: **5** undelivered items
- Added: **3**
- After: **8** undelivered items (verified after write: 5 original entries preserved in order with original addedAt values, 3 new entries appended)

## Queue Order After This Run

1. Copilot CLI /research Command (2026-10-02, phase_4_deep, p3)
2. Canvases: Interactive Collaboration Surfaces (2026-10-23, phase_3_advanced, p2)
3. Bring Your Own OpenAI Key for Voice Mode (2026-10-30, phase_4_deep, p2)
4. Multi-Diff Editor File List (2026-10-30, phase_2_vscode_integration, p2)
5. Sandbox CA Certificate Trust and Per-Session Sandboxing Toggles (2026-10-30, phase_3_advanced, p2)
6. Delegating Work to Copilot from GitHub (Coding Agent PR Workflows) (2026-11-13, phase_3_advanced, p2)
7. Cloud Automations in the VS Code Agents Window (2026-11-13, phase_2_vscode_integration, p2)
8. Agent Host Protocol v1.0 (2026-11-13, phase_4_deep, p3)

## Gaps and Notes

- Current phase remains **phase_3_advanced** (in progress); phase_4_deep is also in progress. The new phase_3 topic is priority 2 per the curriculum-gap rule.
- Docs restructure observed: `docs.github.com/en/copilot/concepts/agents/coding-agent` → 200 redirect to `concepts/copilot-surfaces/copilot-on-github` (the desired page moved). The live "About agents" section is now at `concepts/agent-capabilities/about-agents/`.
- Watch items for next run (2026-11-20): whether the machine upgrades to 1.145 (queue the 1.145 batch if so); candidate deep-dive topics not yet queued: image generation in Copilot chat (Preview), working set Keep/Undo for harness file edits (Experimental), computer use (Experimental, macOS/Windows only — not applicable on this Ubuntu box).
