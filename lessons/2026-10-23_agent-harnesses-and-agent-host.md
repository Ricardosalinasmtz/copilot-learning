# 🎓 Copilot Lesson: Agent Harnesses and the Agent Host
**Date:** 2026-10-23 | **Day:** 27 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/agent-harnesses | **Difficulty:** intermediate

## 🎯 What You'll Learn

VS Code now lets you run agent sessions under different "harnesses" — Copilot, Local, Claude, Codex, or a Cloud target — each with its own tools, customizations, and behavior. You'll learn what a harness actually is (and is not), how to pick one, how to tell which harness a session is using, and how sessions hand off between harnesses while keeping their context.

## 📖 The Concept

A harness is the layer that coordinates the language model, context, and tools an agent uses to complete a task. It's easy to confuse it with related ideas, so the docs are explicit: the harness is not the model, not the interface, and not the machine. "Local" is a harness name, not a promise that the model runs on your machine — a Local session can still send model requests to a hosted service. Picking a different model or agent role (Ask, Agent, Plan) inside a session does not change the harness, and opening the same session in another interface keeps its harness.

The main built-in option is the **Copilot harness**, which runs on the Copilot SDK inside the **Agent Host**. Because the Agent Host is independent of any editor window, Copilot harness sessions can keep working in the background after you close the window, continue across the Chat view, the Agents window, and other Copilot interfaces, and be reached remotely from another machine. The older **Local harness** lives inside VS Code's extension host instead: it's interactive and tied to the editor window, so closing that window stops the agent's work, and it's not available in the Agents window.

**Claude** and **Codex** harnesses are also built in (Codex via an extension or an experimental Agent Host integration), each with their own auth, billing, and permission models, and both can be disabled by enterprise-managed settings. The **Cloud target** isn't a single harness at all — it's an execution target where a provider's agent works against a GitHub repo and returns a pull request, without using your local context. You choose a harness from the Session Target control *before* sending the first prompt; after that, the only supported way to switch is a **handoff** (e.g., `/delegate` from a Copilot session, or the Session Target dropdown from a Local session), which carries the conversation history and context to the destination harness. Enterprise sandbox or permission policies can even force a Local session into the Copilot harness so the Agent Host can enforce them, archiving the original as read-only.

## 🔑 Key Takeaways

- A harness coordinates model, context, and tools — it is separate from the model you pick, the interface you use, and the machine where the session runs.
- Copilot harness = Copilot SDK + Agent Host: background-continuing, cross-interface sessions; Local harness = extension host, tied to the editor window, work stops if you close it.
- You select the harness in the Session Target control before the first prompt; to switch later you do a handoff, which carries history and context but can change tools, permissions, and models.
- Selecting a different model or agent role does not change the harness; check the chat title prefix or the Agents window watermark/tooltip to identify the active one.

## 💡 Practical Example

Start a session that can outlive your editor window:

1. Open the **Agents window**, pick your project folder, and hit **New**.
2. In the **Session Target** control, choose **Copilot** (it's the default for most sessions) and pick the **Agent** role.
3. Send your task — e.g., "Add input validation to the API routes and write tests for them."
4. Close the VS Code window. The Agent Host keeps running; reopen the Agents window (or check from the Copilot CLI/app) and the session is still there, mid-task.

Compare with a **Local** session: the same steps, but choosing Local in the Chat view — close that editor window and the agent's work stops immediately. And if you're already partway through a Local session and want Agent Host continuity, open the Session Target dropdown in the chat input and select **Copilot**: the conversation and context transfer over, the original Local session is archived read-only, and the work continues in the background.

## 🔗 Related Concepts

- [Agent harness concepts](https://code.visualstudio.com/docs/agents/concepts/agent-harnesses)
- [Agent Host](https://code.visualstudio.com/docs/agents/concepts/agent-host)
- [Manage agent sessions](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions)
- [Remote control for Copilot harness sessions](https://code.visualstudio.com/docs/agents/run/remote-agent-sessions)
- [Approvals and permissions](https://code.visualstudio.com/docs/agents/run/approvals)

## 🤔 Quick Check

Your team ships a feature in an agent session you kicked off in the Chat view, and you need to leave the office for the afternoon with your laptop lid closed. The session is a Local harness session that's roughly halfway done refactoring a module. Which two changes to your setup would let work like this survive being away from the machine, and what would you do *today* with this in-flight session to keep it going instead of letting it die when the window closes?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** For future work, start long-running tasks in the Copilot harness (Agent Host) instead of Local, so sessions keep running after the editor window closes and can be checked remotely — and optionally delegate well-scoped subtasks to the Cloud target via `/delegate`. For the in-flight session, use the Session Target dropdown in the chat input to hand off to the Copilot harness: VS Code carries the conversation history and context across, the original Local session is archived read-only, and the Agent Host continues the work in the background while you're away.

</details>

---
*Lesson 27 of Phase 2 (VSCode Integration) | Streak: 27 days*
