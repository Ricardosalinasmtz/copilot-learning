# 🎓 Copilot Lesson: Bring Your Own Key (BYOK) for GitHub Copilot
**Date:** 2026-10-27 | **Day:** 29 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/models/bring-your-own-key | **Difficulty:** advanced

## 🎯 What You'll Learn

GitHub Copilot doesn't have to be powered only by GitHub-hosted models. Bring Your Own Key (BYOK) lets you plug in models you already pay for — or models running on your own hardware — and use them directly inside Copilot. Today we'll look at the two distinct mechanisms GitHub provides: local BYOK (your own key, handled entirely on your machine) and enterprise BYOK (an admin-configured model available to your whole licensed population).

## 📖 The Concept

BYOK exists to solve a few practical problems: saving costs, consolidating billing across one LLM provider, or simply supporting models that GitHub doesn't host. It is built on two separate mechanisms, and it's important not to confuse them.

The first is **local BYOK**. Individual users configure their own API keys (or a local endpoint) directly in a Copilot client. The keys are handled entirely client-side: they're stored locally, the associated models are only available to you, and — crucially — this mechanism removes the dependency on GitHub's Copilot API for those models. That makes it suitable for air-gapped environments or even for users without a Copilot subscription at all. Local BYOK is supported across several clients: VS Code, JetBrains IDEs, Xcode, the Copilot CLI, the GitHub Copilot app, and the Copilot SDK. One caveat: if you're on a Copilot Business or Enterprise plan, your enterprise or organization can disable local BYOK in IDEs via policy — so what you're allowed to self-serve depends on your org's rules.

The second is **enterprise BYOK**, currently in public preview. Here, an enterprise owner (or an org owner, if the enterprise enabled the **Enable custom models** policy) adds keys for custom models in enterprise or organization settings. Unlike local BYOK, this is handled server-side and affects the models served to users by the Copilot API: custom models appear in Copilot Chat, the Copilot CLI, and IDEs, and licensed users select them exactly like GitHub-hosted models. Because it flows through the Copilot API, users need both a Copilot license and internet access — enterprise BYOK is about *which model* answers, not about escaping GitHub's infrastructure.

## 🔑 Key Takeaways

- BYOK = using your own LLM provider or local models inside Copilot, to save costs, consolidate billing, or run offline.
- Two mechanisms, don't mix them: **local BYOK** (user's key, client-side only, no Copilot API dependency) vs **enterprise BYOK** (admin-configured, server-side via the Copilot API, requires license + internet).
- Local BYOK is available in VS Code, JetBrains, Xcode, Copilot CLI, the Copilot app, and the Copilot SDK.
- On Business/Enterprise plans, org policy can disable local BYOK in IDEs — your ability to self-serve keys depends on your org.
- Enterprise BYOK (public preview) needs the **Enable custom models** policy for org-level management; custom models are selectable just like GitHub-hosted ones.

## 💡 Practical Example

Say you already have an Anthropic API subscription and you want to use Claude inside VS Code's Copilot chat instead of burning GitHub's included model usage:

1. Open a Copilot chat session in VS Code and click the **model picker** (the model name at the top of the chat).
2. Choose **"Add a model from a built-in provider"** — this is the documented local BYOK entry point in VS Code.
3. Pick the provider (e.g., Anthropic) and paste your API key. The key is stored locally and used client-side.
4. The new model now appears in the model picker; select it for any session.

If your team is on Copilot Business and the option is missing or greyed out, ask your org admin — local BYOK in IDEs may be disabled by policy. If instead the whole org wants a standard on-prem model for everyone, that's the enterprise BYOK path: the enterprise owner enables the **Enable custom models** policy and registers the model's key in enterprise settings, and it shows up in every licensed user's picker.

## 🔗 Related Concepts

- [Add a model from a built-in provider (VS Code docs)](https://code.visualstudio.com/docs/agent-customization/language-models#_add-a-model-from-a-built-in-provider)
- [Adding LLM models to GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-byok-models)
- [Enabling custom models for GitHub Copilot in your enterprise](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/enable-custom-models)

## 🤔 Quick Check

Your small agency is on a Copilot Business plan, and a client has requested that no client code ever leave the office, but the rest of your work can use any model. You'd like to run a private local model for the client project while keeping normal GitHub-hosted models for everything else. Which BYOK mechanism would you use, why does it satisfy the client's requirement, and what should you check first in your own org settings before promising the client this works?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Use local BYOK: point VS Code at the local model via "Add a model from a built-in provider" (or a local endpoint), and because local BYOK is handled entirely client-side with no dependency on GitHub's Copilot API, your code never leaves the machine. That's exactly why it differs from enterprise BYOK, which still flows through the Copilot API and requires internet access. Before promising the client, check with your Business-plan admin that local BYOK in IDEs hasn't been disabled by an enterprise or organization policy — that is the one thing in your org's control that could block the setup.

</details>

---
*Lesson 29 of Phase 4 (Deep Customization) | Streak: 29 days*
