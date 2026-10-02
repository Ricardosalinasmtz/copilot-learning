# 🎓 Copilot Lesson: Copilot Spaces (Organizing and Sharing Context)

**Date:** 2026-10-01 | **Day:** 11 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/context/spaces | **Difficulty:** intermediate

## 🎯 What You'll Learn

How Copilot Spaces organize and share the context GitHub Copilot uses — repositories, code, issues, notes, images — in curated collections that make your AI assistant far more focused and collaborative.

## 📖 The Concept

### What Is a Copilot Space?

A Copilot Space is a curated collection of context that shapes how Copilot answers your questions. Instead of relying on Copilot's generic understanding or repeatedly pointing it to files, you assemble everything relevant — repositories, code snippets, pull requests, issues, free-text notes, transcripts, images, and file uploads — into a single container. Copilot then grounds its answers in that space's context, producing more precise and task-specific results.

Think of it as giving Copilot a "working memory" for a particular project, investigation, or task.

### Why Spaces Matter

Without spaces, Copilot's context is limited to what's open in your editor or what you explicitly mention in a chat prompt. This creates friction:

- You ask the same contextual questions repeatedly
- Copilot misses important details because they're scattered across repos and issues
- Team knowledge is trapped in individual chat histories

Spaces solve these problems. They provide a persistent, organized context that stays synced with your project. GitHub files added to a space are automatically updated as they change, making Copilot an "evergreen expert" on your project — it always has the latest version.

### Access & Sharing

**Anyone** with a Copilot license (Free, Business, or Enterprise) can create and use Spaces.

- **Organization-owned spaces:** Shared within the org. You assign access levels — admin, editor, or viewer — to other members, or keep the space hidden ("No access").
- **Personal spaces:** Shared publicly (view-only by default; viewers see only sources they can access), shared with specific GitHub users, or kept private.

Spaces can be used in **Copilot Chat on GitHub** directly, and also in your IDE via the GitHub MCP server — bridging the gap between web and desktop workflows.

### Usage & Billing

Space-based chat counts toward your normal Copilot usage:
- **Free users:** Counts against monthly chat limit
- **Business/Enterprise:** Draws from shared AI credits pool, billed by model and token count

## 🔑 Key Takeaways

- **Spaces organize context** — repos, issues, PRs, code, notes, images — into curated collections Copilot uses to ground its answers
- **Context stays current** — GitHub sources in spaces auto-sync as files change; Copilot always sees the latest version
- **Sharing is flexible** — org-owned spaces use role-based access (admin/editor/viewer); personal spaces can be public, shared with users, or private
- **Works everywhere** — accessible from Copilot Chat on GitHub and your IDE via the GitHub MCP server
- **Free for all** — any Copilot license holder can create and use Spaces

## 💡 Practical Example

### Scenario: Onboarding a New Team Member

Instead of pasting links to 10 different repos and files in chat, create a Space:

```
1. Open GitHub → Copilot → Spaces → New Space
2. Name it: "Backend API Migration — Q4 2026"
3. Add sources:
   - Repository: company/backend-api
   - Repository: company/api-gateway
   - Pull Request: #342 (migration PR)
   - Issue: #128 (design doc)
   - Upload: migration-checklist.pdf
   - Text note: "Key decision: use gRPC over REST for inter-service calls"
4. Share with onboarding team as editor
5. New member asks Copilot in the space: "What are the migration milestones?"
   → Copilot answers grounded in ALL added context
```

### Using Spaces in Your IDE

If you have the GitHub MCP server configured:

```
1. In VSCode, open Copilot Chat
2. Reference the space: @github:spaces:my-space-name
3. Ask questions — Copilot pulls context from the space
```

This means your IDE-based Copilot can leverage the same curated context as your GitHub web-based Copilot.

## 🔗 Related Concepts

- [Custom Instructions for Copilot](https://docs.github.com/en/copilot/customizing-your-workflow/customizing-copilot/using-custom-instructions-with-github-copilot) — Personal, repo, and org-level instructions
- [Copilot Chat Interface](https://docs.github.com/en/copilot/github-copilot-chat/using-github-copilot-chat/interacting-with-github-copilot-chat) — Chat modes and features
- [Model Context Protocol (MCP)](https://docs.github.com/en/copilot/concepts/context/mcp) — How IDEs connect to GitHub context
- [Copilot Memory — Repository Facts](https://docs.github.com/en/copilot/customizing-your-workflow/customizing-copilot/customizing-copilot-for-your-repository) — Persistent repo-level context

## 🤔 Quick Check

If you're on a Copilot Business plan and create a Space that references 5 repositories and 10 issues, how does Copilot's usage from that Space get billed? What happens to those GitHub sources if someone pushes changes to the repos?

---

*Lesson 11 of Phase 3 (Advanced Workflows) | Streak: 11 days*
