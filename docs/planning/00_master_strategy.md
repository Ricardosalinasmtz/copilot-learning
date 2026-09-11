# Master Strategy: GitHub Copilot Learning System

## Objective

Create an automated, sustainable learning system that teaches GitHub Copilot and VSCode features through weekly research and daily curated lessons.

## Approach

### Weekly Research Cycle (Fridays 12:30 PM Europe/Rome)

**Agent:** zuko-researcher (delegated by sokka-orchestrator)

**Responsibilities:**
1. Check current VSCode and Copilot versions against tracked versions
2. If version changed: identify new features, breaking changes, updated docs
3. Explore documentation structure systematically (local docs, GitHub docs, web resources)
4. Extract 5-7 teachable topics for the week
5. Add topics to queue with metadata (difficulty, section, estimated lessons, priority)
6. Update progress.json state
7. Commit research summary

**Workflow:**
```
1. Read state/progress.json
2. Run `code --version` to get VSCode version
3. Check Copilot integration status
4. Compare versions, detect changes
5. Explore docs and web resources
6. Identify priority topics based on:
   - Version changes (highest priority)
   - Learning phase curriculum
   - User interest signals (if any)
7. Update queue in progress.json
8. Write docs/planning/XX_research_summary.md
9. Commit changes
```

### Daily Lesson Delivery (Mon-Fri 1:00 PM Europe/Rome)

**Agent:** sokka-orchestrator

**Responsibilities:**
1. Read next topic from queue in state/progress.json
2. Read source documentation (or research if web-based)
3. Format as engaging lesson (5-10 min read)
4. Include:
   - What you'll learn (overview)
   - The concept (clear explanation)
   - Key takeaways (3-5 bullet points)
   - Practical example (code snippet or workflow)
   - Related concepts (links if known)
   - Quick check (reflection question or quiz)
5. Deliver to main chat
6. Save lesson to lessons/ folder
7. Mark topic as completed
8. Update streak counter

**Lesson Format:**
```markdown
# 🎓 Copilot Lesson: [Topic Name]
**Date:** YYYY-MM-DD
**Source:** [doc path or URL]
**Difficulty:** [beginner/intermediate/advanced]
**Phase:** [phase number]

## 🎯 What You'll Learn

[2-3 sentence overview]

## 📖 The Concept

[Clear explanation in plain language, 2-4 paragraphs]

## 🔑 Key Takeaways

- Takeaway 1
- Takeaway 2
- Takeaway 3

## 💡 Practical Example

[Code snippet, workflow, or real usage example in VSCode]

## 🔗 Related Concepts

- [Concept 1](link if known)
- [Concept 2](link if known)

## 🤔 Quick Check

[One reflection question or mini-quiz to reinforce learning]

---
*Lesson [X] of Phase [Y] | Streak: [Z] days*
```

## Learning Phases

### Phase 1: Core Copilot Features (Weeks 1-3)
- Inline suggestions and ghost text
- Copilot Chat interface
- Slash commands (/tests, /explain, /fix, /docs)
- Context management (@workspace, @file, @terminal)
- Accepting/rejecting suggestions
- Natural language to code in comments

### Phase 2: VSCode Integration (Weeks 4-6)
- Copilot in integrated terminal
- Debugging assistance
- Refactoring workflows
- Test generation patterns
- Multi-file edits
- Copilot Edits mode

### Phase 3: Advanced Workflows (Weeks 7-9)
- Custom instructions for Copilot
- PR review workflows
- Enterprise features
- Team collaboration patterns
- Copilot for documentation
- Code review assistance

### Phase 4: Deep Customization (Weeks 10+)
- Copilot API usage
- Extensions ecosystem
- Workflow automation
- Integration with other tools
- Advanced prompt engineering
- Performance optimization

## Success Metrics

- **Consistency:** Daily lessons delivered Mon-Fri without gaps
- **Completion:** All core topics covered across 4 phases
- **Retention:** User can reference and apply concepts in real work
- **Adaptability:** System responds to version changes and new features

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| Queue runs empty | Research job prioritizes filling queue; user can request manual topics |
| Version changes break lessons | Version check flags incompatible content; adjust lessons accordingly |
| User misses lessons | Lessons archived in lessons/ folder for later review |
| Topics too advanced | Difficulty tagging; adjust based on user feedback |
| Source docs missing | Fallback to web research; mark topic as web-sourced |

## Git Workflow

- **Initial setup:** Commit project structure, master strategy, empty state
- **After research:** Commit research summary and updated queue
- **Weekly:** Commit lesson archive (batch lessons from the week)
- **Version changes:** Tag commit with version number

## Next Steps

1. ✅ Create project structure
2. ✅ Initialize state tracking
3. ⏳ Set up automations (weekly research + daily delivery)
4. ⏳ Run first research cycle (Friday 12:30 PM)
5. ⏳ Deliver first lesson (Monday 1:00 PM)
6. ⏳ Monitor and iterate
