# 🎓 Copilot Lesson: Custom Agents (Agent Profiles for Shared, Team-Specific Workflows)
**Date:** 2026-10-13 | **Day:** 19 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-custom-agents | **Difficulty:** advanced

## 🎯 What You'll Learn

Custom agents are specialized versions of the Copilot agent that you define once and reuse across the team — like tailored teammates who already know your conventions, tools, and workflows. Today you'll learn how agent profiles work: the Markdown + YAML format, where to store them (repo, organization, enterprise), and where they can run (GitHub.com, IDEs, the Copilot app, and the CLI).

## 📖 The Concept

Custom agents solve a repetition problem. Without them, you keep pasting the same instructions and context into every Copilot task that touches a particular area of your codebase. A custom agent is a specialized version of the Copilot agent tailored to your unique workflows, coding conventions, and use cases. It acts like a teammate that follows your standards, uses the right tools, and implements team-specific practices — you define it once, then assign it to tasks and issues instead of re-providing the same instructions every time.

An agent is defined with a Markdown file called an **agent profile**, which uses YAML frontmatter. The frontmatter holds a `name` (optional — if omitted, the filename becomes the identifier and display name), a `description` (explains the agent's purpose and capabilities), a `prompt` (the custom instructions defining its behavior and expertise), and optional `tools` (specific tools the agent can access — by default it can use all available tools, including built-in tools and MCP server tools). Profiles can also declare `mcp-servers` to give the agent its own MCP configurations. When you assign the agent to a task or issue, the profile is instantiated into a running agent.

Profiles are scoped by where you store them, following the same hierarchy you've seen with custom instructions. Repository level: create `.github/agents/CUSTOM-AGENT-NAME.md` for project-specific agents. Organization level: create `/agents/CUSTOM-AGENT-NAME.md` in the organization's `.github` or `.github-private` repository so the agent is available org-wide. Enterprise level: it lives in the `.github-private` repository of an organization that an enterprise owner has designated in enterprise settings.

Once created, the agent becomes available across several surfaces: the Copilot cloud agent on GitHub.com (agents tab and panel, issue assignment, pull requests), the cloud agent in IDEs (Visual Studio Code, JetBrains IDEs, Eclipse, and Xcode — the latter three in public preview), the GitHub Copilot app, and the GitHub Copilot CLI. The same profile works in all of these environments, though some properties may behave differently or be ignored depending on where the agent runs.

## 🔑 Key Takeaways

- Agent profiles are Markdown files with YAML frontmatter: `name` (optional), `description`, `prompt`, plus optional `tools` and `mcp-servers`.
- Define once, assign many times — the profile is instantiated when the agent is assigned to a task or issue, so you stop pasting the same instructions.
- Scoping is hierarchical: repo (`.github/agents/`), organization (org `.github` repo's `/agents/`), or enterprise (a designated `.github-private` repo).
- One profile works across GitHub.com (issues, PRs, agents tab), IDEs, the Copilot app, and the CLI.
- The `tools` field narrows the agent to specific tools; by default it has all built-in and MCP tools, so restrict for focus and safety.

## 💡 Practical Example

The docs' example profile, `readme-creator`, shows the minimal shape:

```markdown
---
name: readme-creator
description: Agent specializing in creating and improving README files
---

You are a documentation specialist focused on README files. Your scope is limited to README files or other related documentation files only - do not modify or analyze code files.

Focus on the following instructions:
- Create and update README.md files with clear project descriptions
- Structure README sections logically: overview, installation, usage, contributing
- Write scannable content with proper headings and formatting
- Add appropriate badges, links, and navigation elements
- Use relative links (e.g., `docs/CONTRIBUTING.md`) instead of absolute URLs for files within the repository
- Make links descriptive and add alt text to images
```

Save that file as `.github/agents/readme-creator.md` in your repo, and any task or issue you assign to it gets a documentation specialist that already knows your team's README standards — no instructions to paste. To build your own, start with [Creating custom agents for Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/create-custom-agents).

## 🔗 Related Concepts

- [Custom agents in VS Code](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
- [Creating custom agents for Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/create-custom-agents)
- [Model Context Protocol (MCP) and GitHub Copilot](https://docs.github.com/en/copilot/concepts/context/mcp)
- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet)

## 🤔 Quick Check

Your team maintains a REST API with strict conventions — a fixed error-response shape, a specific pagination style, and naming rules that you currently paste into the prompt every time you ask Copilot to change an endpoint. How would you set up a custom agent so the conventions apply automatically, and where would you place the profile so every teammate working in the repository benefits?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Create a Markdown agent profile (e.g., `.github/agents/api-maintainer.md`) whose `description` says it handles the team's REST API work and whose `prompt` encodes the conventions — the error-response shape, pagination style, and naming rules. Save it in the repository's `.github/agents/` folder so it's available to everyone working in that repo, then assign that agent to API-related issues and tasks. Because the profile is instantiated on assignment, the conventions apply every time without pasting them into the prompt.

</details>

---
*Lesson 19 of Phase 3 (Advanced Workflows) | Streak: 19 days*
