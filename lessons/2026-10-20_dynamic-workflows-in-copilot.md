# 🎓 Copilot Lesson: Dynamic Workflows in Copilot

**Date:** 2026-10-20 | **Day:** 24 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/dynamic-workflows | **Difficulty:** advanced

## 🎯 What You'll Learn

A dynamic workflow is a program defined in code that orchestrates a complex, multi-step task by combining deterministic operations with one or more Copilot agents. Today you'll learn how dynamic workflows differ from autopilot and `/fleet`, how to create, run, and share them, and how to cap their AI-credit consumption so a large run can't blow your budget.

## 📖 The Concept

A dynamic workflow is a program that defines how a task is carried out. Instead of letting Copilot decide the plan on the fly — the way autopilot or `/fleet` do — the workflow's author defines the steps in code: which operations run deterministically, when agents get involved, how results flow between stages, and whether steps run sequentially, in parallel, or both. Agents handle the parts that need analysis or judgment; the surrounding code keeps the process repeatable. The docs' example is an incident investigation: collect logs and telemetry, assign independent agents to analyze different systems, and combine their structured findings into a timeline and root-cause report.

Workflows live inside Copilot extensions. They can be authored by Copilot (you describe the process in natural language), written by you using Copilot's built-in authoring guidance, or shared with you from a repository. Once loaded, you run one by name in a prompt — "Run the java-security-review dynamic workflow on the Java files in the current directory" — and it executes in the background while you keep working. In Copilot CLI you can also run `copilot workflow run WORKFLOW-NAME` directly, which is handy for scripts and CI/CD pipelines, or start a workflow from a slash command, custom tool, programmatic hook, or the Copilot SDK.

The difference from the tools you already know is about who defines the process. **Autopilot** is a mode of autonomy: Copilot decides the next steps. **`/fleet`** encourages delegation to subagents, but Copilot decides how to divide and coordinate the work each time. A **dynamic workflow** carries out a process defined in code — the author fixes the steps, conditions, and handoffs, and one or many agents execute them. Reusing a workflow means reusing its steps and rules; the path through those steps can still vary with the inputs or findings, but the skeleton is stable.

Because a workflow can launch many subagents, Copilot gives it guardrails: a limit on concurrently active subagents, a limit on total subagents launched per run, a maximum active running time (paused time doesn't count), and an approximate cap on AI credits. You can set these in your prompt, in the workflow code, or as personal defaults (`/settings workflows.defaultLimits.*`), with the prompt overriding the workflow code, which overrides your defaults. Runs can be inspected with `/workflows` (or the Workflows button in the Copilot app), paused, and resumed — a resumed run reuses the saved results of completed steps instead of redoing them.

## 🔑 Key Takeaways

- A dynamic workflow is a code-defined process: the author, not the model, fixes the steps, conditions, and handoffs.
- It complements autopilot (autonomous mode) and `/fleet` (model-decided delegation): a deterministic skeleton with agents for the judgment-heavy parts.
- Workflows live in extensions and are scoped to the session, personal, or project level; copy the extension or package it as a plugin to share.
- Guard your credit budget: test on a small scope first, then set limits — prompt > workflow code > personal settings.
- Runs are monitorable and resumable via `/workflows`; completed steps are saved and reused on resume.

## 💡 Practical Example

A release-review workflow in Copilot CLI:

```text
# 1. Create the workflow by describing the process
"Create a dynamic workflow called review-pr that:
 - runs our style checks deterministically on each changed file
 - splits the changed files across subagents that review in parallel
 - only reports findings that a second-model agent also agrees with
 - pauses for my review before posting the summary"

# 2. Test with a limited scope before the real run
"Run the review-pr dynamic workflow on just the two largest changed files."

# 3. Inspect the actual credit usage, then tighten the saved definition
"Update the review-pr dynamic workflow to use no more than 10 subagents in total."

# 4. Set personal defaults once (they apply to every workflow run)
/settings workflows.defaultLimits.timeoutSeconds 3600
/settings workflows.defaultLimits.maxAiCredits 50

# 5. Run it headless from a script or CI, then monitor, pause, or resume
copilot workflow run review-pr
/workflows
```

## 🔗 Related Concepts

- [Using dynamic workflows (how-to)](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-dynamic-workflows)
- [About Copilot CLI](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli) — the CLI hosts `workflow run` and `/workflows`
- Copilot CLI Extensions (custom tools & slash commands) — later in the queue
- Copilot Automations (delivered 2026-10-16) — scheduled and event-triggered cloud agent runs
- Agent Hooks (delivered 2026-10-14) — another way to trigger agent work from code

## 🤔 Quick Check

You've been asked to review 300 changed files in a large monorepo before a release, and last time you tried an agent-based review it burned through your monthly AI-credit allowance in one afternoon. Using what you learned today, how would you design the review so the total credit cost is known in advance — which limits would you set, where would you set them, and what would you run first to find out how expensive the full run would be?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Build it as a code-defined workflow with subagent limits (`maxTotalSubagents` and `maxConcurrentSubagents`) plus a `maxAiCredits` cap, set in the prompt or the workflow code so they override personal defaults, and first run it on a small batch of files. The small run reports its real per-file credit cost in `/workflows`, so you can extrapolate the 300-file cost and confirm the cap before the full run — and if the run does hit a limit, it stops with saved results you can resume after raising it.

</details>

---
*Lesson 24 of Phase 3 (Advanced Workflows) | Streak: 24 days*
