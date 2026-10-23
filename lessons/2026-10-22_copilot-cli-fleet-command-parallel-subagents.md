# 🎓 Copilot Lesson: Copilot CLI /fleet Command (Parallel Task Execution with Subagents)
**Date:** 2026-10-22 | **Day:** 26 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agent-capabilities/choosing-and-delegating-agent-work/delegating-and-parallelizing-work-with-agents | **Difficulty:** advanced

## 🎯 What You'll Learn

Today you'll learn how the `/fleet` command in the GitHub Copilot CLI (and the Copilot app) takes a large task, splits it into subtasks, and hands the independent pieces to subagents that work in parallel. You'll see the typical plan-first workflow, how to assign a specialized custom agent to a subtask, how to monitor and control subagent tasks, and how to start fleet mode straight from the command line with `--fleet`.

## 📖 The Concept

When a task involves several operations that don't depend on each other — refactoring multiple files, updating dependencies, running tests across modules — working through them sequentially in one session is slow. The `/fleet` command changes the division of labor: the main Copilot agent becomes an orchestrator. It analyzes your prompt, decides whether the work can be split into smaller subtasks, maps out their dependencies, and delegates the suitable pieces to subagents. Independent subtasks run in parallel, so the whole task finishes faster than a single agent doing them one at a time.

A key design detail: each subagent gets its own context window. It carries only the context relevant to its assigned piece of the work instead of the entire main-session history, which keeps each subagent focused and prevents one long, sprawling conversation from degrading the quality of the work. The orchestrator tracks the overall goal, manages the dependencies between subtasks, and reviews and combines the results when the pieces come back.

You can also steer specialization: if you have a custom agent suited to a particular subtask, name it in your `/fleet` prompt (or Copilot will pick an appropriate custom agent based on the task and the agent's description). And fleet mode isn't limited to interactive sessions — `copilot --fleet -p "..."` runs an entire fleet task programmatically, which is where it shines for automation.

One trade-off to keep in mind: every subagent interacts with an AI model independently, so a fleet run consumes more GitHub AI credits than letting the main agent handle the work alone. And if your task is inherently sequential — each step depends on the previous one — `/fleet` buys you little or nothing. Parallelism pays off when the subtasks are independent, their boundaries are clear, and the cost of coordinating the results is less than the time saved.

## 🔑 Key Takeaways

- `/fleet` turns the main agent into an orchestrator: it breaks the task into subtasks, runs the independent ones in parallel, and combines the results.
- Each subagent has its own context window, so it can focus on a bounded task without carrying the main session's full context.
- Typical workflow: build an implementation plan in plan mode (Shift+Tab), then either accept the plan with Autopilot execution + /fleet, or exit plan mode and prompt `/fleet implement the plan`.
- Monitor subagent work with `/tasks`: Enter for details, `x` to kill, `r` to remove finished tasks, Esc to return to the prompt.
- `--fleet` on the command line (combined with `-i`, `-p`, or a piped prompt) starts fleet mode without typing the slash command; it isn't supported in ACP server mode (`--acp`).
- Expect increased AI credit usage — each subagent's model interactions are billed on top of the orchestrator's.

## 💡 Practical Example

**Plan first, then fleet** (interactive session):

1. Press Shift+Tab to switch into plan mode.
2. Describe the change: "Add a retry layer to the HTTP client and update all call sites."
3. Work with Copilot until the implementation plan looks right.
4. Choose **Accept plan and continue in Autopilot execution mode + /fleet** — or exit plan mode and prompt:

```text
/fleet implement the plan
```

**Assign a specialist** — if you have a custom testing agent:

```text
/fleet Implement the plan. Use the @test-writer agent
to create tests for each component.
```

**Automate it** — non-interactive fleet runs:

```shell
copilot --fleet -p "Refactor the utils package and update everything that calls it" --allow-tool write

echo "Update the project dependencies and run the tests" | copilot --fleet
```

**Watch progress** — while a fleet is running, run `/tasks` to see the subagent tasks, press Enter on one to view its details (and its summary once complete), and `x` to kill a stuck task.

## 🔗 Related Concepts

- [Delegating and parallelizing work with agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/choosing-and-delegating-agent-work/delegating-and-parallelizing-work-with-agents)
- [Creating and using custom agents for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli)
- [GitHub Copilot CLI command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)

## 🤔 Quick Check

You start a fleet run to update 40 files in a monorepo to a new authentication API. Halfway through, you suspect one subagent is stuck on a particularly gnarly file. What command lets you inspect all of the session's subagent tasks, and which keys do you use to view, kill, and clean up those tasks?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Run `/tasks` to list the session's background tasks, including the fleet subtasks. Navigate with the up/down arrow keys, press Enter on a task to view its details (and its summary once complete), press `x` to kill the stuck task, `r` to remove completed or killed tasks, and Esc to return to the main CLI prompt.

</details>

---
*Lesson 26 of Phase 4 (Deep Customization) | Streak: 26 days*
