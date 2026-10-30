# 🎓 Copilot Lesson: Copilot CLI Extensions (Custom Tools & Slash Commands via SDK)
**Date:** 2026-10-28 | **Day:** 30 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/customizations/extensions | **Difficulty:** advanced

## 🎯 What You'll Learn

Today you'll learn how Copilot CLI extensions work: small Node.js modules that run as a separate process alongside an interactive Copilot session and can register custom tools (the agent calls them) and slash commands (you call them). You'll also see where extensions live (repository, personal, plugin), how they compare to skills, MCP servers, and hooks, and the trust implications of running extension code on your machine.

## 📖 The Concept

An extension for GitHub Copilot CLI is a JavaScript module — a single entry file, `extension.mjs` (or `.cjs`/`.js`), inside a directory whose name becomes the extension name — that runs as a separate Node.js process alongside your interactive session. It connects to the session through `joinSession()` from the extension SDK (`@github/copilot-sdk/extension`), which is bundled with the CLI, so there's nothing extra to install. Once connected, an extension can register **tools** the agent can call while working, **slash commands** you invoke yourself, and session-aware behavior: it can subscribe to events (tool calls starting and completing, assistant usage, and more) and keep state across calls for the lifetime of the session.

Extensions are distinct from two similarly named things. They are not IDE extensions like the Copilot editor extension, and they are not the retired GitHub App-based Copilot Extensions integration. They can be used in the GitHub Copilot app and in Copilot CLI (currently an experimental feature there). Scopes decide who gets the capability: **repository** extensions live in `.github/extensions/NAME/` and stay with the project, **personal** extensions live in `~/.copilot/extensions/NAME/` and follow you across directories, and **plugin** extensions ship as part of an installed plugin. Because the CLI feature is experimental, you enable it per session with `--experimental` at startup or `/experimental on` in an open session; `/extensions manage` shows each extension's status and the path to its log file under `~/.copilot/logs/extensions/`.

The key design idea is stateful observation. The CLI does not surface every internal detail — for example, how long each tool call took, or how many tokens each turn used — but it emits events such as `tool.execution_start`, `tool.execution_complete`, and `assistant.usage`. An extension can listen to those events, accumulate state in its own long-lived process memory, and expose it through a tool or command. A tool definition includes a `description` the model relies on to decide when to call it (so write it explicitly), a `parameters` JSON Schema for its arguments, and an async `handler` whose returned string becomes the result the model reads. Options like `defer: "never"` keep the tool always visible in the model's tool list, and `skipPermission: true` lets a read-only tool run without a permission prompt on every call.

Finally, know when an extension is the right surface for a need. A repeatable workflow expressed as instructions and resources is an agent skill. Access to tools from an external service is an MCP server. A command at a defined agent lifecycle event is a hook. A distributable bundle of related capabilities is a plugin. Session-aware tools or slash commands implemented in JavaScript are an extension. And the security note is worth internalizing: extensions execute on your computer with your privileges, so you should only load extension code you trust — the same standard as any other script you didn't write.

## 🔑 Key Takeaways

- An extension is a long-lived Node.js process connected to your CLI session via `joinSession()` from the bundled `@github/copilot-sdk/extension` SDK.
- It can add agent-callable **tools**, user-invoked **slash commands**, and event-driven state that persists for the whole session.
- Scope decides who gets it: `.github/extensions/NAME/` for the team, `~/.copilot/extensions/NAME/` for yourself, plugin for distribution.
- Extensions are experimental today (`--experimental` or `/experimental on`) and run with your full local privileges — only load trusted code.

## 💡 Practical Example

The official tutorial builds a project-scoped `/tokencount` command that reports how many tokens you've used since you started counting. Create `.github/extensions/token-counter/extension.mjs` in your repo:

```javascript
import { joinSession } from "@github/copilot-sdk/extension";

let tokensUsed = 0;   // running total for the session
let startedAt = null; // baseline recorded by "/tokencount start"

const session = await joinSession({
    commands: [
        {
            name: "tokencount",
            description: "Report how many tokens you've used since '/tokencount start'.",
            handler: async (ctx) => {
                const arg = (ctx.args ?? "").trim();
                if (arg === "start") {
                    startedAt = tokensUsed;
                    await session.log("Token counter started.", { level: "info" });
                    return;
                }
                if (startedAt === null) {
                    await session.log("Start with '/tokencount start' first.", { level: "info" });
                    return;
                }
                await session.log(
                    `You have used ${tokensUsed - startedAt} tokens since '/tokencount start'.`,
                    { level: "info" },
                );
            },
        },
    ],
});

session.on("assistant.usage", (event) => {
    const { inputTokens = 0, outputTokens = 0 } = event.data ?? {};
    tokensUsed += inputTokens + outputTokens;
});
```

Then start the CLI in the same repo with `copilot --experimental`, verify the extension is listed as running via `/extensions manage`, and in a session run `/tokencount start`, send a few prompts, then `/tokencount` to see the count. The totals live in the extension's process memory, so they reset on `/clear` or a restart; if the extension misbehaves, its log under `~/.copilot/logs/extensions/` is where to look.

## 🔗 Related Concepts

- [Comparing Copilot customization features](https://docs.github.com/en/copilot/concepts/customizations/comparing-customizations)
- [Creating extensions for GitHub Copilot CLI (tutorial)](https://docs.github.com/en/copilot/tutorials/create-an-extension)
- [Copilot SDK](https://docs.github.com/en/copilot/how-tos/copilot-sdk)

## 🤔 Quick Check

Your team works in a shared repository and you've built a personal extension under `~/.copilot/extensions/` that adds a useful `/lint-summary` command, but you realize the rest of the team can't see it and they need it. What is the correct way to make the extension available to everyone working in that repository, and what should you warn your teammates about before they enable it?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Move the extension directory into the repository at `.github/extensions/lint-summary/extension.mjs` as a repository-scoped extension, and commit it so collaborators who work in that repo pick it up in their sessions. The warning is that Copilot CLI extensions execute on your computer with your privileges, so each teammate should review and trust the extension code before enabling it — the same standard as running any other script you didn't write.

</details>

---
*Lesson 30 of Phase 4 (Deep Customization) | Streak: 30 days*
