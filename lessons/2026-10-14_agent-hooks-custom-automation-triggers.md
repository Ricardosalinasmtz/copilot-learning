# 🎓 Copilot Lesson: Agent Hooks for Copilot (Custom Automation Triggers)

**Date:** 2026-10-14 | **Day:** 20 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agents/hooks | **Difficulty:** advanced

## 🎯 What You'll Learn

Hooks let you plug your own shell commands into the lifecycle of a Copilot agent — at session start, on prompt submission, before and after every tool call, and on errors. You'll learn the eight hook types, how hooks are declared in JSON, and why `preToolUse` is the one that gives you real enforcement power over what an agent is allowed to do.

## 📖 The Concept

A hook is a custom shell command that runs at a strategic point in an agent's workflow. Instead of only telling Copilot what to do, you get deterministic triggers: "when the agent is about to run a tool, run *my* script first." Hooks receive detailed JSON about the agent's action, so your script can inspect the command or file being touched and react accordingly — approving, denying, logging, or scanning for secrets.

Hooks are available for the **Copilot cloud agent** on GitHub and for the **GitHub Copilot CLI** in your terminal. You define them in JSON files: repository-level hooks live at `.github/hooks/*.json` and apply to any Copilot agent working in that repository, while personal hooks live at `~/.copilot/hooks/*.json` and apply wherever you use the CLI.

There are eight hook types: `sessionStart` and `sessionEnd` (initialize environments or clean up, archive logs), `userPromptSubmitted` (record what people ask), `preToolUse` and `postToolUse` (the before/after pair for every tool such as `bash`, `edit`, or `view`), `agentStop` and `subagentStop` (when the main agent or a subagent finishes), and `errorOccurred` (logging and alerting on failures). Of these, `preToolUse` is the most powerful: it runs *before* a tool executes and is the only hook that can **approve or deny** the execution, which is how you block dangerous commands or enforce security policy.

The configuration format is a JSON document with `"version": 1` and a `hooks` object mapping each hook type to an array of definitions. Each definition is `type: "command"` plus a `bash` script (Unix) and/or `powershell` script (Windows), with optional `cwd`, `env` variables, and a `timeoutSec` (default 30). Because hooks run **synchronously and block the agent**, the docs recommend keeping them under about 5 seconds, logging asynchronously, and always setting timeouts. On the security side: validate and sanitize the JSON input, escape your shell commands properly, never log tokens or passwords, and be careful with hooks that make network calls.

## 🔑 Key Takeaways

- Hooks are your own shell commands wired into the agent lifecycle — configured in JSON at `.github/hooks/*.json` (repo-wide) or `~/.copilot/hooks/*.json` (personal CLI).
- `preToolUse` is the gatekeeper: the only hook type that can approve or deny a tool execution before it runs.
- The before/after pair (`preToolUse` + `postToolUse`) plus `userPromptSubmitted` gives you a complete audit trail of what was asked and what was executed.
- Hooks block the agent — keep them fast (<5 s), sanitize input, never log secrets, and always set timeouts.

## 💡 Practical Example

A repository-level hook file at `.github/hooks/compliance.json` that logs every prompt, screens every tool call, and cleans up on exit:

```json
{
  "version": 1,
  "hooks": {
    "userPromptSubmitted": [
      {
        "type": "command",
        "bash": "./scripts/log-prompt.sh",
        "cwd": "scripts",
        "env": { "LOG_LEVEL": "INFO" }
      }
    ],
    "preToolUse": [
      {
        "type": "command",
        "bash": "./scripts/security-check.sh",
        "cwd": "scripts",
        "timeoutSec": 15
      }
    ],
    "postToolUse": [
      {
        "type": "command",
        "bash": "cat >> logs/tool-results.jsonl"
      }
    ],
    "sessionEnd": [
      {
        "type": "command",
        "bash": "./scripts/cleanup.sh",
        "cwd": "scripts",
        "timeoutSec": 60
      }
    ]
  }
}
```

Your `security-check.sh` reads the agent's JSON input (which tool, with what arguments), exits non-zero to **deny** the call, or exits 0 to approve it. Commit the file to the repo and every Copilot agent working in it inherits the policy.

## 🔗 Related Concepts

- [GitHub Copilot hooks reference](https://docs.github.com/en/copilot/reference/hooks-reference) — full hook types, best practices, advanced patterns
- [Customize cloud agent workflows with hooks](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/use-hooks)
- [Using hooks with GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-hooks)
- [Custom Agents (Agent Profiles for Shared, Team-Specific Workflows)](2026-10-13_custom-agents-agent-profiles-team-workflows.md) — yesterday's lesson; hooks complement custom agent profiles with enforced guardrails

## 🤔 Quick Check

You're rolling out Copilot agents on a repository that contains production deploy scripts, and the security team requires that every prompt and every tool call be recorded for audit. Which hook types would you wire up, where would the configuration file live, and why is `preToolUse` the only hook that can actually stop a dangerous command before it runs?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Use `userPromptSubmitted` to log each prompt, and the `preToolUse` + `postToolUse` pair to audit tool invocations and their results, defined in a `.github/hooks/*.json` file so the policy applies to any agent working in the repo. `preToolUse` fires *before* the tool executes and is the only hook type that can approve or deny it; `postToolUse` only runs after the fact, so a denylist or secret-scan check must live in `preToolUse` to actually prevent a command from running.

</details>

---
*Lesson 20 of Phase 4 (Deep Customization) | Streak: 20 days*
