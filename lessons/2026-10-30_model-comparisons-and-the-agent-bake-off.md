# 🎓 Copilot Lesson: Model Comparisons and the Agent Bake-Off
**Date:** 2026-10-30 | **Day:** 32 of learning streak
**Source:** https://code.visualstudio.com/docs/agents/run/compare-models | **Difficulty:** intermediate

## 🎯 What You'll Learn

Today's feature turns "which model is better for this task?" from a vibes-based debate into an experiment you can run inside VS Code. With Model Comparisons (Preview), you feed the same bounded prompt to 2–10 models or agent harnesses at once, then review their attempts side by side — or let a judge model score them and a synthesizer stitch together the best parts into one new implementation.

## 📖 The Concept

The Agents window can now run a "bake-off": you pick one task, add several models from your eligible harnesses (you can even mix Copilot, Claude, and Codex attempts), and hit "Run N Attempts." All attempts start concurrently as independent sessions, so you're not waiting for one model to finish before the next starts. In the Agents window, comparisons show up as a tiled grid where you can watch each attempt, open its full session, and inspect what it actually did.

There are two flavors. A folderless comparison (quick chat) runs independent chats with no worktrees — good for comparing plans, designs, or explanations. A repository comparison is the heavier option: each attempt gets its own isolated Git worktree based on your repo's committed state, so the models can actually make code changes without clobbering each other or your uncommitted work. Note the worktree only contains committed state — uncommitted changes and some ignored files won't be there.

Two optional roles make the bake-off more than a manual eyeball contest. A judge model automatically evaluates the finished attempts and produces a scorecard with a recommendation, time and token usage per attempt, and expandable rationale with validation evidence. A synthesizer model (which requires a judge to be configured too) then creates a brand-new implementation in yet another isolated worktree, using the judge's verdict and the strengths of the attempts as context — effectively "take what each model did best and write it properly."

Two cautions the docs hammer home. Repository comparisons run their worktree sessions with "Allow all" permissions, which skips tool approval prompts — the worktree separates the *edits*, not the agent's access to your machine or network, so understand sandboxing and approvals before you kick one off. And the judge's verdict is not proof of correctness: review the code, check the evidence, and run the tests before integrating any result. Finally, each attempt, judge evaluation, and synthesis is separate model work, so a 6-model bake-off with judge and synthesis is a lot of tokens — size the bake-off accordingly.

## 🔑 Key Takeaways

- Comparisons run only in the Agents window (not the editor Chat view) and are enabled by default via `sessions.chat.compareAgents.enabled`; you can add 2–10 models, including the same model at different thinking-effort levels.
- Repository attempts run in isolated Git worktrees from committed state, and the whole bake-off can be scored by a judge model and merged into one implementation by a synthesizer.
- Comparisons never merge, push, or open a PR for you — and a judge scorecard is an assessment, not a certificate of correctness. Verify with tests before you integrate.

## 💡 Practical Example

In the Agents window:

1. Select **New**, choose your Git repository (or start a quick chat for a folderless comparison).
2. Open the model picker and select **Compare Models** (scale icon). Add, say, 3 models — Copilot's default, a Claude model, and the same Copilot model at a higher thinking effort.
3. Optionally configure a judge (e.g., a strong general model) and a synthesizer before running.
4. Give every attempt one bounded prompt, e.g.:

```text
Add validation to the profile form so the display name is required and no
longer than 50 characters. Show an inline error, preserve the existing form
styles, and add focused tests for valid, empty, and overlong names.
Don't change other forms.
```

5. Hit **Run 3 Attempts**, watch the tiled grid, then read the judge's scorecard when the attempts finish.
6. Either open the recommended attempt's worktree, or select **Synthesize** with a note like "prefer the attempt with the cleanest test coverage" to get a fresh combined implementation in its own worktree — then review, test, and integrate it like any agent change.

## 🔗 Related Concepts

- [Agent Harnesses](https://code.visualstudio.com/docs/agents/run/agent-harnesses)
- [Agents Window](https://code.visualstudio.com/docs/agents/run/agents-window)
- [Review Code Edits (worktree integration)](https://code.visualstudio.com/docs/agents/run/review-code-edits)
- [Agent Sandboxing](https://code.visualstudio.com/docs/agents/run/agent-sandboxing)

## 🤔 Quick Check

Your team has been guessing at which model to use for refactoring legacy code versus writing new tests, and a teammate suggests running a bake-off in the Agents window. Walk through what you'd configure before hitting "Run N Attempts," and explain why the judge's final recommendation still shouldn't be the last step before you merge anything.

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Before running, pick a repository task, add 2–10 models from eligible harnesses in the Compare Models picker, and configure a judge (and optionally a synthesizer) — these must be set before you run, not after. Then write one bounded prompt with explicit success criteria so every attempt gets the identical goal, and choose the number of attempts knowing each one is separate model work. The judge's recommendation shouldn't be the last step because the docs state a judge's assessment isn't proof that an implementation is correct, and repository attempts run with "Allow all" permissions in isolated worktrees built from committed state. So you must open the recommended attempt, review the changed files, run the project's tests, and follow the normal review-and-worktree-integration workflow before merging — comparisons never merge or push for you.

</details>

---
*Lesson 32 of Phase 4 (Deep Customization) | Streak: 32 days*
