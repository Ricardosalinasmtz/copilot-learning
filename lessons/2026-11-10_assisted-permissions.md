# 🎓 Copilot Lesson: Assisted Permissions (LLM Risk Assessment for Automatic Tool Call Approvals)
**Date:** 2026-11-10 | **Day:** 39 of learning streak
**Source:** code.visualstudio.com/docs/agents/run/approvals | **Difficulty:** intermediate

## 🎯 What You'll Learn

VS Code agents are constantly asking for permission to edit files, run terminal commands, and fetch URLs. Assisted permissions is a middle-ground permission level where a separate LLM "judge" evaluates the risk of each tool call and auto-approves the safe ones, so you only get interrupted for actions that genuinely deserve your attention. You'll also see how it fits into the full permissions model alongside Manual permissions, Allow all, and Autopilot.

## 📖 The Concept

VS Code uses two independent security layers for agents. **Approvals** decide whether an action runs automatically or waits for your confirmation. **Sandboxing** restricts the file system and network access that terminal commands get, even after they're approved. Permission levels live in the approvals layer and are picked from the permissions dropdown in the chat input; the level you choose can change at any time during a session.

There are three permission levels for Copilot harness sessions. **Manual permissions** (the default) applies your own tool, URL, and terminal approval settings, and anything not auto-approved prompts you. **Allow all** runs everything without confirmation. And **Assisted permissions** sits in between: an LLM judge assesses each tool call, calls the judge approves run automatically, and calls the judge doesn't approve still prompt you. The first time you select Assisted permissions, a warning dialog asks you to confirm — VS Code wants you to know this is model-based judgment, not a deterministic allowlist.

Assisted permissions reduces approval interruptions, but it does not replace your judgment: the model-based risk assessment can make mistakes. The practical pattern is to pair it with the other controls — sandboxing still restricts terminal file system and network access even under Allow all or Autopilot, and tool/URL approvals (managed via `chat.tools.eligibleForAutoApproval` and `Chat: Manage Tool Approval`) cover non-terminal actions. You can also force specific tools to always require manual approval by setting them to `false` in `chat.tools.eligibleForAutoApproval`, which keeps the risk judge from ever silently auto-approving those.

Scope matters too. This level exists for Copilot harness sessions (it's not available for Local harness sessions), an organization can set a managed default and can disable levels via enterprise policy, and worktree sessions in the Agents window always run with Allow all. So "how safe is my agent?" is really a stack of decisions: permission level + tool approvals + URL approvals + sandboxing.

## 🔑 Key Takeaways

- Assisted permissions uses an LLM judge to assess each tool call; only calls the judge doesn't approve require your confirmation — it cuts prompt fatigue, not all risk.
- It's a session-level permission (dropdown in the chat input, default via `chat.permissions.default`), available in Copilot harness sessions; Local harness sessions don't offer it.
- It complements rather than replaces other controls: sandboxing still bounds terminal commands even under Allow all, and you can pin any tool to always-ask by setting it to `false` in `chat.tools.eligibleForAutoApproval`.
- The first selection triggers a confirmation dialog — the docs explicitly warn that model-based risk assessment can make mistakes.

## 💡 Practical Example

You're running a multi-step agent task ("add tests for the payments module and fix any failures") and the approval prompts for `npm run test` are breaking your flow. Instead of switching to Allow all for the whole session:

1. Open the permissions dropdown in the chat input (next to the mode picker) in a Copilot harness session.
2. Select **Assisted permissions** and confirm the warning dialog.
3. Before running, open your settings and lock down what the judge must never auto-approve:

```json
{
  "chat.tools.eligibleForAutoApproval": {
    "git": true,
    "terminal": true,
    "github.copilot.pushBranch": false,
    "github.copilot.deleteFile": false
  },
  "chat.tools.terminal.autoApprove": {
    "/^git (status|show\\b.*)$/": true,
    "git push": false,
    "rm": false
  }
}
```

Now the judge auto-approves routine safe calls, your pinned tools always prompt, and anything destructive like `rm` or `git push` still stops for you. If you want to see what the judge has been auto-approving, the session history shows each tool call; and `Chat: Reset Tool Confirmations` (Command Palette) clears all saved approvals if you want a clean slate.

## 🔗 Related Concepts

- [Agent Sandboxing](https://code.visualstudio.com/docs/agents/run/agent-sandboxing)
- [Manage approvals and permissions (this source doc)](https://code.visualstudio.com/docs/agents/run/approvals)
- Sandboxing Copilot Agent Sessions and Enterprise Permission Policies (lesson 2026-10-26)
- GitHub Copilot Policies for Enterprises and Organizations (lesson 2026-10-12)

## 🤔 Quick Check

Your team lead runs a long-running Copilot agent session in the Agents window and complains that the constant "allow terminal command?" prompts are slowing the task down, but the company policy says the agent must never be allowed to delete files or push to the remote without a human looking. You suggest she use Assisted permissions, and also set `"github.copilot.pushBranch": false` and `"github.copilot.deleteFile": false` in `chat.tools.eligibleForAutoApproval`. Explain, in two or three sentences, why that combination satisfies the policy even though Assisted permissions is a model-based judge that can make mistakes.

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Setting a tool to `false` in `chat.tools.eligibleForAutoApproval` forces it to always require manual approval, and the confirmation dialog no longer offers an auto-approval option for that tool — so no LLM judgment is ever made for those tools. The risk judge only governs the tools that remain eligible for auto-approval, which means the two policy-critical actions (pushing and deleting files) always stop for human confirmation while everything else benefits from reduced prompt fatigue.

</details>

---
*Lesson 39 of Phase 3 (Advanced Workflows) | Streak: 39 days*
