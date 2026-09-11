# MEMORY-copilot-learning.md

Project-specific memory for the GitHub Copilot Learning System.

<!-- project: github.com/Ricardosalinasmtz/sokka-orchestrator -->

## Decisions

- **Automation over external cron:** Uses OpenClaw's built-in automations for better integration and reliability
- **Weekly collection, daily delivery:** Research on Fridays, lessons Mon-Fri (not random/hourly)
- **Version-aware learning:** Track VSCode and Copilot version changes and prioritize new features
- **Curriculum approach:** Structured phases instead of random topic selection
- **Direct chat delivery:** Lessons delivered to main session, not webhooks
- **Session configuration:**
  - Weekly research: zuko-researcher (agent:zuko-researcher:main)
  - Daily lesson: sokka-orchestrator (dashboard session to avoid delivery bugs)

## Technical Learnings

- OpenClaw automations support cron schedules with timezone specification
- Automation payloads can include detailed step-by-step instructions
- State tracking via JSON files in project directory
- VSCode 1.136.1+ has Copilot Chat built-in (no separate extension needed)
- GitHub Copilot Business (Enterprise) provides full feature access

## Environment

- **OS:** Ubuntu 24.04.4 LTS
- **VSCode:** 1.136.1
- **Copilot Integration:** Built-in (VSCode native)
- **Copilot Subscription:** GitHub Copilot Business (Enterprise)

## Dead Ends / Avoided Approaches

- ~~Hourly random topic selection~~ → Too chaotic, no curriculum structure
- ~~External cron + webhook delivery~~ → Unnecessary complexity when OpenClaw automations exist
- ~~Single combined job~~ → Separation of concerns: research vs delivery


## Version Tracking

- Initial version: VSCode 1.136.1, Copilot built-in
- Version check: Fridays at 12:30 PM during research phase

## Learning Phases

1. **Phase 1 - Core Copilot Features:** Inline suggestions, chat, slash commands, context management
2. **Phase 2 - VSCode Integration:** Terminal, debugging, refactoring, test generation
3. **Phase 3 - Advanced Workflows:** Custom instructions, Copilot Edits mode, PR workflows, enterprise features
4. **Phase 4 - Deep Customization:** API usage, extensions ecosystem, workflow automation
