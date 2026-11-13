# 🎓 Copilot Lesson: GitHub Copilot Fusion (Automatic Model Routing and Multi-Model Orchestration)
**Date:** 2026-11-06 | **Day:** 37 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/models/copilot-fusion | **Difficulty:** intermediate

## 🎯 What You'll Learn

Today you'll meet Copilot Fusion — a runtime model router that replaces "pick a model" with "pick an effort level". You'll see how it chooses between single-model, cascade, and critique execution patterns per prompt, how its effort levels trade cost against capability, and how it fits with local models, policies, and billing.

## 📖 The Concept

When you select a model in the VS Code model picker today, you're committing the whole task to one model's strengths and limits. Copilot Fusion inverts that: it's a system, not a model. When you choose Fusion, the runtime evaluates each prompt using capability signals — reasoning, code generation, debugging, tool use — and picks the most efficient execution pattern that meets the quality bar for the effort level you selected. You stop manually switching models mid-task; Fusion routes automatically and returns a single response.

Fusion has three execution patterns. **Single** behaves like a plain request to one model. **Cascade** has an efficient model draft a solution, then a judge inspects the draft and either accepts it or escalates the work to a stronger model. **Critique** has one model draft a response while multiple models critique it in parallel; the feedback is aggregated and the drafting model revises its answer based on the combined critique. The pattern is chosen per prompt, so two prompts in the same session can use different patterns, and extra model calls only happen when a task actually warrants them — a simple question won't be billed for a review it doesn't need.

Fusion exposes effort levels instead of model names: **Fusion Standard** (balanced cost/quality/latency, one model per task), **Fusion High** (orchestration worth the extra credits for substantial, well-scoped tasks like a complex bug fix), and **Fusion Max** (frontier capability for the deepest reasoning, available on higher-tier plans). **Fusion Fast** exists for fast-model scenarios like VS Code inline chat and isn't user-selectable. Routing also considers real-time system health and happens along natural cache boundaries, so the main conversation stays on the same model whenever possible — which preserves cached-token savings — while auxiliary models (like a reviewer) only receive the context they need.

A few operational details matter. Fusion has no context window of its own: each step runs within the limits of whichever model executes it, and the context size shown for Fusion is a conservative minimum across its model pool. Policy still applies — Fusion won't use models your plan doesn't include, models your admin excluded, or models barred by data-residency/FedRAMP policies. And in public preview, locally installed models (MAI Code 1.1 Flash on Windows, or Qwen3.8 Flash Next via a local provider on any OS) can be routed to automatically at the Fusion Standard level, once experimental features are enabled.

## 🔑 Key Takeaways

- Fusion is a router and orchestrator, not a model: you select an effort level (Standard/High/Max), and it picks the execution pattern, reasoning effort, and models per prompt.
- Three execution patterns — Single, Cascade (draft → judge → optional escalation), and Critique (draft + parallel multi-model review → revision) — are chosen per prompt and add model calls only when the task warrants them.
- The main conversation stays on one model where possible to preserve cache benefits; auxiliary models get only the context they need.
- Policies still gate it: plan availability, admin model exclusions, and data-residency/FedRAMP restrictions all apply; Fusion disappears from the picker if no usable models remain.
- Billing is per model used at that model's standard rate — no separate Fusion charge, but High/Max can consume more AI credits than Standard. Through November 30, 2026, paid plans get a 30% discount on Fusion model costs.

## 💡 Practical Example

In VS Code, open Copilot Chat and click the model picker. Instead of choosing a specific model, select **Fusion Standard** for everyday work.

Then try the escalation workflow on a real task:

1. Switch the picker to **Fusion High** and ask: "Refactor the retry logic in `sync_service.py` to use exponential backoff, and make it work with our existing `http_client` interface."
2. Watch the session progress — you can see each step and which model is running it. Intermediate drafts may be revised or discarded; only the final response is shown.
3. After the response lands, hover over the effort-level name below it to see exactly which models participated in that prompt.

You can also mix effort levels within one session: keep Fusion Standard for quick follow-up questions and flip to Fusion Max only for the gnarly design decision. If you have a local model installed (e.g., Qwen3.8 Flash Next via Ollama), Fusion Standard may automatically route simple steps to it — on the Copilot app that means turning on "Local model auto routing" in Experimental settings, and in the CLI, enabling experimental features with `--experimental` or `/experimental on`.

One gotcha to remember: if a discarded draft had already made file edits in your workspace, those edits are not rolled back automatically. Review the working tree before committing.

## 🔗 Related Concepts

- [Configuring access to AI models in GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/set-up-copilot/configure-access-to-ai-models)
- [Managing availability of default models in your enterprise](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-availability-of-default-models)
- [AI language models in VS Code (on-device models)](https://code.visualstudio.com/docs/agent-customization/language-models)
- [Models and pricing for GitHub Copilot](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- Creating and using custom agents for GitHub Copilot CLI (delegating tasks to a specific local model)

## 🤔 Quick Check

Your team's Copilot Business plan has admin policies that exclude one frontier model, and your Fusion Max option is missing from the model picker. A colleague insists Fusion must be broken or the policy is overriding the whole feature. Which of the two is more likely, and what would you check to confirm the diagnosis without asking the admin to roll back the policy?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Neither — a missing Fusion Max is expected behavior when one or more of the models it requires is disabled by policy. The docs state that Fusion Max not appearing means its required models may be disabled, and Fusion only falls back to the models your plan and policies allow; it only vanishes from the picker entirely when *no* models are available to it. To confirm, check the enterprise model-availability settings (the admin docs for managing availability of default models, including the Fusion section) to see which model is excluded, and compare it against the model list Fusion Max needs — Standard/High should still work with the remaining pool.

</details>

---
*Lesson 37 of Phase 4 (Deep Customization) | Streak: 37 days*
