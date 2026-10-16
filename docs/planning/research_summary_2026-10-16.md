# Research Summary: 2026-10-16

## Version Status

- **VSCode live:** 1.139.1 (from `code --version`)
- **Baseline in state/progress.json:** 1.139.1
- **Result:** Unchanged. No version history entry added, no currentVersion update.

Note for future runs: VS Code 1.142.0 was released October 14, 2026 (upstream stable) and includes several Copilot-relevant changes (Canvases preview, move chats between folders/worktrees, redesigned model picker, enterprise agent permission policies, Copilot harness becoming default). The live machine is still on 1.139.1, so no version-change topics were added this week. Once the local install is updated, these should be treated as highest-priority topics.

## Queue Length

- **Before:** 8 undelivered topics
- **Added this run:** 3 (queue held ≥ 5, so the 3-topic rule applied)
- **After:** 11 undelivered topics

## Topics Added (all verified: HTTP 200, final URL matches requested URL)

1. **Copilot Automations (Scheduled and Event-Triggered Cloud Agent Runs)**
   - Source: https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-automations
   - Difficulty: intermediate | Section: phase_3_advanced | Priority: 1
   - Rationale: Directly fills a Phase 3 curriculum gap (enterprise/automation workflows). Complements the already-completed "GitHub Copilot Policies" and "Custom Agents" lessons. Covers scheduled + event-driven cloud agent runs (issue triage, nightly test fixes, release notes) and the Business/Enterprise enablement requirements. Priority 1 because it covers an uncovered curriculum area.

2. **Agent Management (Centralized Control of Parallel Agent Sessions)**
   - Source: https://docs.github.com/en/copilot/concepts/agent-capabilities/choosing-and-delegating-agent-work/agent-management
   - Difficulty: intermediate | Section: phase_3_advanced | Priority: 2
   - Rationale: Fills the Phase 3 "team collaboration patterns" curriculum item — one centralized page to monitor and steer multiple concurrent agent sessions (Copilot, third-party agents, agent apps). Natural follow-on to the "Custom Agents" lesson from 2026-10-13.

3. **Remote Control of Copilot CLI Sessions (Steer from GitHub.com or Mobile)**
   - Source: https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-remote-control
   - Difficulty: intermediate | Section: phase_3_advanced | Priority: 2
   - Rationale: Pairs with the queued "GitHub Copilot CLI Deep Dive" topic (2026-10-02). Covers steering a local CLI session from GitHub.com/Mobile, including the "Store local sessions in the Cloud" admin policy relevant to our Business (Enterprise) subscription.

## Research Notes

- Phase 3 (Advanced Workflows) is the current phase; this run prioritized closing its remaining curriculum gaps (enterprise automation, collaboration) rather than Phase 4 deep-dive topics, which already have 5 queued items.
- All three sources live under docs.github.com's restructured "agent-capabilities" section; the legacy paths under `concepts/agents/` are being phased out, so the new canonical paths were recorded.
- No duplicate topics: none of the three appear in the existing queue or in any phase's completedTopics.
