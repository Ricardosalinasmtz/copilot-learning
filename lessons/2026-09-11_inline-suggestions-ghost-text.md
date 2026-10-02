# 🎓 Copilot Lesson: Inline Suggestions and Ghost Text

**Date:** 2026-09-11 | **Day:** 1 of learning streak
**Source:** GitHub Copilot Documentation | **Difficulty:** Beginner

## 🎯 What You'll Learn

You'll learn how to recognize, read, and interact with GitHub Copilot's inline code suggestions (ghost text) in VSCode. This is the foundational feature you'll encounter every time you use Copilot—understanding how to work with these suggestions efficiently is essential for productive AI-assisted coding.

## 📖 The Concept

**Inline suggestions** (often called "ghost text") are Copilot's real-time code completions that appear directly in your editor as you type. They show up as semi-transparent, grayed-out text that extends your current line or adds new lines below your cursor.

Ghost text is Copilot's way of saying "I think this is what you want to write next." It analyzes your current file, the code you've already written, comments, function names, and even patterns from your entire workspace to predict what comes next. The suggestions appear automatically—you don't need to ask for them.

The key insight: **ghost text is a suggestion, not a command**. You're always in control. You can accept it, reject it, ignore it, or type something completely different. Learning to quickly evaluate whether a suggestion is useful—and acting on that evaluation—is the core skill that separates efficient Copilot users from frustrated ones.

## 🔑 Key Takeaways

- **Ghost text appears automatically** as you type or pause; no special key or command needed
- **It's contextual**: Copilot reads your current file, comments, function names, and workspace patterns
- **You're in control**: Always choose whether to accept, reject, or ignore each suggestion
- **Speed matters**: Quick evaluation (useful or not?) + quick action (tab or escape) keeps your flow
- **Position matters**: Ghost text appears at your cursor position—place your cursor strategically

## 💡 Practical Example

**Scenario:** You're writing a Python function to calculate the average of a list:

```python
def calculate_average(numbers):
    # Calculate the sum and divide by count
```

As you type the comment, Copilot might show ghost text like:

```python
def calculate_average(numbers):
    # Calculate the sum and divide by count
    total = sum(numbers)
    return total / len(numbers)
```

**What to do:**
1. **Pause and read** the ghost text (takes ~1 second)
2. **Evaluate**: Is this correct? Is this what I want?
3. **Accept**: Press `Tab` to accept the entire suggestion
4. **Or reject**: Press `Esc` or keep typing to dismiss it
5. **Or partial accept**: Some editors let you accept word-by-word with `Ctrl+Right Arrow`

**Pro tip:** If you find yourself constantly rejecting suggestions for a specific pattern, try writing a more specific comment or function name to guide Copilot better.

## 🔗 Related Concepts

- [Accepting and Rejecting Suggestions](about:blank) - Keyboard shortcuts and workflows
- [Natural Language to Code in Comments](about:blank) - Writing comments that trigger better suggestions
- [Copilot Chat Interface Basics](about:blank) - When to use chat instead of inline suggestions

## 🤔 Quick Check

**Reflection:** Think about your current coding workflow. When you're in the middle of writing a function, do you typically:
- A) Type everything yourself without noticing suggestions
- B) Stop and read every ghost text suggestion carefully
- C) Glance at suggestions and accept/reject quickly
- D) Find ghost text distracting and wish you could disable it

*There's no wrong answer—but option C is the target skill. You want to develop a rhythm where you notice suggestions, evaluate them in under a second, and act without breaking your flow.*

---
*Lesson 1 of Phase 1 (Core Copilot Features) | Streak: 1 day*
