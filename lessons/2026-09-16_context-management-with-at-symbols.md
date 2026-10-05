# 🎓 Copilot Lesson: Context Management with @ Symbols
**Date:** 2026-09-16 | **Day:** 4 of learning streak
**Source:** GitHub Copilot Chat Documentation | **Difficulty:** Intermediate

## 🎯 What You'll Learn

Copilot's chat doesn't just guess from your open files — you explicitly tell it *what context* to use via `@` prefixes. This lesson covers the context menu system that turns Copilot from a blind suggestion engine into a context-aware coding partner.

## 📖 The Concept

By default, Copilot Chat analyzes your open editor and recent activity to infer context. But "inferred context" is often incomplete or wrong. The `@` context system gives you **precise control** over what information Copilot sees when answering your questions.

Think of `@` symbols as a way to **attach files, symbols, or entire documents** to your chat prompt — like dragging files into a message before hitting send. Each `@` prefix type tells Copilot *where* to look for context.

The most commonly used context types are:

- **`@editor`** — Uses code currently visible in your active editor
- **`@open`** (or `@file`) — Uses all files currently open in tabs
- **`@workspace`** — Uses your entire project workspace
- **`@terminal`** — Includes the terminal output in context
- **`@problems`** — Includes your code problems/warnings/errors panel
- **`@docs`** — Enables Copilot's documentation-awareness (if enabled)
- **`#`** — References a specific symbol or function name for precise context

## 🔑 Key Takeaways

- **`@` is intentional context** — It overrides Copilot's automatic context detection
- **Combine multiple `@` references** — You can attach `@workspace @problems @terminal` in one message
- **`#symbol-name`** is the precision tool — Use it when you need Copilot to focus on a specific function, class, or variable
- **Context changes what Copilot *understands*, not just what it *sees*** — Providing relevant context reduces hallucinations and off-target suggestions
- **Use `@` inline** — Type the `@` in the chat input, start typing the file or symbol name, and select from the autocomplete menu

## 💡 Practical Example

Imagine you're debugging a specific function and want Copilot to suggest a fix:

```
#fixDataParser @workspace @problems

I'm getting a TypeError in the data parser. The problems panel shows
"Cannot read property 'map' of undefined" on line 42. Can you suggest
a fix?
```

This tells Copilot:
1. Focus on the `fixDataParser` function (via `#fixDataParser`)
2. Use your entire workspace as background context
3. Pay attention to the current problems/errors panel

Without the `@` references, Copilot might guess wrong about which file or which error you mean.

Another common pattern — reviewing a specific file:

```
@package.json explain the dependencies I should keep vs remove
```

Here, `@package.json` attaches that file as explicit context so Copilot gives a targeted answer about *your* dependencies, not generic advice.

## 🔗 Related Concepts

- [Slash Commands Overview](#) — `@` context works alongside `/` commands
- [Copilot Chat Interface Basics](#) — The chat panel where `@` is typed
- [Accepting and Rejecting Suggestions](#) — How Copilot uses context to generate suggestions

## 🤔 Quick Check

If you're working on a React component called `UserProfile.tsx` and want Copilot to refactor it to use TypeScript strict mode, which `@` reference would you type to give Copilot the most precise starting point — `@workspace` or `#UserProfile`?

---
*Lesson 4 of Phase 1 (Core Copilot Features) | Streak: 4 days*
