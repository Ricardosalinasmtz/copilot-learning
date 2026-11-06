# 🎓 Copilot Lesson: Reclaiming Storage from Inactive Agent Session Worktrees (Worktree Cleanup)
**Date:** 2026-11-04 | **Day:** 35 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions | **Difficulty:** beginner

## 🎯 What You'll Learn

Agent sessions use Git worktrees to keep their code changes isolated, and each worktree is a full checkout of your repository on disk. Over time, worktrees left behind by finished, archived, or abandoned sessions can quietly eat gigabytes of storage. Today you'll learn the built-in Worktree Cleanup command: what it reclaims, what it deliberately preserves, and how to automate the process.

## 📖 The Concept

When you start an agent session with code isolation, VS Code gives the session its own worktree — a second checkout of the repository on a dedicated branch — so the agent's edits never touch your working folder. The catch is that marking a session done doesn't always remove the worktree. Archived, forgotten, or failed sessions can leave folders behind, and each one holds a full copy of the repository, often with build artifacts on top.

VS Code ships a dedicated command for exactly this: **Chat: Open Worktree Cleanup** from the Command Palette. It lists inactive or leftover session worktrees with each one's branch, path, and estimated size, and lets you filter by inactivity or workspace. Clear the selections for any worktree you want to keep, save open edits inside the selected worktrees, and confirm. Normal cleanup marks the listed sessions done, commits non-ignored uncommitted changes to their branches, and schedules the worktree folders for removal. Crucially, it never deletes the branches or the session history — restoring a cleaned-up session re-creates its worktree.

The cleanup editor also hunts down two sneaky leftovers: worktrees from done or archived sessions that weren't removed when the session ended, and Git-registered session worktrees under the managed `<repository>.worktrees` directory that no longer have a listed session. Arbitrary external worktrees and unregistered folders are excluded, so you can't accidentally nuke a worktree you created by hand. There's also a Force Remove option that permanently discards uncommitted changes and can remove a worktree still used by other sessions — reach for it only after you've preserved everything you care about.

Finally, you can make this a non-event. **Cleanup Settings** supports automatic cleanup of merged sessions through `chat.agentSessions.autoMarkAsDoneMergedSessionsAfterDays` and `chat.agentSessions.autoDeleteMarkedAsDoneMergedSessionsAfterDays` (both default to 0, off; 15 days is recommended). A session qualifies when it's not in progress, has sat idle past the threshold, and has at least one merged PR with no related open PRs — and its worktree is removed only if the branch tracks upstream with no outgoing commits or uncommitted changes, never by force. An experimental storage suggestion pings you in the Agents window when inactive worktrees reach 5 GiB or 20 worktrees.

## 🔑 Key Takeaways

- **Worktree Cleanup is a first-class command:** run `Chat: Open Worktree Cleanup` to reclaim disk from inactive or leftover session worktrees, reviewing branch, path, and estimated size before confirming.
- **Normal cleanup is safe by design:** it commits non-ignored uncommitted changes to the session branch, keeps branches and history, and re-creates the worktree if you restore the session — but ignored files are deleted, so don't rely on worktrees to hold generated output.
- **Force Remove and automatic deletion are the destructive paths:** Force Remove discards uncommitted changes outright, and auto-cleanup of merged sessions only fires when the branch is clean and tracks upstream.
- **Leftovers happen on their own:** marking a session done does not guarantee its worktree was removed, which is why the cleanup editor explicitly targets orphaned worktrees and unlisted managed worktrees.

## 💡 Practical Example

Your machine is low on disk after a month of parallel agent sessions. Open the Command Palette (`Ctrl+Shift+P`) and run:

```
Chat: Open Worktree Cleanup
```

The cleanup editor opens with an inactivity filter and a workspace picker. It might list something like:

| Worktree | Branch | Path | Est. size |
|---|---|---|---|
| Session #14 | copilot/auth-refactor | ~/repos/api/.worktrees/auth-refactor | 1.2 GiB |
| Session #21 | copilot/batch-import | ~/repos/api/.worktrees/batch-import | 890 MiB |
| Leftover (done session #9) | copilot/fix-flaky-test | ~/repos/api/.worktrees/fix-flaky-test | unknown |

Deselect `batch-import` — you still need those changes. Save the open editor inside the `auth-refactor` worktree, then confirm cleanup. VS Code commits the uncommitted changes on `auth-refactor` to its branch, marks the session done, and removes the worktree folder; restoring session #14 later re-creates the worktree from the branch. To keep this from recurring, open **Cleanup Settings** and set `chat.agentSessions.autoMarkAsDoneMergedSessionsAfterDays` to 15, so sessions with merged PRs archive themselves after two idle weeks.

## 🔗 Related Concepts

- [Agent sessions](https://code.visualstudio.com/docs/agents/concepts/sessions)
- [Code isolation and worktrees in agent harnesses](https://code.visualstudio.com/docs/agents/run/agent-harnesses)
- [Reviewing code edits](https://code.visualstudio.com/docs/agents/run/review-code-edits)
- [The Agents window](https://code.visualstudio.com/docs/agents/run/agents-window)

## 🤔 Quick Check

You open Worktree Cleanup and spot a leftover worktree from a session you marked done last week — the session's PR is merged, but the worktree folder is still on disk. Walk through what normal cleanup does to that worktree, and what happens to your ability to come back to the session afterward.

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Normal cleanup commits any non-ignored uncommitted changes to the session's branch, marks the session done, and schedules the worktree folder for removal — the branch and session history survive, so restoring the session re-creates its worktree. The only permanent loss is ignored files (for example, build output) inside the removed worktree, which is why normal cleanup is safe even for sessions you might revisit.

</details>

---
*Lesson 35 of Phase 2 (VSCode Integration) | Streak: 35 days*
