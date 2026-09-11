# GitHub Copilot Learning System

An automated learning system that teaches GitHub Copilot and VSCode features through curated daily lessons.

## Overview

This project implements a structured learning approach for mastering GitHub Copilot:

- **Weekly Research (Fridays 12:30 PM):** Explores Copilot/VSCode documentation, checks version changes, and maintains a learning queue
- **Daily Lessons (Mon-Fri 1:00 PM):** Delivers one curated lesson from the queue directly to the user

## Structure

```
copilot-learning/
├── README.md                          # This file
├── MEMORY-copilot-learning.md         # Project decisions and learnings
├── docs/planning/                     # Phase plans and strategies
│   ├── 00_master_strategy.md
│   └── XX_research_summary.md
├── state/                             # Runtime state tracking
│   ├── progress.json                  # Current version, queue, completed topics
│   └── version_history.json           # Version change tracking
├── lessons/                           # Delivered lessons archive
│   └── YYYY-MM-DD_lesson.md
├── scripts/                           # Helper scripts (if needed)
├── input/                             # User-provided materials
├── tmp/                               # Temporary work files
└── trash/                             # Recoverable deletions
```

## Automations

1. **Weekly Research Job** - Fridays 12:30 PM Europe/Rome
   - Checks VSCode and Copilot versions for changes
   - Explores documentation and web resources for new/important topics
   - Updates the learning queue with 5-7 topics
   - Runs in sokka-orchestrator session

2. **Daily Lesson Delivery** - Mon-Fri 1:00 PM Europe/Rome
   - Extracts next topic from queue
   - Formats as an engaging lesson
   - Delivers to the main chat session

## Current Status

- **VSCode Version:** 1.136.1
- **Copilot Integration:** Built-in (VSCode native)
- **Copilot Subscription:** GitHub Copilot Business (Enterprise)
- **OS:** Ubuntu 24.04.4 LTS
- **System Status:** Active
- **Learning Phase:** Core Features

## Usage

Lessons are delivered automatically to the main chat. To check progress or request specific topics, ask the orchestrator.

## Learning Phases

1. **Phase 1 - Core Copilot Features:** Inline suggestions, chat, slash commands
2. **Phase 2 - VSCode Integration:** Terminal, debugging, refactoring, test generation
3. **Phase 3 - Advanced Workflows:** Custom instructions, Edits mode, PR workflows
4. **Phase 4 - Deep Customization:** API usage, extensions ecosystem, workflow automation
