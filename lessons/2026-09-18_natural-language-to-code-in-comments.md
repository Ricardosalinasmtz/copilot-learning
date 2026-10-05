# 🎓 Copilot Lesson: Natural Language to Code in Comments
**Date:** 2026-09-18 | **Day:** 6 of learning streak
**Source:** [Getting code suggestions in your IDE](https://docs.github.com/en/copilot/how-tos/get-code-suggestions/get-ide-code-suggestions) | [Code suggestions in your IDE](https://docs.github.com/en/copilot/concepts/completions/code-suggestions) | **Difficulty:** Beginner

## 🎯 What You'll Learn

How to use natural language comments as prompts for GitHub Copilot to generate code — turning your intentions directly into working implementations without writing a single line of syntax first.

## 📖 The Concept

Copilot doesn't just complete the code you type — it understands the code you *describe*. When you write a comment in natural language that describes what you want to achieve, Copilot analyzes that description and suggests an implementation. This bridges the gap between intent and code, letting you think in terms of *what* you want rather than *how* to write it.

In VS Code (your environment), this works as ghost text suggestions triggered by comments. Type a comment describing your goal, then start writing code beneath it. Copilot will propose a solution in grayed text that you can accept with **Tab**, reject with **Esc**, or cycle through alternatives using **Alt+]** / **Alt+[\** (macOS: **Option+]** / **Option+[\**).

This works across 28+ programming languages including Python, JavaScript, TypeScript, Java, Go, C#, Rust, Ruby, Shell, SQL, and many more. Copilot matches the style and context of your existing codebase, so suggestions blend naturally with your project.

### How It Works

1. **Write a descriptive comment** — Be specific about the behavior you want, not the implementation details.
2. **Start typing below** — Begin a function, variable, or block under the comment.
3. **Copilot suggests** — Gray ghost text appears with a suggested implementation.
4. **Accept, reject, or iterate** — Tab to accept, Esc to reject, or press alternative suggestion keys to see other options.

You can also write the comment and let Copilot fill in the entire function body, or write a partial implementation and have Copilot complete it based on the comment's guidance.

## 🔑 Key Takeaways

- **Write comments in natural language** — Describe *what* you want to accomplish, not *how* to code it. Copilot handles the translation.
- **Be specific but not prescriptive** — "find all images without alternate text and give them a red border" works better than just "process images" but you don't need to specify the exact algorithm.
- **Accept partial suggestions** — You don't have to accept the entire suggestion. Use **Cmd+→** (macOS) or **Ctrl+→** (Windows/Linux) to accept word-by-word, or hover to accept line-by-line.
- **Cycle through alternatives** — Copilot may offer multiple suggestions. Use **Alt+]** / **Alt+[\** to browse them before deciding.
- **Open all suggestions in a new tab** — Press **Cmd+Shift+A** (macOS) or **Ctrl+Enter** (Windows/Linux) to see a full panel of alternative implementations.
- **Duplication detection affects suggestions** — If suggestions seem limited, you may have "Suggestions matching public code" enabled, which filters out code that matches publicly available sources.

## 💡 Practical Example

Here's how you'd use comment-driven code generation in VS Code:

```python
# Calculate the moving average of a list of numbers
# over a sliding window of n elements
# Return empty list if input is shorter than n
def moving_average(data: list[float], window: int) -> list[float]:
```

As soon as you type that comment and press Enter below it, Copilot will suggest something like:

```python
    if len(data) < window:
        return []
    return [
        sum(data[i:i+window]) / window
        for i in range(len(data) - window + 1)
    ]
```

You press **Tab** to accept, and the implementation is inserted. You can then refine it further with additional comments if needed:

```python
# Add logging to track when the window shifts
```

### Real-World Example: Data Processing

```javascript
// Parse the CSV string and return an array of objects
// where keys are the header row and values are cell data
function parseCSV(csvString) {
```

Copilot will generate a complete `parseCSV` function based on your comment's description.

### VS Code Tip

You can also use this for documentation comments. In Visual Studio, Copilot can auto-generate documentation comments (like `///` for C# or `"""` for Python docstrings) based on the code you've written — analyzing your implementation and suggesting descriptive comments that explain what the code does.

## 🔗 Related Concepts

- [Inline Suggestions and Ghost Text](2026-09-11_inline-suggestions-ghost-text.md) — The foundation that comment suggestions build on
- [Accepting and Rejecting Suggestions](2026-09-17_accepting-and-rejecting-suggestions.md) — How to efficiently review Copilot's output
- [Copilot Chat Interface Basics](2026-09-14_copilot-chat-interface-basics.md) — When to use inline comments vs. chat for code generation
- [Context Management with @ Symbols](2026-09-16_context-management-with-at-symbols.md) — Enhancing comment suggestions with file-specific context

## 🤔 Quick Check

> If you write a comment saying `"sort the array in descending order by the 'age' property"` and Copilot suggests a quicksort implementation, but you actually wanted a one-liner using `Array.sort()`, what are your options to get the simpler solution?

*(Hint: You can be more specific in the comment, reject the suggestion and see alternatives, or edit the comment to guide Copilot differently.)*

---
*Lesson 6 of Phase 1 (Core Copilot Features) | Streak: 6 days*
