# 🎓 Copilot Lesson: Remote Control of Copilot CLI Sessions
**Date:** 2026-11-11 | **Day:** 40 of learning streak
**Source:** docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-remote-control | **Difficulty:** intermediate

## 🎯 What You'll Learn

You'll learn how to keep steering a GitHub Copilot CLI session from GitHub.com or GitHub Mobile after you've stepped away from the machine it's running on. We'll cover what remote control can and can't do, why the host machine must stay online, and how org/enterprise policies gate the feature.

## 📖 The Concept

By default, a Copilot CLI session is only steerable from the machine where it runs. When you enable remote control for a session, it becomes reachable from your recent agent sessions list on GitHub.com or GitHub Mobile. The remote interface updates in real time, so you can watch ongoing output, answer the agent's questions, and respond to permission requests without ever touching the machine.

The key thing to internalize: the CLI does not move to the cloud. The session keeps running on your local machine — every shell command, file operation, and tool execution still happens there. Remote control is a control channel, not a relocation. Session events (conversation messages, tool executions, permission requests) flow from your machine to GitHub, and remote commands are polled back by the CLI and injected into the local session. Both the local terminal and the remote interface are active at the same time: you can type in either one, and the CLI uses the first response it receives for any prompt or permission request.

There are practical prerequisites. The machine must stay online and awake — a sleeping laptop or a dropped network connection makes remote control unavailable until it's back — and the session must be interactive. A scripted run using `--prompt` cannot be remotely controlled. No GitHub-hosted repository is required; availability for organizational users is determined by policy, not by where your code lives.

Policy is what actually gates the feature. For seats coming from an organization or enterprise, the "Store local sessions in the Cloud" policy must be set to "View and control" (it is unconfigured by default, and "View from cloud" gives you syncing only, without the ability to steer). Enterprise owners can layer on the `remoteControl` managed setting, applied per device, which can require SSO authorization for the controlling client or disable remote control of sessions hosted on that device entirely. On the privacy side: only the same GitHub account that started the session can view or interact with it remotely — nobody else can.

## 🔑 Key Takeaways

- Remote control lets you monitor and steer a CLI session from GitHub.com or GitHub Mobile, but the session itself keeps running entirely on your local machine.
- Local terminal and remote interface are active simultaneously; the CLI acts on whichever response arrives first for a given prompt or permission request.
- It only works for interactive sessions on a machine that stays online and awake, and org/enterprise users need the "Store local sessions in the Cloud" policy set to "View and control".
- Slash commands (e.g. `/permissions assisted`) are not available from the remote interface; use `/keep-alive` locally to stop the machine sleeping, and `copilot --continue`/`--resume` re-enable remote control automatically.

## 💡 Practical Example

Start a long task at your desk, then head out:

```bash
$ copilot
> Refactor the notification module to use the new event bus, updating all callers.
```

Before leaving, enable remote control for the session (see the "Steering a GitHub Copilot CLI session from another device" how-to) and run `/keep-alive` so the machine doesn't sleep mid-task. Twenty minutes later, in transit, you open GitHub Mobile, find the session under recent agent sessions, and see it parked on a permission request for `npm run test`. You tap approve on your phone; the CLI on your machine picks up the response and keeps working. When it finishes, the final summary is waiting in the same remote view — and none of the code ever left your machine.

## 🔗 Related Concepts

- [Steering a GitHub Copilot CLI session from another device](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/steer-remotely)
- [GitHub Copilot CLI overview](https://docs.github.com/en/copilot/concepts/agents/copilot-cli) (covered 2026-10-21)
- [Enterprise managed settings](https://docs.github.com/en/copilot/reference/enterprise-administrators/enterprise-managed-settings)
- [Sandboxing Copilot agent sessions](https://code.visualstudio.com/docs/agents/run/agent-sandboxing) (covered 2026-10-26)

## 🤔 Quick Check

You start a long Copilot CLI session on your work desktop, enable remote control, and leave the office. In the car you open GitHub Mobile and see the session waiting on a permission request for a file write, followed by a question about which migration strategy to use. What can you do from your phone to keep the task moving, and what is one thing you will not be able to do from the remote interface?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** You can approve the file-write permission request and answer the migration-strategy question directly from GitHub Mobile — the remote interface and the local terminal are both live, and the CLI on the desktop acts on the first response it receives. One thing you cannot do remotely is run a slash command such as `/permissions assisted`, since slash commands are not available in the remote interface; that has to happen from the terminal itself. And all of this only works while the desktop stays online and awake.

</details>

---
*Lesson 40 of Phase 3 (Advanced Workflows) | Streak: 40 days*
