# 🎓 Copilot Lesson: Copilot in Integrated Terminal

**Date:** 2026-09-21 | **Day:** 7 of learning streak
**Source:** [GitHub Copilot CLI Docs](https://docs.github.com/en/copilot/how-tos/copilot-cli) | **Difficulty:** intermediate

## 🎯 What You'll Learn

How to use GitHub Copilot directly from your terminal via Copilot CLI — turning your command line into an AI-powered coding workspace with tool access, custom agents, and session management.

## 📖 The Concept

Copilot CLI lets you run Copilot directly from your terminal. Instead of opening VSCode's Copilot chat panel, you get an interactive session in your shell where Copilot can read, modify, and execute files, run tests, build projects, and even browse GitHub issues and pull requests — all without leaving the terminal.

When you start a Copilot CLI session by typing `copilot` in a folder, it asks you to confirm that you trust the files in that location. This is a security boundary: during the session, Copilot may attempt to read, modify, and execute files and run shell commands. You approve each tool use individually, or you can approve a tool type for the rest of the session.

### Key Capabilities

**Plan Mode:** Press `Shift+Tab` to enter plan mode, where Copilot collaborates on an implementation plan before writing any code. This is useful for complex tasks where you want to agree on an approach first.

**Custom Agents:** Copilot CLI ships with built-in custom agents for common tasks:

- **Explore** — Quick codebase analysis without cluttering main context
- **Task** — Run tests and builds, summarizing success or showing full output on failure
- **General Purpose** — Handles complex multi-step tasks in a separate context
- **Code Review** — Reviews changes while minimizing noise
- **Research** — Deep research across your codebase and the web with citations
- **Rubber Duck** — Acts as a constructive critic (used automatically behind the scenes)

You can also define your own custom agents using Markdown agent profiles stored in `.github/agents/` (repository-level), `~/.copilot/agents/` (user-level), or `/agents/` in `.github-private` (organization-level).

**Tool Approval:** Copilot asks permission before using any tool that modifies or executes files (e.g., `touch`, `chmod`, `node`, `sed`). You can approve once per command, approve a tool for the rest of the session, or reject and give inline feedback for Copilot to adapt.

**Context Management:** Copilot CLI automatically compresses conversation history when approaching the token limit (starting at 80%, or 90% if static context is already large). You can also manually compress with `/compact`, view usage with `/usage`, or inspect your current context with `/context`.

**Voice Input:** You can speak prompts instead of typing — useful for quick questions or brainstorming while your hands are on the keyboard.

**Attachments:** You can attach images and PDFs (JPEG, PNG, GIF, WEBP, PDF, HEIC, HEIF) to prompts via `@path`, drag-and-drop, or paste from clipboard.

**Sandboxing:** Two levels of isolation are available:
- **Local sandbox** (`/sandbox enable`): Restricts the commands and tools Copilot can access to your filesystem, network, and system capabilities
- **Cloud sandbox** (`copilot --cloud`): Runs the entire session remotely in an isolated environment

## 🔑 Key Takeaways

- Copilot CLI turns your terminal into a full AI-powered coding environment with file read/write/execute capabilities
- Use Plan Mode (`Shift+Tab`) for complex tasks — agree on the approach before any code is written
- Built-in agents (Explore, Task, Code Review, Research) let Copilot delegate to specialized subprocesses automatically
- Custom agents defined in `.github/agents/` let you tailor Copilot's behavior for your project's conventions
- Every tool modification asks for your approval — you can approve per-command or for the whole session
- Context compression happens automatically at 80% token usage, and you can inspect/compact manually
- Sandboxing (local or cloud) gives you confidence when letting Copilot run freely on sensitive code

## 💡 Practical Example

### Starting a session and using plan mode:

```shell
# Navigate to your project
cd ~/projects/my-app

# Start Copilot CLI
copilot
# → Do you trust the files in this folder? [Y/n] Yes

# Enter plan mode
# Press Shift+Tab → "Entering plan mode..."

# Propose a plan
I need to add a new REST endpoint for user authentication.
Plan the implementation.

# Copilot responds with a plan — review and approve, then press Shift+Tab
# to exit plan mode and start coding
```

### Using custom agents:

```shell
# Run code review agent on staged changes
/copilot review

# Or delegate to Research agent for deep analysis
Use the Research agent to analyze our error handling patterns
across the codebase and produce a report with citations.
```

### Managing context in a long session:

```shell
# Check current token usage
/context

# See session statistics (credits, duration, lines edited)
/usage

# Manually compress conversation history
/compact

# View all available slash commands
?
```

### Scheduling automated tasks:

```shell
# Run frontend tests every hour
/every 1h Run frontend tests and report any failures

# Run once after 30 minutes
/after 30m Deploy staging build and run smoke tests
```

## 🔗 Related Concepts

- [Natural Language to Code in Comments](../lessons/2026-09-18_natural-language-to-code-in-comments.md) — The chat interface concept that extends to terminal
- [Copilot Memory](https://docs.github.com/en/copilot/concepts/agents/copilot-memory) — Repository facts and user preferences that Copilot CLI can leverage
- [Custom Instructions for Copilot](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide) — `.github/copilot-instructions.md` works with CLI too
- [Copilot CLI Reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference) — Complete command and shortcut reference

## 🤔 Quick Check

> You're starting a Copilot CLI session on a sensitive production codebase. You want Copilot to be able to read files and suggest changes, but you don't want it to execute anything until you've reviewed the changes. Which approach gives you the most control?
>
> A) Start with `copilot --yolo` to save time on approvals
> B) Start normally, review each tool approval carefully, and use `/sandbox enable`
> C) Use `/sandbox enable` immediately after starting, then review each tool approval
> D) Don't use CLI for production — it's not safe
>
> **Answer:** C — Sandboxing restricts what Copilot can access (filesystem, network, system), and you still review each tool approval. This gives you both isolation and explicit control.

---

*Lesson 7 of Phase 2 (VSCode Integration) | Streak: 7 days*
