# 🎓 Copilot Lesson: Copilot Cloud Sessions (Running Agent Tasks on Managed Cloud Compute)
**Date:** 2026-11-09 | **Day:** 38 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/cloud-sandbox-sessions | **Difficulty:** intermediate

## 🎯 What You'll Learn

You'll learn how to offload agent tasks to managed cloud compute so your machine doesn't have to stay online, how to reconnect to the same cloud session later and continue the conversation, and which settings control cloud session discovery, preparation, and auto-connect.

## 📖 The Concept

A cloud session runs a Copilot agent in a managed execution environment, so the work happens in the cloud rather than on the device displaying the conversation. The core idea is continuity: the session's execution is decoupled from your current client. You start a task, can close VS Code or walk away, and later reconnect to the same existing session — from the same or another machine, as long as you're signed in with the account that owns the session. The conversation and environment live on independently of whichever client opened them.

Cloud sessions sit at the opposite end of the remote-execution spectrum from the options you've seen before. GitHub Environments (preview) and SSH or dev tunnels all reach an Agent Host on a machine you own, and that machine has to stay running and reachable. A cloud session needs no host of your own: the service provisions and manages the compute for you. There are limits, though. Today cloud sessions support the Copilot harness only — you can pick an available model, but you can't switch to the Claude or Codex harness inside a cloud session. The repository must be hosted on GitHub.com and accessible to your account, and overall availability depends on your account and organization.

The workspace model matters when you're writing prompts. The selected repository is prepared in the cloud environment, but your local files, dependencies, terminal state, uncommitted changes, and installed extensions are not copied there — the agent only sees what exists in the cloud workspace, so the prompt has to carry the context. And when you resume, keep in mind that a recorded transcript can be readable even when the environment is offline: reading history is not resuming live work. The service, not you, controls environment availability and lifetime, so don't treat a cloud session as permanent storage or assume a transcript guarantees a running environment.

## 🔑 Key Takeaways

- Cloud sessions run the agent on managed cloud compute — your machine can go offline, and you resume the same session from any VS Code signed in with the owning account.
- The cloud workspace is its own prepared clone of the selected GitHub.com repository; local files, terminal state, and extension tools are absent, so put the context in the prompt.
- `chat.agentHost.cloudSandbox.enabled` must be on to create and discover cloud sessions; turning off `chat.remoteAgentHostsEnabled` blocks all remote agent hosts, cloud sessions included.
- Stop request, close chat, mark done, and delete session each do different things — none of them reliably shuts down the underlying compute; the service manages cloud resources separately from the session list.

## 💡 Practical Example

Starting a cloud task from the Agents window:

1. Open the Agents window and select **New**.
2. Choose the **Cloud** environment, then use **Choose Repository…** to pick the GitHub.com repository for the task.
3. Select an available model and review the session's available mode and permission controls.
4. Enter the task with its verification criteria and send the prompt:

```text
@workspace Migrate the pagination in src/api/users.py from offset/limit to
keyset pagination. Add pytest cases for empty, full, and partial pages, run
the test suite to verify, and report the diff plus test output.
```

5. Wait for the cloud environment to connect and the repository to be ready, then continue in the session as the agent responds.

Later, from a different laptop, sign in with the same GitHub account, find the session in the sessions list, check its connection state, and send your follow-up in the same conversation. If you expect to compose long prompts, `chat.agentHost.cloudSandbox.earlyPreparation` (off by default) can start preparing the cloud compute while you're still writing.

## 🔗 Related Concepts

- [Manage agent sessions](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions)
- [Connect to sessions on your own machines (remote agent sessions)](https://code.visualstudio.com/docs/agents/run/remote-agent-sessions)
- [Secure AI-assisted development — cloud session security considerations](https://code.visualstudio.com/docs/agents/run/security#_cloud-sessions)
- [Agent Management (last week's lesson on controlling parallel sessions locally)](2026-11-05_agent-management-centralized-control.md)

## 🤔 Quick Check

You start a cloud session to refactor a large service in your team's GitHub repository, then close VS Code for the day. The next morning on a work laptop, you find the session's transcript in the sessions list, but the session appears read-only and won't accept new messages. Based on what you learned today, what is the most likely explanation, and what should you check before sending any follow-up instructions?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** A readable transcript does not mean the cloud environment is running — reading history is different from resuming live work, and the service controls environment availability and lifetime. You should sign in with the account that owns the session, open it and check its connection/availability state, and let the environment connect or wake (for example from the Agents window, which connects to or wakes the environment on open) before sending a follow-up; if the environment is gone, the transcript remains but there is no live host to message.

</details>

---
*Lesson 38 of Phase 2 (VSCode Integration) | Streak: 38 days*
