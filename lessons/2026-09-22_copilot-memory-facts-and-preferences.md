# 🎓 Copilot Lesson: Copilot Memory (Repository Facts and User Preferences)
**Date:** 2026-09-22 | **Day:** 9 of learning streak
**Source:** [Copilot Memory Docs](https://docs.github.com/en/copilot/concepts/agents/copilot-memory) | **Difficulty:** advanced

## 🎯 What You'll Learn

Copilot Memory is a feature that lets GitHub Copilot learn and remember facts about your codebase and your personal coding preferences over time. Instead of repeating the same context in every prompt, Copilot builds its own understanding of your projects — saving you effort and improving its accuracy.

## 📖 The Concept

### Why Copilot Needs Memory

When you start working on a new codebase, you typically read the README, coding conventions, and architecture docs before you contribute. Over time, you absorb unwritten norms — naming patterns, where error handling lives, how configs are structured. Copilot is the same: without memory, it's stateless, treating every interaction as a blank slate. You'd have to constantly re-explain your project's conventions.

**Copilot Memory solves this** by storing two kinds of information:

1. **Repository-level facts** — Facts about the codebase itself (conventions, build commands, architectural decisions). These are shared with all users who have Copilot Memory enabled in that repository.
2. **User-level preferences** — Your personal coding style and workflow preferences (e.g., "prefer `const` over `let`", "use TypeScript interfaces over types"). These are private to you and travel across all repositories.

### How It Works

Copilot doesn't just memorize blindly. It stores facts **with citations** — references to the actual code that supports each fact. Before using a stored fact, Copilot cross-checks it against the current branch to ensure it's still accurate. If a fact hasn't been used for 28 days, it's automatically deleted to prevent stale knowledge.

Facts are only created in response to actions by users with **write access** to the repository, which prevents unauthorized users from polluting shared facts.

### Where Memory Is Used

Memory isn't just for chat — it feeds into multiple Copilot features:

- **Copilot Cloud Agent** — Uses both repository facts and user preferences
- **Copilot Code Review** — Uses only repository-level facts (not personal preferences)
- **Copilot CLI** — Uses repository facts and the user's own preferences

If the cloud agent learns how your repo handles database connections, code review can later apply that knowledge to spot inconsistencies in a pull request.

### Managing Memory

- **Repository owners** can review and delete repository-level facts for their project.
- **Every user** can view and delete their own user-level preferences.
- **Admins** (Business/Enterprise plans) can export or delete user preferences in bulk.
- To use memory, you need to have **Copilot Memory enabled** in your settings. Individual plans have it on by default; enterprise/organization plans require admin enablement.

## 🔑 Key Takeaways

- **Memory reduces repetition** — Stop explaining the same project conventions in every prompt
- **Facts are cited** — Copilot stores facts with code references and validates them before use
- **28-day auto-cleanup** — Unused facts are automatically removed to prevent stale information
- **Scope is enforced** — Repository facts stay within that repo; user preferences stay tied to the user
- **Multi-feature** — Memory powers Copilot Chat, Code Review, Cloud Agent, and CLI

## 💡 Practical Example

Imagine you're working on a Python project that follows specific conventions. Without memory, every time you ask Copilot for help, you'd need to say something like:

> "In this project, we use `src/` for source code, `tests/` for tests, type hints are required, and all API responses follow the {success, data} pattern."

With Copilot Memory enabled, Copilot learns these conventions from your prompts and subsequent interactions. On your next request, it automatically applies that knowledge without you re-explaining:

```python
# You write a function body
async def get_user(user_id: int):
    # Copilot knows to use the {success, data} pattern and type hints

# Later, in Code Review:
# Copilot flags: "This response doesn't match the project's standard {success, data} pattern"
```

### Managing Your Memory

You can view and manage your stored preferences at:
- **Personal settings:** https://github.com/settings/copilot/memory

Repository owners can manage repository-level facts through their project's Copilot settings.

## 🔗 Related Concepts

- [Custom Instructions for Copilot](https://docs.github.com/en/copilot/concepts/prompting/response-customization) — Manual overrides that complement memory
- [Context Management with @ Symbols](https://docs.github.com/en/copilot/concepts/context) — Direct context injection alongside remembered context
- [Copilot Spaces](https://docs.github.com/en/copilot/concepts/context/spaces) — Organized context collections
- [Agent Hooks for Copilot](https://docs.github.com/en/copilot/concepts/agents/hooks) — Automation triggers for custom workflows

## 🤔 Quick Check

Copilot Memory stores facts with citations and validates them before use. Given this design, what happens if a developer refactors their codebase and changes a convention that Copilot has memorized? Would Copilot automatically detect the change, or could it continue applying stale facts? Think about the validation process described in the documentation.

---
*Lesson 9 of Phase 2 (VSCode Integration) | Streak: 9 days*
