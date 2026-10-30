# Weekly Research Summary — 2026-10-30

## Version Status: CHANGE DETECTED

- **Baseline (progress.json `currentVersion`):** VSCode 1.142.0
- **Live (`code --version`):** VSCode **1.143.1** (commit e646868227c3e3a9e3f24277c8c7220be42c0be7, x64)
- **Action:** appended entry to `state/version_history.json` (detectedAt 2026-10-30) and updated `currentVersion.vscode` to 1.143.1 so next week compares against the new baseline.

Release notes: https://code.visualstudio.com/updates/v1_143 (1.143.1 stable). Headline changes: Copilot harness becomes the **default** for new sessions; model comparisons (Agent Bake-Off) enabled by default in the Agents window; on-device models (MAI Code 1.1 Flash) in Experimental; worktree storage cleanup; multi-diff file list; BYOK OpenAI Live model for Voice Mode; sandbox CA trust management; several enterprise managed-settings additions. Note: VS Code 1.144.0 (stable, released 2026-10-28) is already published upstream with further agent features (Copilot Fusion, cloud sessions, image generation in chat, computer use, AHP v1.0) — worth tracking before the user upgrades past 1.143.1.

## Topics Added (7 — full week, queue was below the 5 threshold)

All seven are tied to the 1.143 version change (highest-priority rule). Every source URL was fetched and verified (HTTP 200 with final URL matching the requested URL) on 2026-10-30.

| # | Topic | Source | Difficulty | Section | Priority |
|---|-------|--------|-----------|---------|----------|
| 1 | Model Comparisons and the Agent Bake-Off (Running the Same Prompt Across Models and Harnesses) | https://code.visualstudio.com/docs/agents/run/compare-models | intermediate | phase_4_deep | 1 |
| 2 | On-Device Models (Running Copilot's MAI Code 1.1 Flash Locally with llama.cpp) | https://code.visualstudio.com/docs/agent-customization/language-models | intermediate | phase_4_deep | 1 |
| 3 | Bring Your Own OpenAI Key for Voice Mode (Live Voice Models with BYOK) | https://code.visualstudio.com/docs/configure/accessibility/voice | intermediate | phase_4_deep | 2 |
| 4 | Enterprise AI Settings in VS Code (Managed Model Defaults, OTel Dynamic Headers, Managed Plugins, Local Harness Retention) | https://code.visualstudio.com/docs/enterprise/manage-ai-settings | advanced | phase_3_advanced | 1 |
| 5 | Reclaiming Storage from Inactive Agent Session Worktrees (Worktree Cleanup) | https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions | beginner | phase_2_vscode_integration | 1 |
| 6 | Multi-Diff Editor File List (Navigating Agent Session, Commit, and Stash Changes) | https://code.visualstudio.com/docs/agents/run/review-code-edits | beginner | phase_2_vscode_integration | 2 |
| 7 | Sandbox CA Certificate Trust and Per-Session Sandboxing Toggles | https://code.visualstudio.com/docs/agents/run/agent-sandboxing | advanced | phase_3_advanced | 2 |

## Rationale

- **Queue policy:** 4 undelivered topics were already in the queue (< 5), so 7 topics were added this week. No topic duplicates the queue or any phase's `completedTopics`.
- **Version-change priority:** the 1.142 → 1.143.1 bump was the dominant signal, so all new topics come from 1.143 release notes; each is scoped to a single daily lesson.
- **Model Comparisons (priority 1):** new in 1.143 (Preview, on by default in the Agents window). Fills the "Advanced prompt engineering / model selection" area of the Phase 4 curriculum; complements the BYOK lesson (2026-10-27) by adding the model-choice dimension.
- **On-Device Models (priority 1):** new in 1.143 (Experimental). Distinct from the completed BYOK lesson: Copilot-provided models (MAI Code 1.1 Flash) managed by VS Code via llama.cpp, no API key, no AI credits. Fills Phase 4 "Copilot API usage / performance optimization" adjacency (local inference, hardware requirements, Auto routing with cloud fallback).
- **Voice Mode BYOK (priority 2):** new in 1.143 (Preview). Note for the lesson: hosted Voice Mode is **not** available with Copilot Business/Enterprise — the BYOK OpenAI Live path (`agents.voice.useBYOKVoiceModel`) is the relevant route for this environment, and org policy can still restrict client BYOK.
- **Enterprise AI Settings (priority 1):** 1.143 added managed model defaults, `telemetry.headersProvider` dynamic OTel headers, auto-installed managed plugins, and Local-harness retirement controls (`vscode.temporaryAllowLocalHarness`, `temporaryDefaultToLocalHarness`). Extends the 2026-10-12 policies lesson into the VS Code side of governance — highly relevant for a Copilot Business (Enterprise) subscription.
- **Worktree Cleanup (priority 1):** new in 1.143 (Experimental). Practical disk-hygiene skill for anyone running parallel agent sessions; small enough for one lesson.
- **Multi-Diff File List (priority 2):** new in 1.143 (Experimental). Editing/tooling improvement that directly supports reviewing agent changes across commits, stashes, and sessions.
- **Sandbox CA Trust (priority 2):** new in 1.143. Deliberately scoped to the new certificate-trust management (`Sandbox: Manage Trusted CA`) and the per-session sandboxing toggle when policy permits bypass — the 2026-10-26 lesson already covered sandboxing setup and enterprise permission policies, so this is a narrow follow-up, not a duplicate.
- **Curriculum gaps covered:** Phase 4 "advanced prompt engineering / model orchestration" (topics 1–3) and Phase 3 "enterprise features" (topic 4) — the areas with the thinnest coverage so far, hence their priority-1 treatment where applicable.
- **Deferred for future weeks:** 1.144 items (Copilot Fusion, cloud sessions/automations, image generation in chat, computer use, AHP v1.0) — the user is on 1.143.1, so those are not installed yet; they will be the natural version-change batch if/when the baseline moves.

## Queue Length

- **Before:** 4 undelivered topics
- **After:** 11 undelivered topics (4 previous + 7 new)
- Delivery estimate at 1 lesson/day: ~9 school days, plus any additions from next week's run.

## Sources Consulted

- https://code.visualstudio.com/updates/v1_143 — 1.143 release notes (all seven topics traced here)
- https://code.visualstudio.com/updates/ (redirects to v1_144) — 1.144 release notes, for forward-looking context
- Verified 200/final-URL-match for: compare-models, language-models, voice (accessibility), manage-ai-settings, manage-sessions, review-code-edits, agent-sandboxing
