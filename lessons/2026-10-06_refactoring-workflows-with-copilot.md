# 🎓 Copilot Lesson: Refactoring Workflows with Copilot
**Date:** 2026-10-06 | **Day:** 14 of learning streak
**Source:** https://code.visualstudio.com/docs/chat/inline-chat | **Difficulty:** intermediate

## 🎯 What You'll Learn

Today we look at how VS Code's inline chat turns the editor itself into a refactoring workbench: select code, describe the change in plain language, and review the result as a diff — without ever leaving the file you're editing.

## 📖 The Concept

Inline chat (Ctrl+I, or Cmd+I on macOS) is the no-context-switch way to talk to Copilot. Open it in a file and your prompt is scoped to the code in the active editor, with Copilot free to pull in other workspace files when they seem relevant. The key refactoring trick: select a block of code first, and the prompt is scoped to that exact selection. That makes it ideal for targeted refactors — extracting a method, renaming a concept across a function, converting a loop to a functional style, or reworking error handling.

The result never lands silently. Copilot shows an inline diff, and you either Keep or Undo it. Each step of a multi-step refactor is therefore reviewable and reversible before anything is committed, and you can simply re-prompt to iterate until the change is right.

One subtlety worth knowing: if a file belongs to an active Copilot editing session (an agent-driven change already in progress), pressing Ctrl+I on that file opens "Ask in Chat" instead — your prompt gets routed into the existing session so it carries the full conversation context. That's handy for steering an agent mid-refactor, but if you want plain inline chat even on those files, set `inlineChat.askInChat` to `false`.

## 🔑 Key Takeaways

- **Ctrl+I (Cmd+I) opens inline chat in the active editor** — select code first to scope the prompt to that exact block.
- **Edits arrive as a diff with Keep/Undo** — review and accept or reject before the change sticks.
- **Files in an active agent editing session route to "Ask in Chat"** so you can steer the session; `inlineChat.askInChat: false` forces regular inline chat.
- **`inlineChat.defaultModel` sets the model for inline chat** — a model change made mid-session persists until you reload VS Code.

## 💡 Practical Example

Extract a helper from an existing function, no mode switch:

1. Open `orderService.ts` and select the block that computes discounts.
2. Press **Ctrl+I** and type: `Extract this into a private helper function called applyDiscounts with a unit-testable signature`.
3. Copilot shows the diff: the inlined logic becomes an `applyDiscounts(...)` call and the new private method appears below.
4. **Keep** the change, then select the new helper, Ctrl+I, and type `Add JSDoc with parameter descriptions` — **Keep** again.
5. Verify with your test suite, or pop Quick Chat (Ctrl+Shift+Alt+L) and ask "did I miss any callers of the old inline logic?"

If the refactor spans several files, that's the point to switch to the Chat view and let an agent editing session handle the cross-file changes — inline chat is the tool for surgical, in-file edits.

## 🔗 Related Concepts

- [Chat view in VS Code](https://code.visualstudio.com/docs/chat/chat-overview)
- [Adding context to your chat prompt (@-mentions)](https://code.visualstudio.com/docs/chat/copilot-chat-context)
- [Reviewing AI-generated code edits](https://code.visualstudio.com/docs/agents/run/review-code-edits)
- [Choosing the right model](https://code.visualstudio.com/docs/agents/concepts/language-models)

## 🤔 Quick Check

You're mid-refactor: an agent editing session is active on `parser.ts`, and you want a small standalone edit right there — tightening one condition — but Ctrl+I keeps routing your prompt into the agent's session via "Ask in Chat" instead of doing a plain inline edit. Which setting changes this behavior, and what should you set it to?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Set `inlineChat.askInChat` to `false`. By default, files that belong to an active chat editing session route Ctrl+I into that session so the prompt keeps the conversation context; flipping the setting off makes regular inline chat open on the file so your edit stays standalone and scoped to your selection.

</details>

---
*Lesson 14 of Phase 2 (VSCode Integration) | Streak: 14 days*
