# Research Summary: 2026-10-23

## Version Status

- **VSCode live:** 1.142.0 (from `code --version`)
- **Baseline in state/progress.json:** 1.139.1
- **Result:** **CHANGED** — version bump 1.139.1 → 1.142.0 (stable released 2026-10-14). Appended an entry to `state/version_history.json` and updated `currentVersion.vscode` to 1.142.0 so the next run compares against the new baseline.
- New-version features are treated as the highest-priority topics this week (3 added, all tied to the version change).

### New in 1.142.0 (per [release notes](https://code.visualstudio.com/updates/v1_142))

- **Copilot harness** (powered by the Copilot SDK): runs sessions through the Agent Host, continues in the background, accessible from both the editor window and the Agents window; progressively rolling out as the default harness for new sessions.
- **Canvases (Preview)**: interactive collaboration surfaces beside chat (e.g., kanban boards); provided by canvas extensions; `/create-canvas` skill generates one.
- **Move chats between folders and worktrees**: continue a chat in another folder or a new Git worktree without losing conversation history (Agents window).
- **Enterprise management**: enforced agent sandboxing and permission policies via managed settings; per-permission-mode controls (e.g., allow Assisted while blocking Allow All); automatic migration of policy-managed chats to Copilot Agent Host.
- **Redesigned model picker**: switch between Copilot and BYOK contributed models; manage effort levels and context windows.
- **Markdown editor** improvements (pasting images, renaming file links, breadcrumb navigation).

Note: VS Code 1.143 is already upstream (1.143.1), making the Copilot harness the hard default, adding model comparison (Preview), on-device models (Experimental), and file-level working sets (Experimental). Since the local install is on 1.142.0, 1.143 features are **not** queued; revisit if/when the machine updates.

## Queue Length

- **Before:** 6 undelivered topics
- **Added this run:** 3 (queue held 6 undelivered topics, i.e., ≥ 5, so the 3-topic rule applied)
- **After:** 9 undelivered topics

(Existing entries preserved in original order with original addedAt values untouched; verified by re-reading the file after writing.)

## Topics Added (all verified: HTTP 200, final URL matches requested URL)

1. **Agent Harnesses and the Agent Host (Copilot Harness, Background Sessions Across Windows)**
   - Source: https://code.visualstudio.com/docs/agents/run/agent-harnesses
   - Difficulty: intermediate | Section: phase_2_vscode_integration | Priority: 1
   - Rationale: Core new architecture in 1.142.0. The Copilot harness (Copilot SDK + Agent Host) is the foundation for background sessions, the Agents window, and the enterprise migration behavior below; without it the other 1.142 features don't make sense. Scope: harness comparison (Copilot vs Local vs Claude vs Codex vs Cloud target), identifying the active harness, session lifetime across editor/Agents windows. Fills the VSCode-integration surface of the new release; priority 1 as a version-change topic.

2. **Sandboxing Copilot Agent Sessions and Enterprise Permission Policies**
   - Source: https://code.visualstudio.com/docs/agents/run/agent-sandboxing
   - Difficulty: advanced | Section: phase_3_advanced | Priority: 1
   - Rationale: Directly relevant to our Business (Enterprise) subscription and the 1.142.0 "Enterprise management" highlight: enforced sandboxing (`sandbox.enabled`), file/network boundaries, the Linux prerequisite stack (bubblewrap, slirp4netns) on our Ubuntu 24.04 host, managed-settings permission policies, and the automatic migration of policy-managed chats to Copilot Agent Host. Natural follow-up to the completed "Copilot Policies for Enterprises" lesson (2026-10-12), now with the concrete sandbox/permission-mode enforcement layer.

3. **Canvases: Interactive Collaboration Surfaces with Agents (Canvas Extensions, /create-canvas)**
   - Source: https://docs.github.com/en/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
   - Difficulty: intermediate | Section: phase_3_advanced | Priority: 2
   - Rationale: New in 1.142.0 (Preview). Covers what canvas extensions are, how canvases differ from chat-only workflows, discovery/installation, and using `/create-canvas` in VS Code with the Copilot harness. Fits Phase 3 "team collaboration patterns" — canvases are explicitly a human-agent shared-surface collaboration feature (kanban, triage boards).

## Research Notes

- Phase 3 (Advanced Workflows) remains the current phase; its curriculum is now well covered (9 topics completed). The 1.142.0 release shifts focus: two of the three new topics close the enterprise/VSCode-integration gap opened by the version bump.
- Candidate source URLs verified by fetching each and confirming final URL equality. The guessed paths `docs/agents/concepts/agents-window-overview`, `docs/agents/concepts/agent-permissions`, and `docs/copilot/customization` all 404 — not recorded. "Move chats between folders/worktrees" is covered under https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions (verified 200) and is deferred: it is a small feature and pairs with the Agents-window lesson that can be spun off from topic 1 if needed.
- The redesigned model picker was not queued separately: it is a UI change, and the queued "BYOK" topic (2026-10-02) already covers the model-selection substance.
- No duplicate topics: none of the three appear in the existing queue or in any phase's completedTopics.
- Note for the next run: the 1.143 upstream release (harness as hard default, model comparison, on-device models, working sets) is the next version-change candidate once the local install catches up.
