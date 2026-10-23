# 🎓 Copilot Lesson: GitHub Copilot CLI Deep Dive (Interactive/Programmatic Modes, Plan Mode, Autopilot)
**Date:** 2026-10-21 | **Day:** 25 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/copilot-surfaces/copilot-cli (queue link `.../concepts/agents/copilot-cli/about-copilot-cli` redirects here) | **Difficulty:** intermediate

## 🎯 What You'll Learn

The GitHub Copilot CLI gives you the full agentic power of Copilot directly in your terminal — no IDE required. Today you'll learn its two interfaces (interactive and programmatic), plan mode, which makes Copilot design before it writes code, and how the approval and sandboxing system keeps an agent that can run shell commands from doing something you didn't mean.

## 📖 The Concept

Copilot CLI is a standalone agent that lives in your terminal. It answers questions, writes and debugs code, and interacts with GitHub.com — it can start work on an assigned issue, raise a pull request, check the changes in a PR, or merge your open PRs. It runs on Linux and macOS, and on Windows from PowerShell or WSL.

There are two interfaces. The **interactive** one starts with a plain `copilot` and behaves like a conversation: you prompt it, steer it, and can even enqueue follow-up messages while it's still thinking or give inline feedback when you reject a tool permission request. The **programmatic** one takes a single prompt with `-p` / `--prompt`, completes the task, and exits — which is what makes it scriptable and pipeable for headless automation (`copilot -p "..." --allow-tool='shell(git)'`, or piping options in from a script).

Inside interactive sessions you can press Shift+Tab to cycle into **plan mode**. In plan mode, Copilot analyzes your request, asks clarifying questions to pin down scope, and builds a structured implementation plan *before* writing any code. That's the difference between "agent charges ahead" and "agent proposes, you approve" — it's the best guardrail for complex, multi-step tasks where a misunderstanding early on would be expensive to unwind.

The "autopilot" side is the automatic approval system. By default, the first time Copilot wants to use a tool that can modify or execute files, it asks — once for that command, or for the tool for the rest of the session. You can pre-decide with `--allow-all-tools`, `--allow-tool='shell(git)'`, `--deny-tool='shell(rm)'` (deny always wins). Because `--allow-all-tools` means the agent has your full filesystem and shell with no review, the docs' recommended safety net is sandboxing: `/sandbox enable` inside a session, or `copilot --cloud` for a fully isolated cloud-hosted session. Model selection and usage also matter here: credits are consumed per interaction, and extended 1M-token context or higher reasoning levels burn more, so the docs recommend keeping defaults and escalating only for hard tasks.

## 🔑 Key Takeaways

- Two interfaces: interactive `copilot` sessions for iterative work, and programmatic `copilot -p "prompt"` for one-shot, scriptable, headless runs.
- Plan mode (Shift+Tab) makes Copilot ask clarifying questions and produce a structured plan before any code is written — the guardrail for complex multi-step tasks.
- Approval is granular: approve a single command, a tool for the session, or pre-configure with `--allow-tool` / `--deny-tool` (e.g., `--deny-tool='shell(git push)'`); `--deny-tool` always takes precedence.
- Sandboxes are the safety net for automation: local (`/sandbox enable`) or cloud (`copilot --cloud`) isolate the agent's file and network access.
- Context is managed for you: auto-compaction near 95% of the token limit, `/compact` to compress manually, `/context` to see token usage.

## 💡 Practical Example

A programmatic run with scoped permissions — one-shot, headless, but the agent can only touch `git`:

```bash
# One-shot: summarize this week's commits — git allowed, nothing else
copilot -p "Show me this week's commits and summarize them" \
  --allow-tool='shell(git)'

# Headless but safe: allow everything except destructive commands
copilot -p "Revert the last commit, leaving the changes unstaged" \
  --allow-all-tools --deny-tool='shell(rm)' --deny-tool='shell(git push)'
```

And in an interactive session: run `copilot`, press Shift+Tab into plan mode for something like "refactor the auth module," let it ask its questions and present the plan, approve it, and watch it execute. Use `/context` to see how full the window is and `/compact` when you want to shrink history deliberately.

## 🔗 Related Concepts

- [Installing GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli)
- [Using GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/overview)
- [About cloud and local sandboxes for GitHub Copilot](https://docs.github.com/en/copilot/concepts/about-cloud-and-local-sandboxes)

## 🤔 Quick Check

You're wiring Copilot CLI into a post-deploy script that runs unattended: `copilot -p "Verify the deployment and fix any failing health checks" --allow-all-tools`. What is the primary risk of that combination, and what's the minimal change that keeps the script working headless while reducing it?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** With `--allow-all-tools` in an unattended run, the agent can execute any shell command you could run, without anyone reviewing it — so an over-eager "fix" could delete files or push broken state with no one watching. The minimal change is to replace `--allow-all-tools` with scoped allowances (e.g., `--allow-tool='shell(curl)' --allow-tool='shell(docker)' --allow-tool='write'`) so only the tools the script actually needs run unattended, ideally inside a sandbox (`copilot --cloud` or `/sandbox enable`) so even those tools are contained.

</details>

---
*Lesson 25 of Phase 3 (Advanced Workflows) | Streak: 25 days*
