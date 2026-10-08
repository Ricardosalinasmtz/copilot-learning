# 🎓 Copilot Lesson: Multi-File Edits with Copilot
**Date:** 2026-10-08 | **Day:** 16 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/review-code-edits | **Difficulty:** intermediate

## 🎯 What You'll Learn

Today's agents don't just edit one file — they change an entire feature across many files in a single request. You'll learn how VS Code surfaces those multi-file changes (per-request diff summaries, multi-file diffs, Source Control) and how the checkpoint plus request-editing system lets you roll back an entire batch of agent changes without digging through Git history.

## 📖 The Concept

When you delegate a task to the Copilot agent, it applies and saves edits directly in the session's folder or an isolated Git worktree. There is no "pending" state you approve file by file — the files on disk are real, and your job shifts from *accepting edits* to **reviewing changes before integrating them**. Select a changed file in the agent's response, or open Source Control, and treat the result like any other branch change: read the diff, run tests, use the debugger, and only then stage and commit (or apply/merge the worktree branch into your main workspace).

To keep that review manageable, VS Code tracks every request. Set `chat.checkpoints.showFileChanges` to `true` and each completed request gets a changed-files summary with line statistics, from which you can open a multi-file diff of the whole batch at once. Before every request, VS Code also takes a snapshot of the affected files — a **checkpoint**. If the agent went down the wrong path, hover over an earlier request and choose **Restore Checkpoint** to roll the workspace files and chat history back to that point. Alternatively, you can **edit a previous request**: VS Code reverts the changes from that request and everything after it, then resends your corrected prompt to the model.

One important caveat: checkpoints cover workspace files and chat history only. They do *not* undo terminal commands the agent ran, network requests, deployments, or changes to external services — for those effects, Git and the external service's own recovery tools are your safety net. Worktree-isolated sessions add another layer of safety: the agent's whole branch stays separate from your primary workspace, and after review you can apply, merge, check out, or discard the changes. And if you want the agent to confirm before touching certain files (say, `.env` or `.vscode/*.json`), the `chat.tools.edits.autoApprove` setting uses glob patterns to require explicit approval for those paths.

## 🔑 Key Takeaways

- Agent edits land on disk immediately (folder or worktree). Review via diff, Source Control, or a PR — there's no per-file pending approval state in Agent Host sessions.
- `chat.checkpoints.showFileChanges` gives you per-request changed-file summaries and a multi-file diff view of the whole batch.
- **Restore Checkpoint** rolls the workspace and chat history back to a previous request; **editing a request** reverts its changes and resends with the corrected prompt.
- Checkpoints do *not* undo terminal commands or external-service changes — Git remains the safety net for those.
- `chat.tools.edits.autoApprove` uses glob patterns to make the agent ask before editing sensitive files.

## 💡 Practical Example

Workflow: adding validation across a multi-endpoint API.

1. Ask the agent: "Add request validation to every handler in `src/routes/`, reusing the `zod` schemas from `src/schemas/`. Run `npm test` when done."
2. The session finishes with a per-request summary: `6 files changed, +142 −8`. Open **View All File Changes** to inspect the multi-file diff.
3. The diff shows it also reworked `src/middleware/auth.ts` — not what you asked for. Hover the request and choose **Restore Checkpoint** (or edit the request to add "don't touch middleware") and the workspace reverts.
4. Set a guard for the future:

```json
"chat.tools.edits.autoApprove": {
  "**/*": true,
  "**/.vscode/*.json": false,
  "**/.env": false
}
```

Now the agent can edit freely, but it must show you a diff and get your approval before touching `.env` or workspace configuration.

5. Happy with the result? Stage and commit via Source Control (for a worktree session: apply or merge the branch instead).

## 🔗 Related Concepts

- [Chat view and steering requests](https://code.visualstudio.com/docs/agents/run/chat-view)
- [Planning before implementation](https://code.visualstudio.com/docs/agents/run/planning)
- [Agent sandboxing](https://code.visualstudio.com/docs/agents/run/agent-sandboxing)
- [Security considerations for using AI in VS Code](https://code.visualstudio.com/docs/agents/run/security)

## 🤔 Quick Check

You asked the Copilot agent to add input validation to every endpoint handler in your API project. After the session you notice it also reworked the auth middleware in a way you didn't ask for, and while running the test suite the agent executed a database migration that created a file you don't want. You hover over the earlier request and choose Restore Checkpoint. What exactly gets rolled back, and why might the migration file still be sitting in your workspace?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Restoring rolls back the workspace files and chat history to that request's state, discarding later requests — so the unwanted auth middleware change disappears. But the migration file may remain, because checkpoints do not reverse completed terminal commands or changes to external services: the agent created the migration by running a CLI command, so you have to remove it yourself (Git is your safety net), and you'd want to restore to a checkpoint taken before that command ran.

</details>

---
*Lesson 16 of Phase 2 (VSCode Integration) | Streak: 16 days*
