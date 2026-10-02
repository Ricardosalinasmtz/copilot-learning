# 🎓 Copilot Lesson: Custom Instructions for Copilot (Personal, Repository, Organization)
**Date:** 2026-09-30 | **Day:** 10 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/prompting/response-customization | **Difficulty:** intermediate

## 🎯 What You'll Learn

GitHub Copilot can tailor its responses using layered "custom instructions" instead of you retyping the same context every prompt. This lesson covers the three scopes — personal, repository, and organization — how they combine, and how to write instructions that actually work.

## 📖 The Concept

Custom instructions are contextual detail that Copilot silently attaches to every chat request, so you don't have to repeat "we use TypeScript" or "respond in Spanish" in every message. There are three scopes, from narrowest to widest:

- **Personal instructions** — set once in your GitHub Copilot Chat settings on GitHub.com; apply to everything you personally ask, regardless of repo. Good for individual style preferences ("be concise", "answer in Portuguese").
- **Repository custom instructions** — live in the repo itself, so they apply to anyone working in it. There are three flavors: repo-wide (`.github/copilot-instructions.md`), path-specific (`.github/instructions/*.instructions.md`, scoped to matching file paths), and agent instructions (`AGENTS.md`, `CLAUDE.md`, `GEMINI.md` — the same mechanism this very orchestrator relies on).
- **Organization instructions** — set by org owners (Copilot Business/Enterprise only) and apply org-wide, e.g. "always respond in English" or "for security questions, check the internal knowledge base."

All applicable instructions are sent to Copilot together — none silently override the others in terms of *inclusion*. But when instructions conflict, precedence order is: **Personal > Repository (path-specific > repo-wide > agent) > Organization**. In VS Code specifically, there's no personal/org tier — just repository custom instructions plus **prompt files** (`*.prompt.md`), reusable one-off prompt templates you invoke deliberately rather than context injected automatically every time.

## 🔑 Key Takeaways

- Precedence order when instructions conflict: Personal → Repository (path-specific → repo-wide → agent files) → Organization.
- Repository instructions come in three forms: repo-wide (`copilot-instructions.md`), path-specific (`*.instructions.md`), and agent files (`AGENTS.md`/`CLAUDE.md`/`GEMINI.md`) — this workspace's own `AGENTS.md` is literally this pattern in action.
- Effective instructions are short, self-contained, and broadly applicable — avoid asking Copilot to "always check an external styleguide" or "answer in under 1000 characters with short words," since these tend to be ignored or applied inconsistently.

## 💡 Practical Example

A minimal `.github/copilot-instructions.md` for a repo:

```markdown
# Project Overview
Task management web app: React + Node.js + MongoDB.

## Coding Standards
- Use semicolons; single quotes for strings.
- Function-based React components; arrow functions for callbacks.

## UI guidelines
- Support light/dark mode toggle.
```

Any Copilot Chat request in that repo will now silently "know" the stack and style conventions without you restating them.

## 🔗 Related Concepts

- [Copilot Memory (Repository Facts and User Preferences)](2026-09-22_copilot-memory-facts-and-preferences.md) — how Copilot Memory differs from static instruction files
- [Support for different types of custom instructions](https://docs.github.com/en/copilot/reference/custom-instructions-support)

## 🤔 Quick Check

If a repository's `copilot-instructions.md` says "always respond in English" but your personal instructions say "always respond in Portuguese," which one wins — and why does that hierarchy make sense from a security/consistency standpoint?

---
*Lesson 10 of Phase 3 (Advanced Workflows) | Streak: 10 days*
