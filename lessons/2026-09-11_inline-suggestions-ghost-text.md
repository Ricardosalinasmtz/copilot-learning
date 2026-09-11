# 🎓 Copilot Lesson: Inline Suggestions and Ghost Text
**Date:** 2026-09-11 | **Day:** 1 of learning streak
**Source:** https://docs.github.com/en/copilot/using-github-copilot/getting-code-suggestions-in-your-personal-developer-environment/about-inline-code-suggestions | **Difficulty:** Beginner

## 🎯 What You'll Learn

You'll learn how to recognize, read, and interact with GitHub Copilot's inline code suggestions (ghost text) in VSCode. This is the most fundamental Copilot feature — the one you'll use dozens of times per day.

## 📖 The Concept

**Inline suggestions** (also called "ghost text") are code completions that appear directly in your editor as you type. They look like faded, gray text that extends your current line or adds new lines below your cursor.

When Copilot detects what you're trying to write, it predicts the next logical code and displays it inline. You can:
- **Accept** the suggestion (usually with Tab key)
- **Reject** it (keep typing or press Esc)
- **Accept partially** (use cursor keys to select only part of the suggestion)

The ghost text is non-intrusive — it doesn't modify your code until you explicitly accept it. Think of it as Copilot "whispering" suggestions while you remain in full control.

## 🔑 Key Takeaways

- Ghost text appears automatically as you type — no need to invoke it
- Press **Tab** to accept the full suggestion
- Press **Esc** or keep typing to reject it
- Use **Ctrl+→** (Windows/Linux) or **Cmd+→** (Mac) to accept word-by-word
- You remain in control — ghost text never commits without your action

## 💡 Practical Example

**Scenario:** You're writing a function to calculate the average of an array:

```javascript
function calculateAverage(numbers) {
  // You start typing:
  const sum = numbers.reduce((acc, num) => acc + num, 0);
  
  // Copilot might suggest:
  const average = sum / numbers.length;
  return average;
}
```

As you type `const sum = ...`, Copilot recognizes the pattern and suggests the next logical lines. You can:
- Press **Tab** to accept both lines
- Press **Ctrl+→** to accept `const average = sum / numbers.length;` first, then review the return statement
- Reject and write your own version

## 🔗 Related Concepts

- Accepting and Rejecting Suggestions (Lesson 5)
- Natural Language to Code in Comments (Lesson 6)
- Copilot Chat Interface Basics (Lesson 2)

## 🤔 Quick Check

**Question:** You're typing a function and Copilot suggests 3 lines of code, but you only want the first 2 lines. What's the most efficient way to accept only part of the suggestion?

<details>
<summary>Click for answer</summary>

**Answer:** Use **Ctrl+→** (Windows/Linux) or **Cmd+→** (Mac) to accept word-by-word or line-by-line until you've accepted the first 2 lines, then continue typing to reject the rest.
</details>

---
*Lesson 1 of Phase 1 | Streak: 1 day*
