# 🎓 Copilot Lesson: Prompt Engineering for Copilot Chat

**Date:** 2026-09-22 | **Day:** 8 of learning streak
**Source:** [Prompt Engineering for GitHub Copilot Chat](https://docs.github.com/en/copilot/concepts/prompting/prompt-engineering) | **Difficulty:** intermediate

## 🎯 What You'll Learn

How to structure your requests to GitHub Copilot so it gives you better, more accurate code — without endless back-and-forth. You'll master the core strategies that turn vague prompts into precise results.

## 📖 The Concept

A **prompt** is any request you make to Copilot — a question in Copilot Chat, a comment asking for code, or even a file you reference with `@workspace`. Copilot doesn't just read your words; it uses surrounding context (open files, chat history, highlighted code) to generate a response. The quality of that response depends heavily on the quality of your prompt.

GitHub's official guidance boils down to several key strategies. **Start general, then get specific:** give Copilot the broad goal first, then list concrete requirements. Instead of just "write a function," say what the function should do, what it takes as input, and what it should return. **Give examples** — actual input/output pairs help Copilot understand edge cases far better than abstract descriptions. Even unit tests can serve as examples; write tests first, then ask Copilot to implement the function.

**Break complex tasks into smaller ones.** Don't ask Copilot to build a whole app in one prompt. Ask for one function, then the next, then wire them together. This also makes it easier to catch mistakes and iterate. **Avoid ambiguity** — instead of "what does this do?", specify exactly which function, file, or code block you're asking about. **Indicate relevant code** by opening relevant files, closing irrelevant ones, or using `@workspace` / `@project` to supply context explicitly.

## 🔑 Key Takeaways

- **General → Specific:** Start with the goal, then add requirements and constraints
- **Examples beat descriptions:** Show input/output pairs, even for edge cases
- **Chunk big tasks:** One function per prompt, not a whole module
- **Be explicit about context:** Use `@workspace`, highlight code, name files directly
- **Keep history clean:** Use threads for separate tasks; delete stale context

## 💡 Practical Example

**Bad prompt:**
```
Write a utility for dates.
```

**Good prompt:**
```
Write a Go function that finds all dates in a string and returns them in an array.
Dates can be formatted like:
  - 05/02/24, 05/02/2024
  - 05-02-24, 05-02-2024

Example:
findDates("I have a dentist on 11/14/2023 and book club on 12-1-23")
Returns: ["11/14/2023", "12-1-23"]
```

**Even better** (iterated):
```
1. First, write unit tests for findDates covering all the date formats above.
2. Then, implement findDates() to pass those tests.
```

## 🔗 Related Concepts

- [Slash Commands Overview](/copilot/reference/chat-cheat-sheet) — @workspace, @edit, @vscode terminals
- [Context Management with @ Symbols](/copilot/concepts/context) — how Copilot gathers context
- [Custom Instructions for Copilot](/copilot/concepts/prompting/response-customization) — personalizing responses
- [Prompting GitHub Blog Guide](https://github.blog/2023-06-20-how-to-write-better-prompts-for-github-copilot/)

## 🤔 Quick Check

You want Copilot to refactor a 200-line function into smaller, testable pieces. Your first prompt is: "Refactor this function." Copilot gives you a mess. Using today's strategies, what's the **first thing** you should change about your approach?

*(Answer: Break it into smaller tasks — ask for one extracted function at a time, specify what each new function should do, and use `@workspace` to give Copilot full context of the file.)*

---
*Lesson 8 of Phase 2 (VSCode Integration) | Streak: 8 days*
