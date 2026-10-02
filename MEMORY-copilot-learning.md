# MEMORY-copilot-learning.md

Project-specific memory for the GitHub Copilot Learning System.

<!-- project: github.com/Ricardosalinasmtz/sokka-orchestrator -->

## Decisions

- **Automation over external cron:** Uses OpenClaw's built-in automations for better integration and reliability
- **Weekly collection, daily delivery:** Research on Fridays, lessons Mon-Fri (not random/hourly)
- **Version-aware learning:** Track VSCode and Copilot version changes and prioritize new features
- **Curriculum approach:** Structured phases instead of random topic selection
- **Direct chat delivery:** Lessons delivered to main session, not webhooks
- **Automation architecture:** Delegation pattern via sokka-orchestrator, not direct agent assignments
  - Weekly research runs in sokka-orchestrator session (`agent:sokka-orchestrator:main`) but delegates to zuko-researcher via `sessions_spawn`
  - Daily lesson runs in sokka-orchestrator session (`agent:sokka-orchestrator:main`)
  - No direct zuko-researcher session keys in automation definitions
- **Communication pattern:** User ↔ sokka ↔ zuko (no direct user-zuko interaction)
- **Session configuration:**
  - Weekly research: sokka-orchestrator (agent:sokka-orchestrator:main), delegates to zuko
  - Daily lesson: sokka-orchestrator (agent:sokka-orchestrator:main)

## Technical Learnings

- OpenClaw automations support cron schedules with timezone specification
- Automation payloads can include detailed step-by-step instructions
- State tracking via JSON files in project directory
- VSCode 1.136.1+ has Copilot Chat built-in (no separate extension needed)
- GitHub Copilot Business (Enterprise) provides full feature access

## Environment

- **OS:** Ubuntu 24.04.4 LTS
- **VSCode:** 1.136.1
- **Copilot Integration:** Built-in (VSCode native)
- **Copilot Subscription:** GitHub Copilot Business (Enterprise)

## State File Fixes (2026-09-22 / 2026-10-02 Audit)

The `state/progress.json` file had several inconsistencies discovered during a thorough audit. Fixes 1-6 applied 2026-09-22; fix 7 applied 2026-10-02:

1. **`currentPhase` stale:** Was `"phase_1_core"` but phase 1 was already `completed` and phase 2 was `in_progress`. Changed to `"phase_2_vscode_integration"`. The daily lesson automation reads this for lesson numbering.
2. **Missing phase completion record:** "Copilot in Integrated Terminal" (delivered 2026-09-21) existed at root level but not in `phase_2_vscode_integration.completedTopics`. Added it.
3. **Duplicate queue entry:** "Custom Instructions for Copilot (Personal, Repository, Organization)" appeared twice — one with the wrong source URL (`prompt-engineering` docs instead of `response-customization`). Removed the incorrect entry; kept the correct one (priority 5, `response-customization` source).
4. **Root `completedTopics` redundant:** Root-level array duplicated all phase completion records. Removed to establish a single source of truth (per-phase `completedTopics`). Stats are tracked via `stats.totalLessonsDelivered` instead. The daily lesson and research automations must be updated to no longer append to root `completedTopics`.
5. **`versionHistory` always empty:** `state/progress.json.versionHistory` was never populated (version tracking happens in the separate `state/version_history.json`). Removed to eliminate dead data.
6. **Queue items lacked `addedAt`:** No record of when topics were added, making it impossible to distinguish stale vs fresh queue entries. Added `addedAt` field (ISO date) to every queue item. This also helps break priority ties — when two items share the same priority, the older entry (earlier `addedAt`) is delivered first.
7. **`environment` duplicated `currentVersion` (2026-10-02):** `environment.vscode` (`"1.136.1"`) and `environment.copilotIntegration` (`"Built-in (VSCode native)"`) carried the same two facts as `currentVersion.vscode` and `currentVersion.copilot` — a drift risk of the same class as fixes 1 and 4. Removed the two duplicated keys from `environment`, which now holds only static setup context (`os`, `copilotSubscription`).

### Why `currentVersion` was kept and `environment` trimmed

The two blocks have different intents: `environment` is a static, human-readable setup description, while `currentVersion` is the **mutable baseline the weekly research job diffs against** each Friday before appending to `state/version_history.json`. Keeping the machine-updated field in its own clearly-named block avoids mixing automation state into human setup notes.

Verified before removal that nothing consumed the deleted keys: the only occurrence of `copilotIntegration`/`environment.vscode` in the repo was the definition itself, `scripts/` is empty, and no code reads `progress.json`. By contrast the weekly research automation payload names `currentVersion` explicitly ("Compare with state/progress.json currentVersion"), so that field must stay.

**Check before editing either block:** the weekly research payload also hardcodes the environment as prose ("Ubuntu 24.04.4 LTS, VSCode 1.136.1, Copilot built-in..."). That is literal prompt text, not a field lookup, so it did not break — but it is a third copy of the same facts and will go stale on a VSCode upgrade. The `## Environment` section of this memory file is a fourth copy with the same caveat.

### Open item

The weekly research job reports "no version changes detected" but has never written to `state/version_history.json` — that file still holds only the initial 2026-09-11 entry. The version-check step should actually append on change.

## Dead Ends / Avoided Approaches

- ~~Hourly random topic selection~~ → Too chaotic, no curriculum structure
- ~~External cron + webhook delivery~~ → Unnecessary complexity when OpenClaw automations exist
- ~~Single combined job~~ → Separation of concerns: research vs delivery


## Version Tracking

- Initial version: VSCode 1.136.1, Copilot built-in
- Version check: Fridays at 12:30 PM during research phase

## Learning Phases

1. **Phase 1 - Core Copilot Features:** Inline suggestions, chat, slash commands, context management
2. **Phase 2 - VSCode Integration:** Terminal, debugging, refactoring, test generation
3. **Phase 3 - Advanced Workflows:** Custom instructions, Copilot Edits mode, PR workflows, enterprise features
4. **Phase 4 - Deep Customization:** API usage, extensions ecosystem, workflow automation
