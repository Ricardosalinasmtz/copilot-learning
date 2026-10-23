# 🎓 Copilot Lesson: Copilot Automations (Scheduled and Event-Triggered Cloud Agent Runs)
**Date:** 2026-10-16 | **Day:** 22 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-automations | **Difficulty:** intermediate

## 🎯 What You'll Learn

Copilot automations let you run the Copilot cloud agent without anyone manually starting each task. You define a job once — a prompt, a trigger (a schedule or a repository event), and the tools it may use — and Copilot executes it automatically: triaging incoming issues, fixing failing tests nightly, or drafting release notes. Today you'll learn what makes up an automation, how triggers and tools shape its behavior, and the security guardrails GitHub builds in for unattended agent runs.

## 📖 The Concept

With a manually started Copilot cloud agent session, you hand Copilot a task every time you want work done. An automation inverts that: you define the task once and set the conditions under which it fires. Each automation has a name, a prompt describing the work, one or more triggers, a model, and a set of allowed tools.

There are four trigger types: a recurring schedule (hourly, daily, or weekly), when an issue is created, when a pull request is opened, and when a pull request is synchronized (new commits pushed to an existing PR). Event triggers can be narrowed with search-query filters, and PR triggers can additionally filter on files changed. By default, automations ignore events triggered by users who don't have write access to the repository — a built-in mitigation against prompt injection from external contributors. You can opt in to those events if you need them.

Tools are the main scoping mechanism. You explicitly grant each tool (pushing changes, opening pull requests, updating issue labels, and so on), or use the "Suggest tools" button to have Copilot propose a set based on your prompt. Grant only what the task needs. An automation can only act in the single repository it is scoped to, and it inherits the repository's cloud agent configuration: custom instructions, agent skills, firewall rules, and secrets/variables.

Two operational details are worth internalizing. First, automations are not committed to Git — they're stored separately from your code, and each automation is private to the user who created it, even while the cloud agent sessions it starts (prompts, logs, and any PRs) are visible to everyone with repository access. That's why prompts must never contain secrets; pass sensitive values through repository secrets instead. Second, each run consumes GitHub Actions minutes and GitHub AI Credits, billed to the automation's creator.

## 🔑 Key Takeaways

- An automation is a prompt + triggers (schedule or issue/PR events) + model + tools, defined once and run automatically.
- Least-privilege tools are the primary scope control: grant only what the task needs, and actions stay confined to one repository.
- Events from users without write access are ignored by default — a prompt-injection defense you can opt out of.
- The automation itself is hidden from the repo (not versioned, private to its creator), but the sessions it starts are visible to all repo members.
- Every run costs Actions minutes and AI Credits, billed to the creator.

## 💡 Practical Example

A common setup: "fix failing tests nightly."

1. Open the repository's **Agents** tab on GitHub → **Automations** pane (or the Automations tab in the Copilot app).
2. Name it `nightly-test-fixes`.
3. Prompt: "Check the main branch for failing tests. If any exist, attempt to fix them and open a draft pull request summarizing the changes."
4. Trigger: **On a schedule** → daily.
5. Tools: click **Suggest tools**, then review and keep only what's needed (push changes, open pull request).
6. Save. Each night, a cloud agent session runs the task and you wake up to a draft PR — attributed to you, and approvable by anyone but you, so normal review controls stay intact.

## 🔗 Related Concepts

- [Creating automations with Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-automations)
- [GitHub Agentic Workflows (automations as code)](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows)
- [Managing access to GitHub Copilot cloud agent](https://docs.github.com/en/copilot/concepts/enterprise/cloud-agent-access)

## 🤔 Quick Check

Your team's repository receives a steady stream of issues from external contributors, and you want an automation that labels new issues as "bug" or "enhancement" and adds a triage comment so maintainers stop triaging by hand. Given what you read today, what is the main security risk of letting those external issues trigger an agent, and what does GitHub do by default to prevent it?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** The main risk is prompt injection: a contributor can craft an issue title or body that tries to steer the automation's agent toward actions beyond labeling, such as pushing code. By default, GitHub automations ignore events triggered by users who don't have write access to the repository, so an external contributor's issue never fires the automation at all. You can opt in to those events, but you should only do it if the workflow genuinely requires it.

</details>

---
*Lesson 22 of Phase 3 (Advanced Workflows) | Streak: 22 days*
