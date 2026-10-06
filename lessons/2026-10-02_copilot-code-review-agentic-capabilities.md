# 🎓 Copilot Lesson: Copilot Code Review — Full Context Analysis, Cloud Agent Handoff & Agentic Capabilities

**Date:** 2026-10-02 | **Day:** 12 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agents/code-review | **Difficulty:** advanced

## 🎯 What You'll Learn

Copilot code review isn't just a static linter — it's an agentic system that gathers full repository context, routes complex PRs to higher-reasoning models, and can automatically create fix PRs via Cloud Agent. This lesson covers how it works under the hood.

## 📖 The Concept

### From Review to Agent

Traditional automated code review (think CodeQL, sonarqube, ESLint) runs a fixed set of rules against your code. Copilot code review works differently: it uses **agentic capabilities** that extend far beyond pattern matching.

When you (or your repository config) request a review, Copilot doesn't just diff your changed files. It performs **full project context gathering** — analyzing your entire repository to understand the architecture, conventions, dependencies, and coding standards before commenting on any specific change. This means its feedback is *contextually aware* rather than generic.

### Two Agentic Capabilities

1. **Full Project Context Gathering** — Copilot scans the whole repo to build a mental model of your codebase. It reads custom instructions, AGENTS.md, agent skills, and MCP server context to tailor its review to *your* project specifically.

2. **Cloud Agent Handoff** — After reviewing a PR, Copilot can pass its suggestions to the **Copilot Cloud Agent**, which can automatically create a *new pull request* against your branch with the suggested fixes applied. This turns review feedback into actionable work without you manually implementing each suggestion. (Currently in public preview.)

### Lite vs. Balanced Effort

Copilot code review offers two effort levels you can choose per-PR:

- **Lite** (default): Fast, targeted feedback on common bugs, security issues, and style problems. Estimated cost: $0.05–$1.00 in AI credits.
- **Balanced**: Routes complex PRs to a higher-reasoning model for deeper analysis of security-sensitive code, cross-service changes, and intricate logic. Estimated cost: $0.25–$5.00 in AI credits.

The effort selection follows a priority chain: per-review choice → previous effort on this PR → requestor's default → repository default → organization default → GitHub's built-in default (Lite).

### Cost & Billing

Reviews cost in two dimensions:
- **AI credits** for the model interaction itself
- **GitHub Actions minutes** for agentic features (context gathering, Cloud Agent handoff)

If GitHub Actions is unavailable, reviews still generate — but without agentic capabilities. By default, standard GitHub-hosted runners are used; you can upgrade to larger runners or use self-hosted runners.

### Copilot Approvals (Public Preview)

Copilot's review includes an **approval assessment** — a determination of whether the PR looks ready to merge. When enabled in your repository/org/enterprise settings, Copilot can submit an **approving review that counts toward your required-approval rule**, just like a human teammate's approval. New commits after approval dismiss it automatically.

### MCP Servers & Agent Skills in Reviews

Copilot code review integrates with your existing tooling:
- **Agent Skills** (`.github/skills/`) — task-specific workflows that extend review beyond built-in analysis. Copilot reads skills from the *head branch* (the PR branch), so you can test changes in the same PR.
- **MCP Servers** — pull context from issue trackers, docs, service catalogs, and incident tools. GitHub MCP and Playwright MCP are enabled by default. You can disable MCP tools for code review independently from Cloud Agent.

### Custom Instructions, AGENTS.md & Skills — When to Use What

| Source | Best For | Scope |
|---|---|---|
| `.github/copilot-instructions.md` | Repo-wide Copilot rules | Repository, Copilot-only |
| `.github/instructions/**/*.instructions.md` | Path-specific rules | Specific directories/files |
| `AGENTS.md` | Cross-agent conventions | Any AI tool/agent |
| `.github/skills/` | Task-specific workflows | Per-task invocation |

### Excluded Files

Copilot skips: dependency lock files (`package.json`, `Gemfile.lock`), log files, and SVG files.

## 🔑 Key Takeaways

- **Agentic ≠ static**: Copilot reviews with full repository context, not just changed files
- **Cloud Agent handoff** can auto-create fix PRs from review suggestions (public preview)
- **Lite vs. Balanced** lets you balance speed vs. depth based on PR criticality
- **Two cost dimensions**: AI credits + GitHub Actions minutes
- **Copilot approvals** can satisfy required-approval rules when enabled
- **Skills and MCP** extend review with your own workflows and tooling context
- **Model switching is not supported** — it's a purpose-built product with a tuned model mix

## 💡 Practical Example

### Setting up automatic reviews with Balanced effort for security-sensitive PRs

In your repository settings, configure automatic reviews, then use this approach:

1. **Set repo-level review effort** to Balanced for sensitive repos:
   - Go to Settings → Copilot → Code Review
   - Set default effort level

2. **Add review-specific agent skills:**
   ```
   .github/skills/code-review/
   └── SKILL.md          # Review-focused workflow
   ```

3. **Configure MCP servers** to pull Jira/Linear issue context:
   - Settings → Copilot → MCP Servers
   - Ensure "Allow Copilot to use MCP tools when reviewing pull requests" is enabled

4. **Enable Copilot approvals** (if your org allows):
   - Settings → Copilot → Approvals
   - Enable for required-approval rule satisfaction

### Manual override example

When reviewing a critical security PR:
- In the PR's **Reviewers** section, assign Copilot
- Click the effort level selector and choose **Balanced** instead of default Lite
- Copilot will route to the higher-reasoning model with full context

## 🔗 Related Concepts

- [Configuring Code Review by GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/set-up-copilot/configure-code-review)
- [Using GitHub Copilot Code Review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)
- [GitHub Code Quality](https://docs.github.com/en/code-security/concepts/code-quality/code-quality)
- [Agent Skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Configure MCP Servers](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers)
- [About GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agents/about-github-agentic-workflows)

## 🤔 Quick Check

> You have a PR that modifies a payment processing module across three microservices. The module handles PCI data, and your team has strict quality standards. Which review effort level should you use, and why might you also want to add agent skills to your repo?

---
*Lesson 12 of Phase 3 (Advanced Workflows) | Streak: 12 days*
