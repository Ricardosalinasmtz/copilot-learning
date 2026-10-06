# 🎓 Copilot Lesson: Debugging Assistance with Copilot
**Date:** 2026-10-05 | **Day:** 13 of learning streak
**Source:** [Tutorial: Fix an API bug with an agent](https://code.visualstudio.com/docs/agents/guides/fix-a-bug-with-agents) | **Difficulty:** Intermediate

## 🎯 What You'll Learn

How to use the VS Code agent as a disciplined debugging partner rather than a "fix it" button. The workflow: reproduce → investigate (no edits) → failing regression test → minimal fix → boundary checks → diff review.

## 📖 The Concept

Most real work starts with existing code and a bug report, not a blank file. The temptation is to paste the symptom into chat and accept whatever patch comes back. The official VS Code tutorial argues for the opposite: **separate diagnosis from repair**, and make the agent prove each step with evidence.

First, reproduce the bug yourself. In the tutorial, a paginated API returns IDs 1–10 for `GET /issues` but 21–30 for `?page=2` (expected 11–20). A concrete symptom plus an expected result stops the agent from guessing what "pagination is broken" means. Then ask the agent to **investigate without editing files**: run the tests, trace the request, explain the cause with file references, and propose a regression test. A good diagnosis must explain *both* observed responses, link the one-based API page to the zero-based array offset (`start = page * PAGE_SIZE`), and explain why the existing tests still pass.

Next, have the agent add a **failing regression test**, and nothing else. The test should fail because the IDs are wrong, not because of a setup error, and it should check the data, not just the HTTP status. If the agent sneaks in an implementation change, [restore the checkpoint](https://code.visualstudio.com/docs/agents/run/review-code-edits#_restore-a-checkpoint) and narrow the prompt. Only then ask for the **smallest root-cause fix**, with constraints: keep the API contract, response shape and validation, add no dependencies, and run the full suite.

Finally, verify beyond the reported case (page 1, 2, the last partial page and an out-of-range page) and review the diff in Source Control before committing. The agent does the legwork; you stay responsible for evidence.

## 🔑 Key Takeaways

- **Investigate before editing:** say explicitly "Do not edit files yet." Review the diagnosis before allowing changes.
- **Red before green:** a regression test that fails first is your proof the bug is captured *and* your success condition for the fix.
- **Constrain the fix:** "smallest production-code change, preserve contract/shape/validation, no new dependencies" keeps diffs reviewable.
- **Passing tests ≠ correct code:** the original suite passed while the bug was live. Check boundaries manually.
- **Use checkpoints** to roll back when the agent oversteps the scope you gave it.

## 💡 Practical Example

A three-prompt sequence you can reuse in Agent mode (`Ctrl+Alt+I` → Agent):

```text
1) Investigate this bug. Do not edit files yet.
   Symptom: <command + actual output>
   Expected: <correct output and the contract it follows>
   Run the existing tests, trace the code path, explain the root cause
   with file references, and propose a regression test.

2) Add regression coverage in the existing tests, but do not change
   production code. Run the tests and confirm the new test fails
   because the result is wrong, not because of a setup error.

3) Fix the root cause with the smallest production-code change.
   Preserve the public contract, response shape, and validation.
   Do not add dependencies. Run the complete test suite.
```

For an ECG pipeline this maps directly: "R-peak indices are offset by one window after resampling. Expected: peaks align with the annotated `.atr` positions within ±1 sample. Don't edit yet. Trace where sample indices are converted between sampling rates."

## 🔗 Related Concepts

- [Test code with AI](https://code.visualstudio.com/docs/agents/guides/test-code-with-ai)
- [Review, revise and revert agent edits (checkpoints)](https://code.visualstudio.com/docs/agents/run/review-code-edits)
- Copilot in Integrated Terminal (earlier Phase 2 lesson)
- Prompt Engineering for Copilot Chat (earlier Phase 2 lesson)

## 🤔 Quick Check

You ask the agent to fix a bug where a CSV exporter drops the last row. It replies with a patch and says "all tests pass." You notice the test suite had no test touching the last row before or after the change. Following today's workflow, what should you do before accepting the patch?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Revert to the checkpoint and ask the agent to first add a regression test asserting that the last row is exported, then confirm it *fails* against the original code, and only then reapply a minimal fix and confirm it passes.

"All tests pass" means nothing if no test covers the bug. Watching the test go from red to green is what proves the bug was both captured and fixed.

</details>

---
*Lesson 13 of Phase 2 (VSCode Integration) | Streak: 13 days*
