# Weekly Research Summary — 2026-11-06

## Version Status

**Version change detected: VS Code 1.143.1 → 1.144.0** (stable released 2026-10-28).

- Live version from `code --version`: **1.144.0** (commit 2c33b50b16c472efe58357d1d2a8a5414bc23fbf)
- Previous baseline in `state/progress.json`: 1.143.1
- Actions taken: appended a 1.144.0 entry to `state/version_history.json` and updated `currentVersion.vscode` to 1.144.0 so the next run compares against the new baseline.
- Note: VS Code 1.145 (released 2026-11-04) is the newest stable, but this machine is on 1.144.0, so 1.145 features (remote agent host browsing, remote canvases, mixed-harness nested sessions, cron automation schedules, agent verbosity) are recorded in the version history notes only and not queued yet. They become queue candidates once the machine is upgraded.

### 1.144.0 new features (from the official release notes, all sources verified live)

| Feature | Status | Queued? |
|---|---|---|
| GitHub Copilot Fusion — automatic model routing/orchestration, effort levels (Standard/High/Max) | GA (High/Max on Business/Enterprise) | ✅ Yes |
| Cloud sessions — agent tasks on GitHub-managed compute from VS Code | New (paid plan, org policies apply) | ✅ Yes |
| Assisted permissions — LLM risk assessment auto-approves low-risk tool calls | GA | ✅ Yes |
| Cloud automations — scheduled/event-triggered (issue/PR) cloud runs in Agents window | Preview | No (covered later; local automations already taught) |
| Agent Host Protocol v1.0 — open, extensible agent session protocol | v1.0 | No (deep/API candidate for a later week) |
| Image generation in Copilot chat (GPT Image 2.5 Flare) | Preview | No |
| Computer Use — agent drives desktop apps | Experimental | No (macOS/Windows only; this box is Ubuntu) |
| Working set — Keep/Undo review of Copilot harness file edits | Experimental | No (overlaps queued multi-diff editor topic) |
| Remote Environments dialog (GitHub Environments, Dev Tunnels, WSL, SSH) | New | No (bundled into cloud/remote lessons) |
| Copilot Memory in Copilot harness sessions | New | No (Copilot Memory already taught 2026-09-22; harness integration noted in lesson) |
| Model comparisons expanded (folderless chat, local/empty repos) | Default-on | No (model comparisons already taught 2026-10-30) |

## Topics Added (3)

Per the queue rule, the queue held 6 undelivered items (≥5), so this run adds exactly 3. All three are 1.144.0 version-change topics (highest priority), each scoped to a single daily lesson, none overlapping the queue or any phase's completedTopics.

1. **GitHub Copilot Fusion (Automatic Model Routing and Multi-Model Orchestration)**
   - Source: https://docs.github.com/en/copilot/concepts/models/copilot-fusion (fetched, 200, final URL matches; aka.ms/vscode-copilot-fusion also verified redirecting here)
   - Section: phase_4_deep · Difficulty: intermediate · Priority: 1
   - Rationale: GA in 1.144.0 and directly available on the Business plan. Completes the model-selection story (manual model choice → comparisons/Bake-Off taught 2026-10-30 → automated routing).

2. **Copilot Cloud Sessions (Running Agent Tasks on Managed Cloud Compute)**
   - Source: https://code.visualstudio.com/docs/agents/run/cloud-sandbox-sessions (fetched, 200, final URL matches)
   - Section: phase_2_vscode_integration · Difficulty: intermediate · Priority: 1
   - Rationale: headline 1.144.0 feature (session continuity theme); extends the just-completed Agent Management lesson into running sessions off-machine. Pairs well with the queued "Remote Control of Copilot CLI Sessions" topic (remote agent sessions doc links to it).

3. **Assisted Permissions (LLM Risk Assessment for Automatic Tool Call Approvals)**
   - Source: https://code.visualstudio.com/docs/agents/run/approvals (fetched, 200, final URL matches; the Assisted permissions section lives on this page)
   - Section: phase_3_advanced · Difficulty: intermediate · Priority: 1
   - Rationale: GA in 1.144.0; enterprise-relevant (org policies can restrict it; requires Folder isolation). Completes the permissions/sandboxing sequence (sandboxing taught 2026-10-26 + queued sandbox CA topic).

All source URLs were fetched and confirmed to return 200 with the final URL matching the requested URL before being recorded.

## Queue Length

- Before: **6** undelivered items
- Added: **3**
- After: **9** undelivered items (verified after write: 6 original entries preserved in order with original addedAt values, 3 new entries appended)

## Queue Order After This Run

1. Copilot CLI /research Command (2026-10-02, phase_4_deep, p3)
2. Remote Control of Copilot CLI Sessions (2026-10-16, phase_3_advanced, p2)
3. Canvases: Interactive Collaboration Surfaces (2026-10-23, phase_3_advanced, p2)
4. Bring Your Own OpenAI Key for Voice Mode (2026-10-30, phase_4_deep, p2)
5. Multi-Diff Editor File List (2026-10-30, phase_2_vscode_integration, p2)
6. Sandbox CA Certificate Trust and Per-Session Sandboxing Toggles (2026-10-30, phase_3_advanced, p2)
7. GitHub Copilot Fusion (2026-11-06, phase_4_deep, p1)
8. Copilot Cloud Sessions (2026-11-06, phase_2_vscode_integration, p1)
9. Assisted Permissions (2026-11-06, phase_3_advanced, p1)

## Gaps and Notes

- Current phase is **phase_3_advanced** (in progress); phase_4_deep is also in progress. New p1 version-change topics are deliberately spread across phases 2/3/4 so upcoming daily lessons continue the version-change thread.
- Next research run (2026-11-13) should watch: whether the machine upgrades to 1.145, and if so queue the 1.145 highlights (remote agent host browsing/remote canvases, mixed-harness nested sessions, cron schedules for local automations).
- Candidate deep-dive topics for later weeks (not queued): Agent Host Protocol v1.0 (phase_4_deep), image generation in chat (phase_2 or phase_4), working set Keep/Undo (phase_2), cloud automations (phase_3).
