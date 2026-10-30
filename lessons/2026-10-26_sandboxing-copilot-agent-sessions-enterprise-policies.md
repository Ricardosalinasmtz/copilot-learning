# 🎓 Copilot Lesson: Sandboxing Copilot Agent Sessions and Enterprise Permission Policies
**Date:** 2026-10-26 | **Day:** 28 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/agent-sandboxing | **Difficulty:** advanced

## 🎯 What You'll Learn

Copilot agent sessions can run real terminal commands and start language or tool servers on your machine (or a connected remote). Agent sandboxing confines those processes to the file system and network access you explicitly configure, so a misbehaving or prompt-injected agent can't roam your disk or leak data to the network. Today you'll learn the security model, how to enable and tune sandboxing, how to inspect the effective policy, and how an organization can lock it down.

## 📖 The Concept

VS Code gives agents two *separate* protection layers, and it matters not to conflate them. **Approvals** decide whether a specific action runs automatically or waits for your confirmation — that's a gate on *whether* something is allowed. **Sandboxing** decides *what* an action can touch once it runs: the file system paths and network destinations that supported agent operations can reach. Approvals gate intent; sandboxing limits blast radius. Both can be active at the same time, and each does work the other doesn't.

The key mental model: sandboxing is **not** a virtual machine and **not** a user-account boundary. A sandboxed process still runs on the execution host under your own account. What sandboxing removes is *capability* — paths it can't read or write, domains it can't reach, credentials it can't see. Everything you grant weakens that isolation: extra paths, developer-tool access, outbound network, or permission to run outside the sandbox. The guiding principle is least privilege — grant only what the task actually requires.

One surprise worth calling out: **turning on sandboxing does not block outbound network by default.** `chat.agent.sandbox.network.allowNetwork` defaults to `true`, so a sandboxed agent can still talk to the internet unless you restrict it. File-system isolation and network isolation are independent controls, each configured separately.

Platform support matters because each OS implements the sandbox differently. macOS 15+ uses the built-in Seatbelt facility; glibc-based Linux and WSL2 need `bubblewrap 0.5.0+`, `slirp4netns`, `util-linux 2.35+`, and iptables; Windows 11 needs BaseContainer and a recent OS build. Alpine/musl Linux and WSL1 are not supported by this runtime.

## 🔑 Key Takeaways

- Approvals and sandboxing are **distinct layers**: approvals gate *whether* an action runs; sandboxing restricts *what* it can touch once it runs.
- Sandboxing applies to terminal commands **and** their child processes, and (when configured) to the MCP and language servers a session starts — but not to built-in tools that don't spawn OS processes.
- **Outbound network is allowed by default** (`chat.agent.sandbox.network.allowNetwork` = `true`). You must explicitly restrict domains or disable network to lock it down.
- `chat.agent.sandbox.fileSystem.allowDevToolAccess` defaults to `true` and silently grants read access to toolchains, PATH, and dev caches — **including registry tokens**. Set it to `false` for tighter isolation.
- Credential access (`authenticateGit`, `authenticateGh`) also defaults to `true`. A sandboxed process holding credentials can act with that account's full permissions, so turn off credentials the task doesn't need.
- Verify what actually applies with the **`/sandbox policy`** command instead of inferring behavior from another tool.
- On Linux, install the dependencies with `sudo apt-get install bubblewrap slirp4netns util-linux iptables`.
- Organizations can force sandboxing via managed settings (preview); `sandbox.allowBypass` controls whether users can toggle it off per session.

## 💡 Practical Example

Enabling sandboxing on a glibc Linux machine and locking it down to a narrow network with no dev-tool access:

```jsonc
// settings on the machine where the session actually runs
{
  // Turn on sandboxing for all supported agent operations
  "chat.agent.sandbox.enabled": true,

  // Remove the default broad read access to toolchains, PATH, and dev caches (incl. registry tokens)
  "chat.agent.sandbox.fileSystem.allowDevToolAccess": false,

  // Keep outbound network but only to specific destinations
  "chat.agent.sandbox.network.allowNetwork": true,
  "chat.agent.sandbox.network.allowedDomains": ["api.github.com"],
  "chat.agent.sandbox.network.deniedDomains": [],
  "chat.agent.sandbox.network.allowLocalNetwork": false,

  // Grant only the credentials the task actually needs
  "chat.agent.sandbox.credentials.authenticateGit": true,
  "chat.agent.sandbox.credentials.authenticateGh": false,

  // Keep managed MCP and language servers inside the sandbox
  "chat.agent.sandbox.mcpServers": true,
  "chat.agent.sandbox.lspServers": true
}
```

Then install the Linux dependencies and start a **new** harness session:

```bash
sudo apt-get install bubblewrap slirp4netns util-linux iptables
```

Finally, confirm what actually applies to the running session:

```
/sandbox policy
```

`/sandbox policy` prints the effective permissions — runtime defaults plus any organization-managed restrictions — which is the source of truth rather than guessing from a different tool's behavior.

## 🔗 Related Concepts

- [Manage approvals and permissions](https://code.visualstudio.com/docs/agents/run/approvals)
- [Trust and safety for AI agents (agent sandboxing)](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
- [Agent Host](https://code.visualstudio.com/docs/agents/concepts/agent-host)
- [Enterprise agent sandboxing / manage AI settings](https://code.visualstudio.com/docs/enterprise/manage-ai-settings)

## 🤔 Quick Check

A team is enabling agent sandboxing for the first time on macOS. A teammate's sandboxed `gh` command starts failing with a TLS/certificate error, even though the very same command works fine outside the sandbox. Based on what you learned today, what is the most likely cause, what is the correct fix, and what should you absolutely never do just to make the error go away?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** The most likely cause is that the sandbox's credential proxy uses a local, self-signed CA that the machine's OS trust store doesn't trust — so a tool that relies only on the OS certificate store can't verify the proxy's TLS. The correct fix is to install (or rotate) the trusted CA on the machine running the agent host, via the "Sandbox: Manage Trusted CA" command or the in-chat "Sandbox trusted CA is not installed" notice. You should never disable TLS certificate verification to work around the error.

</details>

---
*Lesson 28 of Phase 3 (Advanced Workflows) | Streak: 28 days*
