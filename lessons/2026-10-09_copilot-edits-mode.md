# 🎓 Copilot Lesson: Copilot Edits Mode

**Date:** 2026-10-09 | **Day:** 17 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/review-code-edits | **Difficulty:** intermediate

## 🎯 What You'll Learn

How Copilot's multi-file "edits" experience works in current VS Code, and — just as important — how to review, keep, revert, or roll back every batch of changes the agent makes. The standalone "Edits mode" label from mid-2025 has been folded into the agent workflow, but the mechanics you learned (coordinated multi-file diffs, per-edit keep/undo, checkpoints) are still the core of how you work.

## 📖 The Concept

When you ask Copilot for a change that spans several files — renaming a function, updating all call sites after a signature change, applying a refactor — the agent doesn't just type into one buffer. It gathers context (code, error messages, test results), makes the edits across files, runs checks, and revises its work as needed. That repeated cycle of reason → act → validate is the agent loop, and "edits mode" is really the review layer that sits on top of it.

Today there are two review workflows, depending on which harness runs the session. In Agent Host sessions (the current default), the agent applies and saves edits directly in the session's folder or an isolated Git worktree. There is no pending-review state: you inspect the diff in the changed-files list or in Source Control, then commit or merge. In the older Local (extension host) harness, supported edits are marked *pending* with a square-dot indicator in the Explorer and editor tabs — you step through them with the Up/Down overlay and choose Keep or Undo per edit, per file, or for everything at once. Staging those files in Source Control accepts all pending edits automatically; discarding rejects them.

The safety net is checkpoints. VS Code snapshots affected workspace files before each request. If a batch goes wrong, hover over the earlier request and select Restore Checkpoint — workspace files roll back and later requests are dropped from the conversation. You can also edit an earlier request, which reverts everything since it and resends your corrected prompt, or fork a new conversation from a checkpoint to try a different direction. If the session ran in a Git worktree, its changes are isolated from your main workspace entirely until you choose to apply, merge, check out, or discard them.

The mental model to keep: nothing lands in your codebase until you review and integrate it. Worktrees and checkpoints keep you safe, but they are not a security boundary, and they cannot undo commands the agent has already executed.

## 🔑 Key Takeaways

- The classic "Copilot Edits" experience (multi-file diffs with per-edit Keep/Undo) now lives inside the agent workflow: pending-edit overlay on the Local harness, review-before-commit on Agent Host.
- Checkpoints (`chat.checkpoints.enabled`, on by default) snapshot files before every request; Restore Checkpoint reverts file changes and drops subsequent requests, and you can fork a conversation from one.
- Staging in Source Control auto-accepts pending edits; discarding auto-rejects them.
- Worktree-isolated sessions keep agent changes out of your main workspace until you apply, merge, or check them out — ideal for parallel tasks.
- Checkpoints don't reverse terminal commands, network calls, or changes to external services — Git remains your permanent history.

## 💡 Practical Example

A realistic rename across a small project:

1. Open the Chat view (⌘I on macOS; Ctrl+Alt+I on Windows/Linux), switch to Agent, and prompt:
   > "Rename `formatDate` to `formatTimestamp` across `src/` and update every call site. Run the unit tests when done. Don't touch `migrations/`."
2. The agent finds the definition, edits each call site, runs the test command, and reports results. Turn on `chat.checkpoints.showFileChanges` to see per-request diff statistics in the summary.
3. Click a changed file to open its diff. On the Local harness, use the Up/Down overlay to Keep or Undo individual hunks.
4. If it still touched `migrations/0042_add_index.py`, hover over the agent's request and select Restore Checkpoint — the file returns to its earlier state and the request is dropped from the conversation.
5. Re-prompt with the constraint, review the new diff, run the tests yourself, then stage and commit from Source Control (staging also accepts any pending edits).

## 🔗 Related Concepts

- [Build with AI in VS Code (agents overview)](https://code.visualstudio.com/docs/agents/overview)
- [AI features cheat sheet](https://code.visualstudio.com/docs/agents/reference/ai-features-cheat-sheet)
- [Review and revert agent changes](https://code.visualstudio.com/docs/agents/run/review-code-edits)
- [Planning with an agent](https://code.visualstudio.com/docs/agents/run/planning)
- [Sessions and handoff](https://code.visualstudio.com/docs/agents/concepts/sessions)

## 🤔 Quick Check

You asked the agent to migrate a project from CommonJS to ESM imports, and it finished the job in four requests: updating imports in `src/`, fixing a test helper, updating the build config, and finally rewriting `package.json`. After reviewing, you realize the fourth request clobbered the `package.json` scripts you had been editing by hand, and you'd rather redo that last step yourself — but the first three requests are all good. What is the least destructive built-in way to roll back only the fourth request's file changes, and what kind of effect from that request could it still not undo?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Hover over the fourth request in the chat history and select Restore Checkpoint (or edit that request and resend it). VS Code reverts the file changes made by that request — and any later ones — and drops it from the conversation, leaving requests one through three intact. It cannot, however, undo any side effects the request already had outside workspace files, such as a terminal command the agent ran or a change pushed to an external service.

</details>

---
*Lesson 17 of Phase 2 (VSCode Integration) | Streak: 17 days*
