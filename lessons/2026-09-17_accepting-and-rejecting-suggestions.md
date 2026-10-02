# 🎓 Copilot Lesson: Accepting and Rejecting Suggestions
**Date:** 2026-09-17 | **Day:** 1 of learning streak
**Source:** GitHub Copilot Documentation | **Difficulty:** Beginner

## 🎯 What You'll Learn

How to interact with Copilot's inline code suggestions using Tab, Shift+Tab, and the arrow keys — and how to fine-tune your workflow with the settings that control suggestion behavior.

## 📖 The Concept

Copilot continuously generates inline suggestions (ghost text) as you type. Learning how to accept, reject, and navigate these suggestions efficiently is fundamental to using Copilot productively.

**Accepting a suggestion** inserts the suggested code into your editor. The most common way is pressing **Tab**, which accepts the entire suggestion in one go. If the suggestion contains multiple possible completions (shown as a list), you can use **Tab** to cycle through them.

**Partially accepting** is useful when you want some of the suggestion but not all of it. Position your cursor anywhere in the ghost text and press **Tab** to accept only up to that point. Alternatively, use **Alt+Right Arrow** (Option+Right Arrow on Mac) to accept the next word.

**Rejecting a suggestion** is just as important. When Copilot suggests something you don't want, press **Escape** to dismiss it instantly. You can also use **Shift+Tab** to move past a suggestion without accepting it — the ghost text stays visible for future suggestions.

**Navigating between suggestions** when multiple options are shown: use **Alt+Right Arrow** (Option+Right Arrow on Mac) to cycle through the suggestion list. Pressing **Enter** or clicking a suggestion in the list will accept it.

**Controlling behavior** — Copilot provides several settings:
- `github.copilot.enable` — Global enable/disable with per-language control
- `editor.inlineSuggest.enabled` — Toggle inline suggestions on/off
- `github.copilot.editor.enableAutoCompletions` — Enable/disable auto-completion mode (Tab-based acceptance)
- `github.copilot.suggest.upstreamDelay` and `downstreamDelay` — Fine-tune timing for when suggestions appear after you type

**Pro tip:** If you find Copilot suggesting too aggressively, increase the upstream delay or use `editor.inlineSuggest.mode: "subword"` to limit suggestions to subword completions, which are less intrusive.

## 🔑 Key Takeaways

- **Tab** accepts the full suggestion; position cursor for partial acceptance
- **Escape** rejects the current suggestion instantly
- **Shift+Tab** dismisses and moves past without accepting
- **Alt+Right Arrow** (Option+Right Arrow on Mac) cycles through multiple suggestion options
- Use VSCode settings to control auto-completion behavior and timing delays
- Subword mode is a good middle ground for developers who want light suggestions

## 💡 Practical Example

```typescript
// Type this and watch Copilot suggest a function:
function calculateTotal

// Tab → accepts: "function calculateTotal() {"
// Shift+Tab → dismisses, you continue typing manually
// Alt+Right Arrow → if multiple suggestions, cycle through them
```

Try this exercise:
1. Create a new file and type `const fetchData = async`
2. Let Copilot generate a suggestion
3. Use Tab to accept all of it
4. Delete part of it and type more to trigger a new suggestion
5. Use Alt+Right Arrow to accept word-by-word through the new suggestion

## 🔗 Related Concepts

- [Inline Suggestions and Ghost Text](../lessons/2026-09-11_inline-suggestions-ghost-text.md) — The previous lesson on how ghost text works
- [Copilot Chat Interface Basics](../lessons/2026-09-14_copilot-chat-interface-basics.md) — The chat-based interaction model
- [Settings Reference](https://code.visualstudio.com/docs/editor/intellisense) — VSCode IntelliSense configuration

## 🤔 Quick Check

> If you're typing and Copilot shows a suggestion, but you want to accept only the first two words of it, what keys do you press?

**Answer:** Position your cursor after the second word in the ghost text and press **Tab** to accept only up to that point.

---
*Lesson 5 of Phase 1 (Core Copilot Features) | Streak: 5 days*
