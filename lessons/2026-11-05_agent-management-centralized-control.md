# 🎓 Copilot Lesson: Agent Management (Centralized Control of Parallel Agent Sessions)
**Date:** 2026-11-05 | **Day:** 36 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/agent-capabilities/choosing-and-delegating-agent-work/agent-management | **Difficulty:** intermediate

## 🎯 What You'll Learn

GitHub now offers a single, centralized control surface — the **Agents** tab in a repository (and the Copilot home page) — where you can launch, monitor, and steer multiple agent sessions at once without juggling windows or losing your place. You'll learn how to fan work out across concurrent sessions, follow live progress, and hand completed sessions off to VS Code or the Copilot CLI for local finishing.

## 📖 The Concept

AI agents are autonomous systems that evaluate their environment, make decisions, and take actions toward a goal. Because they can work independently, you can run **multiple agent sessions concurrently** in the same repository and delegate different work items to each of them — for example one agent fixing a failing test while another drafts a refactoring and a third investigates a flaky CI job. The Agents tab is the single pane of glass for all of them.

From the Agents tab you can kick off new tasks (choosing the AI model, a third-party agent like Claude or Codex, or a custom agent profile you've built), open any session's **live log** to watch the agent's progress and reasoning in real time, and see every active session at a glance. Crucially, you can **steer a session mid-run**: if the agent is heading the wrong direction or should use a particular tool, you send steering input without stopping the run. Note that steering consumes AI credits per message, so reserve it for moments that actually change the outcome.

The other half of the loop is handoff and review. When you want to work on a session's changes locally, you can **Open in VS Code** (requires the latest VS Code, GitHub Copilot, and GitHub Pull Requests extensions) or **Continue in GitHub Copilot CLI** to bring the session onto your machine. When the agent finishes, you jump to the pull request to review, request improvements, or merge. You can also set up automations so agents run on a schedule or in response to events like a new issue, and you can even query your past sessions in natural language from VS Code or the CLI.

## 🔑 Key Takeaways

- The **Agents tab** (repo level) and the **Copilot home page** are the two centralized control points: start sessions, see all active sessions, and open live logs — all without leaving the repo.
- You can **steer an agent mid-session** without stopping it, but each steering message costs AI credits, so steer deliberately rather than conversationally.
- Sessions are **portable**: hand a cloud session to **VS Code** or **GitHub Copilot CLI** to continue locally, and bring results back as pull requests for review and merge.
- Agent choice is flexible: pick the **model**, a **third-party agent** (Claude, Codex), or a **custom agent** with a tailored system prompt for task-specific roles like writing tests or refactoring.

## 💡 Practical Example

A realistic fan-out workflow on a busy release day:

1. Open your repo on GitHub.com and go to the **Agents** tab.
2. Start three sessions from the same tab:
   - **Session A** — custom "test-writer" agent: "Add regression tests for the payment retry bug fixed in PR #482."
   - **Session B** — default Copilot, chosen model: "Investigate the flaky `test_checkout` failure on main and open a PR with a fix."
   - **Session C** — default Copilot: "Update the OpenAPI spec in `/api` to match the new webhook endpoints."
3. While A and C run, open **Session B's live log** and watch its reasoning. If it starts downgrading the test instead of fixing the race condition, send one steering message: "Do not modify the test assertions — find the race in `checkout_service.py`."
4. When A finishes, click **Open in VS Code**, run the new tests locally, and push from the session branch.
5. When B opens its PR, review it on GitHub, request one change, and merge.

The point: one tab to launch and watch everything, one steering intervention that actually mattered, and clean handoff to local tools only where you needed hands-on work.

## 🔗 Related Concepts

- [Managing agent sessions (steering, tracking)](https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/manage-and-track-agents)
- [Starting GitHub Copilot sessions](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)
- [Custom agents](https://docs.github.com/en/copilot/concepts/customizations/custom-agents)
- [Copilot automations](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-automations)
- [Review output from Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/review-copilot-output)

## 🤔 Quick Check

Your team is preparing for a quarterly release and you have four independent tasks: fixing two product bugs, updating documentation, and bumping the version with a changelog entry. You want all of them progressing in parallel, but you're also the one who will review the resulting pull requests. Using what you learned about the Agents tab, how would you organize this work, and where would you be cautious about how often you send steering messages?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Open the repo's Agents tab and start one session per task — two for the bugs, one for the docs, one for the version bump (a docs-oriented or default agent profile fits each role). Monitor all four from the same tab via live session logs, let them run to completion, then review each resulting pull request. Be sparing with steering: every steering message consumes AI credits, and mid-run interruptions can churn an agent's plan, so only intervene when a session is clearly heading in the wrong direction — otherwise let it finish and correct course in the PR review, which costs nothing and keeps the diff reviewable.

</details>

---
*Lesson 36 of Phase 3 (Advanced Workflows) | Streak: 36 days*
