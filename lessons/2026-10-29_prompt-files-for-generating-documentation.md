# 🎓 Copilot Lesson: Prompt Files for Generating Documentation (e.g., /document-api)
**Date:** 2026-10-29 | **Day:** 31 of learning streak
**Source:** https://code.visualstudio.com/docs/agent-customization/prompt-files | **Difficulty:** intermediate

## 🎯 What You'll Learn

Prompt files turn a task you repeat often, such as writing API documentation, into a standalone Markdown file that you run as a slash command in chat. You'll learn the file format, where the files live, and how to build a `/document-api` prompt that produces consistent OpenAPI-style docs.

## 📖 The Concept

A prompt file is a Markdown file with the `.prompt.md` extension. Unlike custom instructions, which apply automatically, you invoke a prompt file manually by typing `/` and its name in chat (with the Local harness selected). Each file encodes task-specific context and guidelines, so the whole team gets the same quality of output rather than relying on everyone's ad-hoc prompting.

Workspace prompt files live in `.github/prompts`. User-level prompt files live in your VS Code profile and are available in every workspace. They can also be synced across devices through Settings Sync (select "Prompts and Instructions"). An optional YAML frontmatter configures `description`, `name` (the text after `/`; defaults to the file name), `argument-hint`, `agent` (ask, agent, plan or a custom agent), `model` and `tools`. If `tools` is set, the default agent becomes `agent`.

The body is the prompt itself. You can link to other files with relative Markdown links or `#file:`, and use `~/` for your home folder. Link to your instructions files (for example a docs style guide) instead of duplicating rules in each prompt. You can run a prompt via `/name`, the Chat: Run Prompt command, or the play button in the editor, which is handy for iterating. Typing `/create-prompt` also lets the agent draft one for you.

## 🔑 Key Takeaways

- Prompt files are manual, on-demand slash commands. Custom instructions are always-on.
- Workspace files go in `.github/prompts/*.prompt.md`. User files live in your profile and sync via Settings Sync.
- Frontmatter (`agent`, `model`, `tools`, `argument-hint`) fixes how the prompt runs. The body defines what it does.
- Tool priority: tools in the prompt file, then tools from the referenced custom agent, then the agent's defaults.
- Reference style guides by link instead of copying their rules into every prompt.

## 💡 Practical Example

`.github/prompts/document-api.prompt.md`:

```markdown
---
name: document-api
description: 'Generate OpenAPI 3.1 documentation for the selected endpoint code'
argument-hint: 'endpoint file or route name'
agent: 'agent'
---
Document the API endpoint(s) in the referenced code.

- Follow the conventions in [docs style guide](../instructions/docs.instructions.md)
- Produce an OpenAPI 3.1 YAML fragment: path, method, parameters, request body,
  responses (including error codes) and an example for each
- Infer schemas from the code. Never invent fields that are not there
- Finish with a short list of undocumented behaviours you could not infer

Target: ${selection}
```

In chat: `/document-api for the orders routes`, or open the file and press the play button to test it.

## 🔗 Related Concepts

- [Custom instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Custom agents](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Agent skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [Awesome Copilot community prompts](https://github.com/github/awesome-copilot)

## 🤔 Quick Check

Your team wants every developer to generate API docs in the same format, but only when they ask for it, not on every chat request. The docs style rules already exist in an instructions file. How would you set this up?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** Create a workspace prompt file such as `.github/prompts/document-api.prompt.md` that links to the existing instructions file and defines the output format, then commit it so everyone can run `/document-api`.

Prompt files are invoked manually, which fits the "only when asked" requirement. Linking the style rules avoids duplicating them, and putting the file in `.github/prompts` shares it through the repository.

</details>

---
*Lesson 31 of Phase 3 (Advanced Workflows) | Streak: 31 days*
