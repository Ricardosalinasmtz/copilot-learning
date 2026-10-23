# 🎓 Copilot Lesson: GitHub Agentic Workflows in GitHub Actions
**Date:** 2026-10-19 | **Day:** 23 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows | **Difficulty:** advanced

## 🎯 What You'll Learn

GitHub Agentic Workflows are markdown-defined automations that run an AI coding agent inside GitHub Actions: you write natural language instructions, and the agent reads repository context, makes decisions, and takes action (public preview). You still define the guardrails — triggers, permissions, and which write operations are allowed — in YAML frontmatter. By the end you'll be able to author, compile, and reason about the security model of an agentic workflow.

## 📖 The Concept

Traditional Actions workflows run a fixed sequence of steps; an agentic workflow replaces the step list with natural language instructions that an AI coding agent interprets. Each workflow is a single `.md` file with two parts: YAML frontmatter between `---` markers configuring when it runs, what permissions it has, and which write operations are allowed ("safe outputs"), and a markdown body containing the instructions the agent follows. So instead of scripting "if CI failed, grep the error and open an issue," you can write "investigate the failing CI run, explain the likely cause, and create an issue with a summary and suggested fix."

The lifecycle is simple but worth understanding: you author the markdown file, compile it into a hardened `.lock.yml` GitHub Actions workflow, then commit and push both files to the default branch. After that it runs like any other Actions workflow — on a trigger, from the repo web UI, or from the GitHub CLI. It's also engine-agnostic: the `engine` frontmatter property selects the coding agent (GitHub Copilot by default, plus Anthropic Claude, OpenAI Codex, and Google Gemini), and each engine needs its own authentication secret configured in the repository.

Security is the centerpiece of the design. Workflows are read-only by default, and the only writes permitted are the `safe-outputs` you explicitly declare (create an issue, add a comment, open a PR) — and those proposed outputs are scanned by agentic threat detection before they're applied. Secrets stay outside the agent runtime, agents execute in firewalled GitHub Actions environments, and role-based access control can restrict who may trigger or modify a workflow. Cost-wise, usage bills in two parts: Actions minutes plus inference, measured in AI Credits (1 AIC = $0.01 USD), with a per-run cap you can set via `max-ai-credits` (default 1,000 AIC).

## 🔑 Key Takeaways

- An agentic workflow is one markdown file: YAML frontmatter for triggers, permissions, and safe outputs, plus a natural-language instruction body.
- It compiles into a hardened `.lock.yml` Actions workflow — commit and push both files to the default branch to activate it.
- Read-only by default: writes only happen through declared, validated `safe-outputs`, which are scanned by threat detection before application.
- The engine is pluggable (Copilot default, Claude, Codex, Gemini), and usage bills in AI Credits (1 AIC = $0.01) on top of Actions minutes, with a `max-ai-credits` per-run cap.

## 💡 Practical Example

The docs' canonical example is a daily repo status report. A complete workflow file:

```markdown
---
on: daily

permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write

network: defaults

tools:
  github:
    toolsets: [default]

safe-outputs:
  create-issue:

---

# Daily Repo Status Report

Review recent activity in the repository, including issues, pull
requests, discussions, and code changes.

Create a GitHub issue summarizing what changed in the last 24 hours
(merged pull requests, closed issues, and new discussions), any
blockers or open questions mentioned in comments, progress toward
visible goals, and recommended next steps for maintainers.

Keep the summary concise. Adjust the level of detail based on how
much activity occurred.
```

Notice the minimal surface: read permissions everywhere and exactly one permitted write operation — `create-issue`. After compiling to `.lock.yml` and committing both files, the workflow runs on its schedule or on demand. In an organization repo with a Copilot plan, adding `copilot-requests: write` under `permissions` lets the built-in `GITHUB_TOKEN` make Copilot requests billed to the organization, so you skip a personal `COPILOT_GITHUB_TOKEN` entirely.

## 🔗 Related Concepts

- [Creating GitHub Agentic Workflows](https://docs.github.com/en/copilot/how-tos/github-agentic-workflows/creating-github-agentic-workflows)
- [Your first agentic workflow (quickstart)](https://docs.github.com/en/copilot/how-tos/github-agentic-workflows/quickstart)
- [Engine reference (gh-aw)](https://github.github.com/gh-aw/reference/engines/)
- [Copilot Automations — lesson 22](2026-10-16_copilot-automations-scheduled-event-triggered-cloud-runs.md)

## 🤔 Quick Check

Your team's Friday status-report agentic workflow currently creates a single summary issue, and the team now wants the agent to also comment on each PR merged that week and open a draft PR with suggested fixes. What do you need to add to the workflow's frontmatter for those writes to be permitted, and what would have happened if the agent tried them without that declaration?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Write-level permissions for each target (e.g. `issues: write`, `pull-requests: write`) plus a corresponding `safe-outputs` entry for every operation, including creating the draft PR; any attempted write not in `safe-outputs` is blocked. The design is a whitelist: workflows are read-only by default, and write actions are allowed only through the validated safe outputs you declared, which threat detection scans before they're applied.

</details>

---
*Lesson 23 of Phase 3 (Advanced Workflows) | Streak: 23 days*
