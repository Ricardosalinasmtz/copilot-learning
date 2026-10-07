# 🎓 Copilot Lesson: Test Generation Patterns with Copilot
**Date:** 2026-10-07 | **Day:** 15 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/guides/test-code-with-ai | **Difficulty:** intermediate

## 🎯 What You'll Learn

Today you'll learn a disciplined workflow for adding tests to existing code with an agent: agree on which behaviors to test first, generate tests without touching implementation, diagnose failures properly, and review assertions before keeping anything.

## 📖 The Concept

Adding tests with AI is easy to get wrong. The tempting shortcut is "generate tests, make them pass, done" — but that produces tests that merely mirror the current code, bugs included. The guide's core idea is separation of concerns: first the agent *inspects* the project and proposes test cases (boundaries, error cases), and you review those cases before any code is written. For example, a `validateUsername` with a 2–20 character limit earns cases at 2, 3, 20, and 21 — at and just outside each limit, which is where off-by-one bugs live.

The second principle: the agent should generate **tests only, not implementation changes**. Keeping the implementation untouched means the new tests can expose real existing bugs instead of being shaped around them. Expected results must be explicit literals, not computed by calling the function under test — otherwise the same bug infects both the actual and the expected value and the test silently passes.

When tests fail, the guide insists on diagnosing before "fixing": a failure can be a test setup problem (bad imports, fixtures), an incorrect expectation, or a genuine implementation bug. Never accept deleted assertions, skipped tests, or tweaked expected values just to get green. After the targeted tests pass, run the related suite to catch interactions with existing tests. Finally, the whole loop becomes repeatable by saving your test commands and quality safeguards in the project's custom instructions, so future sessions follow the same rules without re-prompting.

## 🔑 Key Takeaways

- Agree on test cases (especially boundary values) *before* the agent generates any code.
- Constrain the agent to tests only: no implementation edits, no new dependencies, no unrelated refactors.
- Keep expected values explicit and independent of the function under test.
- Diagnose failures as setup / wrong expectation / real bug — don't weaken assertions to pass.
- Save your test conventions and safeguards in project custom instructions to make the workflow repeatable.

## 💡 Practical Example

A realistic agent-driven session in VS Code's Chat view (Agent mode):

1. **Inspect first:** "Inspect the testing setup in this project (framework, runner, naming, location). Propose boundary test cases for `validatePassword` before writing any code."
2. **Generate tests only:** "Add the agreed tests using the existing framework and conventions. Reuse existing helpers. Do not change implementation code, add dependencies, or refactor unrelated tests. Keep expected results explicit."
3. **Run and diagnose:** "Run the new tests with the project's existing test command. Report the command, pass/fail counts, and skipped tests. If a test fails, explain whether the cause is test setup, an incorrect expectation, or a possible implementation bug. Do not change implementation code or weaken assertions."
4. **Test Explorer shortcut:** if a testing extension exposes the failure, hover the failed test and click the sparkle **Fix Test Failure** icon — then review the suggestion against your agreed behavior before accepting.
5. **Make it repeatable:** add to the project's custom instructions: *"Never delete or weaken assertions, skip tests, or change expected values solely to make failing tests pass. If an assertion appears incorrect, explain why and ask for confirmation before changing it."*

Note: even with these instructions in place, review the diff and the actual test output yourself — custom instructions guide the agent but don't enforce anything.

## 🔗 Related Concepts

- [Review code edits](https://code.visualstudio.com/docs/agents/run/review-code-edits)
- [TDD custom agents and handoffs](https://code.visualstudio.com/docs/agents/guides/test-driven-development-guide)
- [Custom instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Agent tools (running tests)](https://code.visualstudio.com/docs/agents/run/tools)

## 🤔 Quick Check

You ask Copilot to add tests for a function `parseDate` that should reject malformed input. Three of the generated tests fail. You check the failure output and notice that for one failing case, the test's expected value was itself computed by calling `parseDate` on the same input. What does this tell you about that test, and what concrete change should you make to the assertion?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** The test is circular — it uses the function under test to compute the expected result, so if `parseDate` has a bug (or "works" by coincidence), both the actual and expected values agree and the test can never catch the defect. You should replace the computed expectation with an explicit, hard-coded value derived from the agreed requirement (e.g., the literal error you expect `parseDate("not-a-date")` to throw), then rerun the test to see whether it now exposes a real implementation bug.

</details>

---
*Lesson 15 of Phase 2 (VSCode Integration) | Streak: 15 days*
