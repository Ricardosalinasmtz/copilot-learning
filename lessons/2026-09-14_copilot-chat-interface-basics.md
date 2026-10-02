# 🎓 Copilot Lesson: Copilot Chat Interface Basics
**Date:** 2026-09-14 | **Day:** 2 of learning streak
**Source:** [About GitHub Copilot Chat](https://docs.github.com/en/copilot/concepts/chat) | **Difficulty:** Beginner

## 🎯 What You'll Learn

GitHub Copilot Chat is the conversational AI interface bundled with Copilot — it's how you ask questions, get code suggestions, request explanations, and generate tests in a natural chat format. This lesson covers what it is, where you can use it, and the key concepts behind its design.

## 📖 The Concept

GitHub Copilot Chat is not a standalone product — it's the chat-powered layer of GitHub Copilot that works across multiple environments. Think of it as your AI pair programmer that you can talk to instead of just waiting for ghost text suggestions.

### Where It Lives

Copilot Chat runs in several places:

- **Visual Studio Code** — The most common use case; accessible via the Copilot chat panel (Cmd+I / Ctrl+I for inline chat, or the chat icon in the sidebar)
- **GitHub.com** — Open Copilot Chat from any repository, PR, or issue page
- **JetBrains IDEs** — Available through the Copilot plugin
- **Xcode** — Apple's IDE support
- **GitHub Mobile** — On-the-go coding assistance
- **GitHub Copilot CLI** — Terminal-based AI assistant
- **GitHub Copilot app** — The dedicated desktop agent application

The core functionality is consistent across all environments, though some environments offer additional features.

### What It Can Do

Copilot Chat helps you with coding-related tasks:

- **Code suggestions** — Propose implementations for your current task
- **Code explanation** — Describe what a piece of code does and why
- **Unit test generation** — Create tests for your existing functions
- **Bug fixing** — Propose fixes for errors in your code
- **Natural language to code** — Describe what you want in plain English

### The Context Passing Feature

An important concept: Copilot Chat and the **Copilot Cloud Agent** can share context. When you start an agent session from a chat, the agent inherits your conversation context. While the agent runs, you can continue chatting with Copilot about its progress — it can even answer questions about pull requests the agent created by pulling in session logs.

This is different from **Copilot Memory**, which builds long-term persistent understanding of your repositories and preferences across sessions.

### Custom Instructions

Instead of repeating instructions in every prompt, you can create saved custom instructions:

- **Personal instructions** — Apply to all your chat responses (set in your GitHub settings)
- **Repository instructions** — Stored in a `.github/copilot-instructions.md` file; automatically included for any prompt in that repo
- **Organization instructions** — Admin-defined; apply across all repos in an organization

This means you can set project-wide coding standards, preferred libraries, or team conventions once, and Copilot Chat will apply them every time.

### AI Models

Copilot Chat lets you switch between different AI models. Some models excel at certain types of questions — code generation might work better with one model while code review works better with another. Premium models with advanced capabilities are also available.

## 🔑 Key Takeaways

- Copilot Chat is a **conversational interface** available across VSCode, JetBrains, GitHub.com, mobile, CLI, and more — the core features work everywhere
- It handles code suggestions, explanations, test generation, and bug fixes through natural language
- **Context passing** between Chat and Cloud Agent sessions lets you iterate on agent work without losing conversation thread
- **Custom instructions** (personal, repository, organization) let you tailor responses without repeating yourself
- You can **swap AI models** to find the best fit for different types of questions

## 💡 Practical Example

**In VSCode, try these workflows:**

1. **Open Copilot Chat** — Click the Copilot icon in the activity bar, or press the keyboard shortcut for the chat panel.

2. **Ask about your code** — Open a file and type something like:
   ```
   @Workspace Explain what this function does
   ```
   The `@Workspace` mention tells Copilot to include your current project files as context.

3. **Get a code review** — Open a pull request on GitHub.com and ask:
   ```
   Review this PR and suggest improvements for the authentication module
   ```

4. **Generate tests** — Highlight a function in VSCode and ask:
   ```
   Generate unit tests for this function using Jest
   ```

5. **Switch models** — Click the model selector in the chat panel to try a different AI model and compare how it handles the same question.

## 🔗 Related Concepts

- [Code Suggestions & Ghost Text](/en/copilot/concepts/completions/code-suggestions) — The non-chat side of Copilot
- [Copilot Cloud Agent](/en/copilot/concepts/agents/cloud-agent/about-cloud-agent) — Autonomous agent that can make code changes on branches
- [Copilot Memory](/en/copilot/concepts/agents/copilot-memory) — Long-term repository and preference learning
- [Model Context Protocol (MCP)](/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp) — Extending Chat with external tools and data sources
- [Custom Instructions](/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/) — Personal, repository, and organization-level instruction files

## 🤔 Quick Check

You're working on a Python project and want Copilot Chat to always follow PEP 8 style guidelines and prefer `pytest` over `unittest` for testing. Instead of typing these requirements into every prompt, what's the best way to set this up so it applies automatically?

---
*Lesson 2 of Phase 1 (Core Copilot Features) | Streak: 2 days*
