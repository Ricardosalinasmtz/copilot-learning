# MEMORY-copilot-learning.md

Project-specific memory for the GitHub Copilot Learning System.

<!-- project: github.com/Ricardosalinasmtz/sokka-orchestrator -->

## Decisions

- **Automation over external cron:** Uses OpenClaw's built-in automations for better integration and reliability
- **Weekly collection, daily delivery:** Research on Fridays, lessons Mon-Fri (not random/hourly)
- **Version-aware learning:** Track VSCode and Copilot version changes and prioritize new features
- **Curriculum approach:** Structured phases instead of random topic selection
- **Direct chat delivery:** Lessons delivered to main session, not webhooks
- **Automation architecture:** Delegation pattern via sokka-orchestrator, not direct agent assignments
  - Weekly research runs in sokka-orchestrator session (`agent:sokka-orchestrator:main`) but delegates to zuko-researcher via `sessions_spawn`
  - Daily lesson runs in sokka-orchestrator session (`agent:sokka-orchestrator:main`)
  - No direct zuko-researcher session keys in automation definitions
- **Communication pattern:** User ↔ sokka ↔ zuko (no direct user-zuko interaction)
- **Session configuration:**
  - Weekly research: sokka-orchestrator (agent:sokka-orchestrator:main), delegates to zuko
  - Daily lesson: sokka-orchestrator (agent:sokka-orchestrator:main)

## Technical Learnings

- OpenClaw automations support cron schedules with timezone specification
- Automation payloads can include detailed step-by-step instructions
- State tracking via JSON files in project directory
- VSCode 1.136.1+ has Copilot Chat built-in (no separate extension needed)
- GitHub Copilot Business (Enterprise) provides full feature access

## Environment

- **OS:** Ubuntu 24.04.4 LTS
- **Copilot Subscription:** GitHub Copilot Business (Enterprise)
- **VSCode version / Copilot mode:** see `state/progress.json` → `currentVersion` (do not duplicate it here — the weekly research job updates that field, not this file)

## State File Fixes (2026-09-22 / 2026-10-02 Audit)

The `state/progress.json` file had several inconsistencies discovered during a thorough audit. Fixes 1-6 applied 2026-09-22; fix 7 applied 2026-10-02:

1. **`currentPhase` stale:** Was `"phase_1_core"` but phase 1 was already `completed` and phase 2 was `in_progress`. Changed to `"phase_2_vscode_integration"`. The daily lesson automation reads this for lesson numbering.
2. **Missing phase completion record:** "Copilot in Integrated Terminal" (delivered 2026-09-21) existed at root level but not in `phase_2_vscode_integration.completedTopics`. Added it.
3. **Duplicate queue entry:** "Custom Instructions for Copilot (Personal, Repository, Organization)" appeared twice — one with the wrong source URL (`prompt-engineering` docs instead of `response-customization`). Removed the incorrect entry; kept the correct one (priority 5, `response-customization` source).
4. **Root `completedTopics` redundant:** Root-level array duplicated all phase completion records. Removed to establish a single source of truth (per-phase `completedTopics`). Stats are tracked via `stats.totalLessonsDelivered` instead. The daily lesson and research automations must be updated to no longer append to root `completedTopics`.
5. **`versionHistory` always empty:** `state/progress.json.versionHistory` was never populated (version tracking happens in the separate `state/version_history.json`). Removed to eliminate dead data.
6. **Queue items lacked `addedAt`:** No record of when topics were added, making it impossible to distinguish stale vs fresh queue entries. Added `addedAt` field (ISO date) to every queue item. This also helps break priority ties — when two items share the same priority, the older entry (earlier `addedAt`) is delivered first.
7. **`environment` duplicated `currentVersion` (2026-10-02):** `environment.vscode` (`"1.136.1"`) and `environment.copilotIntegration` (`"Built-in (VSCode native)"`) carried the same two facts as `currentVersion.vscode` and `currentVersion.copilot` — a drift risk of the same class as fixes 1 and 4. Removed the two duplicated keys from `environment`, which now holds only static setup context (`os`, `copilotSubscription`).

### Why `currentVersion` was kept and `environment` trimmed

The two blocks have different intents: `environment` is a static, human-readable setup description, while `currentVersion` is the **mutable baseline the weekly research job diffs against** each Friday before appending to `state/version_history.json`. Keeping the machine-updated field in its own clearly-named block avoids mixing automation state into human setup notes.

Verified before removal that nothing consumed the deleted keys: the only occurrence of `copilotIntegration`/`environment.vscode` in the repo was the definition itself, `scripts/` is empty, and no code reads `progress.json`. By contrast the weekly research automation payload names `currentVersion` explicitly ("Compare with state/progress.json currentVersion"), so that field must stay.

**Check before editing either block:** the weekly research payload also hardcoded the environment as prose ("Ubuntu 24.04.4 LTS, VSCode 1.136.1, Copilot built-in..."). That was literal prompt text, not a field lookup, so it did not break — but it was a third copy of the same facts. Removed in fix 8 below.

## Automation Fixes (2026-10-02)

8. **Weekly research payload hardcoded a VSCode version:** The brief handed zuko `VSCode 1.136.1` as context while also telling it to diff the live version against `currentVersion`. Survivable on the first upgrade, but once state advanced the prose would disagree with state and risk spurious "version changed" reports. Removed the version from the brief; the payload now says explicitly: *"Do NOT assume a VSCode version from this brief. The version baseline lives in state/progress.json currentVersion — read it."*
9. **Version-change branch was never implemented:** Step 1 said "If changed: update state/version_history.json" but nothing advanced `currentVersion`, so the next run would re-detect the same change forever. Step 1 now spells out both branches: append to `version_history.json` **and** advance `currentVersion` to the new baseline. **Verified working** — the 2026-10-02 run detected 1.136.1 → 1.139.1, wrote the history entry, and advanced the baseline. This closes the open item previously logged here.
10. **Research run overwrote the queue instead of appending (data loss):** The 2026-10-02 run replaced the queue with only its 8 new findings, silently dropping two undelivered topics (Agent Hooks, MCP — both `addedAt: 2026-09-18`). They were never delivered and were absent from every phase's `completedTopics`. Notably the run's own summary claimed it had "verified against existing queue (Agent Hooks, MCP) — no overlaps", so zuko believed it was appending; the instruction "Add topics to queue" was simply too weak to force a read-modify-write.
    - **Recovered:** both topics restored at the head of the queue with their original `addedAt` preserved, so FIFO delivery picks them before the newer batch.
    - **Prevented:** step 5 rewritten to "Update State (APPEND, never replace)" with an explicit read → build (existing first, new appended) → write → re-read sequence, plus a length assertion (`new == previous + added`, restore if lower). Steps 6 and 8 now require reporting queue length before and after, so a future drop is visible in the summary rather than silent.
11. **Stale version references in docs:** README "Current Status" and this file's `## Environment` both still claimed 1.136.1 after state moved to 1.139.1 — three files disagreeing. Rather than re-hardcode the new number, both now point at `state/progress.json` → `currentVersion` as the single source of truth. Static facts (OS, subscription tier) stayed inline since they do not drift.

**Lesson for future automation payloads:** an instruction that reads as "write X" will be executed as a replace. Where state is cumulative, say *append*, name what must survive, and require a post-write verification the agent can fail.

## Queue Ordering (decided 2026-10-02)

**Priority wins; `addedAt` only breaks ties.** The daily lesson payload says "first item from queue (highest priority, oldest first)" and the job reads it exactly that way: on 2026-10-02 it delivered a `priority: 1` topic sitting at index 2 ahead of two `priority: 2` topics at the head of the array. That is intended behaviour, confirmed and kept — do not reorder the array expecting position alone to control delivery.

Consequence to remember: restoring an entry to the front of the queue does **not** make it next. The two 2026-09-18 topics (Agent Hooks, MCP, both `priority: 2`) sit behind every `priority: 1` topic added since. They are safe — nothing drops them — but they yield until the higher-priority batch drains. To actually advance a topic, raise its `priority`; moving it is not enough.

## Lesson Format Standards (decided 2026-10-02)

**Quick Check uses a collapsible answer.** The original payload said only `[One reflection question or mini-quiz to reinforce learning]`, and `00_master_strategy.md` said `reflection question or quiz`. That "or" licensed two genres, specified no formatting, and never said whether to include the answer — so all twelve archived lessons improvised independently and drifted on three axes: genre (open vs multiple-choice), formatting (plain / blockquote / `**Question:**` label), and answer disclosure (none / hint / full answer in the open).

Disclosure was the axis that actually mattered: six lessons withheld the answer, five printed it directly below the question, destroying the retrieval practice the section exists for. The sibling AI Act learning automation had already solved this with a `<details>` disclosure; that pattern existed in the system but was never written back into this payload.

The daily lesson payload now pins all three axes:
- Exactly ONE open-ended question, plain paragraph, no blockquote, no bold label, never multiple choice.
- The answer is ALWAYS present and ALWAYS inside `<details><summary>Click to reveal the answer</summary>` — never in the open, never as an inline `*(Answer: ...)*`, never as a hint that gives it away.
- Blank lines around the `**Answer:**` line are required for Markdown to render inside `<details>`.

Archived lessons were left in their original form; only the factual errors below were corrected.

**Footer is derived, not guessed.** Exact form: `*Lesson [X] of Phase [N] ([Phase Name]) | Streak: [Z] days*`. The payload now computes lesson number from `stats.totalLessonsDelivered + 1`, phase name verbatim from the `phases` entry matching `currentPhase`, and forbids "Phase X + Phase Y" — a lesson belongs to exactly one phase.

**Selection rule made explicit.** Step 2 previously said "first item from queue (highest priority, oldest first)", which reads ambiguously next to an ordered array. It now states that the lowest priority number wins, `addedAt` breaks ties, and array position does not determine selection — matching the behaviour confirmed on 2026-10-02.

### Archived-lesson corrections (2026-10-02)

Two factual errors, both in the 09-16→09-18 window — the same stretch where `currentPhase` was stale:

| File | Was | Now |
|------|-----|-----|
| `2026-09-18_natural-language-to-code-in-comments.md` | "Lesson 8 ... Phase 1 (Core Features) + Phase 2 (VSCode Integration) \| Streak: 8" | "Lesson 6 of Phase 1 (Core Copilot Features) \| Streak: 6 days" |
| `2026-09-16_context-management-with-at-symbols.md` | "Streak: 5 days" (and `Day: 5` in header) | "Streak: 4 days" (and `Day: 4`) |

09-18 was the 6th delivery, not the 8th, and claimed two phases at once. 09-16 was day 4 of the streak (Fri 09-11, Mon 14, Tue 15, Wed 16), not day 5. Also normalised `2026-09-15` from `Phase 1` to `Phase 1 (Core Copilot Features)` for consistency — cosmetic, not an error.

Note: `2026-09-22` also claims "Lesson 8", which collided with the wrong 09-18 number but is itself **correct**. Verified all 12 footers against `phases[].completedTopics` sorted by date after the fix: zero mismatches.

**When auditing numbering, derive the true order from state rather than trusting the files** — sort every phase's `completedTopics` by `completedDate` and compare against each footer. Reading the archive alone will not reveal an off-by-two.

## Commit Ownership (decided 2026-10-02)

**Two commits per week, each explicitly staged.** The weekly research payload previously said only "Git commit with message 'Weekly research: added X topics to queue'" — a message with no staging scope. The 2026-10-02 run therefore committed 16 files: its own two outputs plus 10 untracked lesson archives, README.md, and MEMORY-copilot-learning.md while the latter two were being hand-edited mid-review. Unrelated work was absorbed under a message claiming it was queue additions, and the run gave no signal it had done so.

Step 7 now specifies:
- **Commit 1 (research output):** stage `state/progress.json`, `state/version_history.json` (only if changed), and the summary file just written. Message unchanged.
- **Commit 2 (lesson archive):** stage `lessons/`, message `Archive lessons for week ending YYYY-MM-DD`. Guarded by `git diff --cached --quiet` so a week with nothing new produces no empty commit.
- `git add -A`, `git add .`, and `git commit -a` are banned by name, with the reason stated in the payload — a bare prohibition tends to get "helpfully" widened; naming the failure mode does not.
- After both commits, run `git status --short` and report anything still dirty under "Left uncommitted:" rather than staging it. `MEMORY-copilot-learning.md` and `README.md` are named as expected-dirty and off-limits — they are hand-edited, which is exactly how the 10-02 run swallowed in-progress work.
- Step 8 requires reporting both commit hashes plus anything left uncommitted, so a future over-reach is visible in the summary.

**Why weekly batching rather than per-lesson commits (option B).** The daily lesson job has no commit step at all — it writes `lessons/<file>.md` and stops. `00_master_strategy.md` already assigned the archive to the weekly cadence ("Weekly: Commit lesson archive (batch lessons from the week)"), so lessons are *designed* to sit untracked until Friday sweeps them. Chosen for cleaner history: two automated commits per week plus manual ones, rather than one per lesson.

Consequence: a lesson delivered after Friday 12:30 waits a full week for its commit — the daily job runs at 13:00, after the research job. Do not read untracked files in `lessons/` as evidence of a failure.

The payload carries that explanation inline (`this directory is written by the daily lesson job, which does not commit; batching it here is intentional`) because the step immediately above it says *stage only your own output*. Without the note, a careful future run finds unowned files in `lessons/` and has two plausible-but-wrong readings available: someone's work in progress, or a crashed earlier run. The clause answers that before it is asked. It changes nothing mechanically.

**On `MEMORY-<project>.md` and the AGENTS.md tiers.** The Tier 1 "stage and notify, never auto-commit" rule covers the *agent workspace* files `MEMORY.md` and `USER.md`, which hold personal user context. `MEMORY-copilot-learning.md` is a project artifact in the project repo and is not in any tier — AGENTS.md describes it separately as project decisions and technical learnings. It is normal to commit it alongside the work that changed it. The 10-02 problem was not that it was committed, but that it was committed mid-edit by a job that did not know it was touching it.

## Research Summary Filenames (decided 2026-10-03)

**New summaries are `research_summary_YYYY-MM-DD.md` — no sequence number.** The payload previously hardcoded `01_research_summary_YYYY-MM-DD.md`, so all three existing summaries carry `01_`: the numbering never incremented and is now meaningless.

The rejected alternative was to keep `NN_` and compute the next number by listing the directory, parsing prefixes, taking the max, and incrementing. That rule has already failed three times out of three. Any rule requiring that computation can fail the same way again regardless of wording; a rule requiring no computation cannot. The date is already unique, already in the filename, and already sorts correctly — the number adds nothing over it.

Note `XX_` in the workspace AGENTS.md is specified for *phase* plans and summaries, which are genuinely ordinal. Research summaries are a weekly time series (~52/year) where the date is the natural key.

**The three legacy `01_` files stay as they are** — renaming rewrites history for no benefit, and the dates already disambiguate. The payload explicitly tells future runs not to renumber them or read them as a sequence to continue. Accepted cost: that folder carries a mixed convention permanently.

## Queue Depth Rule (decided 2026-10-03)

**Research adds 3 or 7 topics, chosen by queue depth.** Step 4 now reads: if the queue holds 5 or more items, add 3 (preferring version-change or current-phase-gap topics); if it holds fewer than 5, add 7, never more.

The old rule said "Extract 5-7 Topics" and was not honoured — the three completed runs added 7, 8, 8. More importantly it had no feedback from queue depth, so with ~5 lessons consumed per week the backlog could only grow.

**What actually drives convergence is the add amount relative to consumption, not the threshold.** An earlier draft of mine (threshold 10, add ~6) was simulated over 10 weeks and drifted upward without settling — 6 exceeds the ~5/week consumed at every level below the cap, so the queue climbs until it hits the ceiling and sawtooths high. The adopted 3-vs-7 straddles consumption: below it when the queue is healthy, above it when thin. Simulated against the real cadence (−4 Mon–Thu, research reads the queue Friday 12:30, −1 at 13:00), it settles into a stable 7↔9 cycle by week 2 and stays there.

When revisiting this, simulate before trusting intuition — tuning the threshold feels like the lever and is not.

**Header says "3 or 7", not "3-7".** The rule only ever emits 3 or 7; a range invites splitting the difference, which breaks convergence. The cap sentence lives inside the `fewer than 5` bullet so it cannot be misread as applying to the `5 or more` branch.

**No "deferred candidates" mechanism.** An earlier draft asked the job to note surplus topics in the summary for a future week. Dropped: nothing reads summaries back, so those topics would never return — the note would only record what was discarded. Real deferral needs a second array in `progress.json`; treat as separate work if ever wanted. Surplus candidates come from the same docs and will resurface in a later run.

**Known gap: no outage cushion.** If research fails for two consecutive weeks the queue reaches zero and lessons stop for roughly four days; recovery runs at +2/week, so rebuilding takes ~3 weeks. A lower threshold does not meaningfully help. The real protection is noticing the job is failing — see the 2026-09-25 incident in Operational Notes, which ran four weeks undetected.

## `estimatedLessons` Removed (2026-10-03)

**Deleted — it never affected behaviour.** Every queue entry carried `estimatedLessons: 2` or `3`, the research job's guess at how many daily lessons a topic needed. Nothing ever read it: the daily job dequeues one topic, writes one lesson, marks it complete. The delivery record is unambiguous — **12 lessons for 12 topics**, exactly one each. The field appeared nowhere outside `progress.json` — not in the daily payload, not in `00_master_strategy.md`, not in any script.

Worse than unused, it was misleading: the 9 queued topics summed to 25 implied lessons, so anyone reading the queue to judge backlog depth got a number off by 2.8× — five weeks of material rather than under two.

**Applied:** stripped from all 9 queue entries; removed from the research payload's required-fields list. Queue entries now carry exactly `topic`, `source`, `difficulty`, `section`, `priority`, `addedAt`. The payload names that list explicitly and tells future runs not to re-add `estimatedLessons` or invent other fields — without that, a run reasoning about a dense topic would plausibly reintroduce it.

**Compensating change:** since the estimate is gone, scoping moves to research time. Step 4 now states each topic is exactly ONE daily lesson, and that an area too large for one lesson must be split into separate topics rather than filed as one broad entry. This is the useful half of what `estimatedLessons` was gesturing at — a topic like "Copilot CLI Deep Dive" marked `3` was really an admission it was scoped too broadly.

**Rejected alternative (option B): make the field real** — daily job delivers "Part 1 of 3", decrements a counter, dequeues only on the last part. Genuinely useful for dense CLI topics, but needs per-topic progress state in `progress.json`, weekend-resume logic, and interacts with the lesson numbering fixed earlier today. Treat as separate feature work if ever wanted; do not bolt it on.

## Phase 2 Reopened (2026-10-03)

**Phase 2 was marked `completed` having taught 1 of its 6 curriculum items.** `00_master_strategy.md` lists six Phase 2 topics; only "Copilot in integrated terminal" was ever delivered. Debugging assistance, refactoring workflows, test generation patterns, multi-file edits, and Copilot Edits mode were never taught. The other two topics filed under `phase_2` do not belong to it — *Prompt Engineering* is Phase 4 material per the strategy, and *Copilot Memory* is not in the curriculum at all.

Cause: the research job reads the curriculum for inspiration, but nothing checks coverage before a phase closes. The daily payload says "if this delivery completes a phase, mark it completed and advance" with no definition of complete behind it — so it rests on an agent's in-the-moment judgement, which is exactly what closed Phase 2 at 1/6.

**Applied (option C — recover the material, leave the machinery alone):**
1. `phase_2_vscode_integration.status` → `in_progress`; `currentPhase` → `phase_2_vscode_integration`
2. Queued the 5 missing curriculum topics at `priority: 1`, `section: phase_2_vscode_integration`, `addedAt: 2026-10-03`
3. Demoted the 2 queued `priority: 1` Phase 3 topics (Agentic Workflows, Dynamic Workflows) to `priority: 2`
4. Left `phase_3_advanced` as `in_progress` with its 3 completions — those lessons were really delivered
5. Left *Prompt Engineering* and *Copilot Memory* filed under Phase 2 — re-filing is the phase-assignment fix's business, not this one

Queue 9 → 14.

**Why the demotion was necessary.** The two Phase 3 topics were already `priority: 1` with `addedAt: 2026-10-02`. Adding the recovery topics at priority 1 would have tied, and the documented tie-break is oldest `addedAt` first — so Phase 3 would still have delivered first and recovery would not have started until Wednesday. That is worse than a two-day delay: filing uses `currentPhase`, so with Phase 2 reopened those Phase 3 lessons would have been filed *into Phase 2*, making the coverage record worse than before the fix. Demoting uses the documented rule rather than working around it, and is trivially reversible.

**Sources were initially wrong, then verified.** The five entries were first queued with `code.visualstudio.com/docs/copilot/...` URLs constructed from the curriculum item names. Checked before Monday's delivery: all five were broken. One was a 404; four returned **HTTP 200 after silently redirecting** to different pages (two of them to the same generic chat overview). VS Code had moved its docs under `/docs/agents/` and `/docs/chat/`, so the whole `/docs/copilot/...` namespace was gone. All five were repointed to pages fetched and confirmed to resolve without redirect, using links found in the live site navigation (web search was disabled in that session):

| Topic | Source |
|---|---|
| Debugging Assistance | `/docs/agents/guides/fix-a-bug-with-agents` |
| Refactoring Workflows | `/docs/chat/inline-chat` (closest page; not refactoring-specific) |
| Test Generation Patterns | `/docs/agents/guides/test-code-with-ai` |
| Multi-File Edits | `/docs/agents/run/review-code-edits` |
| Copilot Edits Mode | `/docs/agents/overview` (landing page; Edits mode seems to have merged into agent mode) |

The refactoring and Edits-mode lessons are the weakest matches. Check those two lessons when they are delivered.

**This does not prevent recurrence.** Phase 3 can close the same way Phase 2 did. A real guard means tracking curriculum coverage in state and refusing to mark a phase complete with uncovered items (the rejected option B). Separate work; recorded here so the gap is not mistaken for closed.

## Source URL Verification (decided 2026-10-03)

**No step anywhere verified source URLs.** Research recorded a URL, and delivery first touched it days later. The only guard was the daily job's "Source doc missing: skip topic" edge case. That only catches a *missing* page. The real failure is a **200 after a redirect**: the job reads a real page about something else and writes a confident lesson from it, and nothing detects the mismatch. Checking for 404 alone would have caught only 1 of the 5 broken URLs above.

**Two guards added:**
- **Research, step 4 (when the topic is queued):** fetch every source URL before recording it. It must return 200 *and* end on the same URL that was requested. On a redirect or 404, find the correct page in the live docs. Never record an unfetched URL, and never build a path by guessing from a topic name.
- **Daily, step 3 (when the lesson is delivered):** on a redirect or 404, do not write a lesson from the page it landed on. Find the correct page, use it, and report the corrected URL so the queue entry can be fixed. If no suitable page exists, fall back to the skip-and-notify edge case. This second check exists because a URL can go stale between queueing and delivery.

The 2026-10-02 batch of 8 research topics was recorded before these guards existed, so its URLs were never checked. The daily guard covers them at delivery time.

## Phase Assignment (decided 2026-10-03)

**Problem: two different answers to "which phase does this lesson belong to?"** Each queued topic had its own `section` field, written as free text (`"Phase 4 - Deep Customization"`). There was also one global `currentPhase` pointer. The daily job used only `currentPhase`: it filed every completion under it and printed it in the footer. Delivery is priority-first, so phases interleave, and a single pointer cannot label a mixed stream correctly. This had already happened: *Prompt Engineering* (Phase 4 material) and *Copilot Memory* (not in the curriculum) were filed under Phase 2 because that was `currentPhase` on the day they were delivered. That is part of why Phase 2 looked like it had 3 topics when it covered 1 of 6.

**Considered and rejected: phase-first delivery (phase > priority > addedAt).** This would have stopped the interleaving at its source. It was rejected because new-version features would lose their fast lane: a Phase 4 feature from a new VS Code release would wait until Phases 2 and 3 were empty, so lessons would lag behind the Copilot actually in use. Later phases could also wait for weeks. **Delivery stays priority > addedAt.** Phases are expected to interleave, and consecutive footers may show different phases.

**Adopted: each topic carries its phase, plus a defined completion check.**
- `section` must be exactly one of `phase_1_core`, `phase_2_vscode_integration`, `phase_3_advanced`, `phase_4_deep`. Research writes only these values. The 9 free-text entries were converted.
- The daily job files each lesson under its own `section` and takes the footer's phase from it. It falls back to `currentPhase` only if `section` is missing or invalid, and says so in its reply.
- A lesson filed into a `locked` phase changes that phase to `in_progress`. `locked` now simply means nothing has been delivered there yet.
- **Phase completion has a definition:** a phase is complete only when every curriculum item listed for it in `00_master_strategy.md` has a delivered lesson in that phase's `completedTopics`. Topics outside the curriculum do not count. If unsure, the job does not mark it complete. This is the guard that would have stopped Phase 2 from closing at 1/6.
- `currentPhase` now has a narrower role: it marks the earliest phase whose curriculum is not covered yet. Research uses it to find gaps. When it becomes complete, it moves to the lowest-numbered phase that is not complete.
- Research gives topics that fill a curriculum gap priority 1 or 2, never lower, so newer topics cannot starve a gap and hold a phase open forever.

**Left as is, by decision:** *Prompt Engineering* and *Copilot Memory* stay filed under Phase 2, matching their archived lesson footers. They do not affect anything going forward. Under the new rule neither is a Phase 2 curriculum item, so neither counts toward closing Phase 2.

**Not built: full curriculum tracking** (a `curriculum` array per phase in `progress.json`, with items ticked off as lessons are delivered). The completion check above relies on the agent matching lesson titles against curriculum text. If a phase ever closes with gaps again, that matching is the weak point, and structured tracking is the next step.

## Repository Hygiene

- `.gitignore` added 2026-10-02 covering `tmp/`, `trash/`, and OS/editor noise, matching the workspace convention that those directories are never committed. Before this the project had none — `tmp/` stayed clean only because it happened to be empty. Verified with `git check-ignore`.
- Debug backups belong in `tmp/`. Check whether git already holds the same content before keeping one: the 2026-10-02 `progress.json` backup turned out byte-identical to commit `7e0d564`, so it was redundant and deleted.

## Dedicated Delivery Sessions (2026-10-05)

- Both jobs moved off Home (`agent:sokka-orchestrator:main`) to dedicated sessions in sidebar group `copilot-learning`:
  - Daily lesson `c5b85c26` → **Copilot Daily Learning** `agent:sokka-orchestrator:dashboard:c8cb22e8-93e8-4991-8948-0f6bfd19d7f6`
  - Weekly research `9960fa77` → **Copilot Weekly Research** `agent:sokka-orchestrator:dashboard:65090175-48ba-4ba5-a1cb-71c51857db14`
- Only `sessionKey` changed; `sessionTarget: current`, `delivery: announce`, schedule, payload and owner unchanged. Rollback = set `sessionKey` back to `agent:sokka-orchestrator:main`.
- Why: Home was at ~113k/128k tokens and mixed with heartbeats and other projects' output.
- **Archiving either dedicated session disables its job**, and restoring does not re-enable it (`openclaw automations enable <jobId>`).
- Job owner is still the old setup session `…:dashboard:09f0d840-…` ("Copilot-learning"). Docs don't list the owner session among archive triggers, but verify before archiving it.
- Sessions created by the user from the group's **+**; the group accepted them (and this review chat) despite their parent being Home.
- **Live test passed 2026-10-05 13:00:** run `ok`, `delivered`, 76 s; lesson 13 ("Debugging Assistance with Copilot") landed in Copilot Daily Learning, not Home.
- **Daily job thinking raised low → medium (2026-10-05)**, set by the user after the test; read back as `payload.thinking: "medium"`, nothing else changed. Reason: past failures were reasoning slips (phase guessing, premature phase completion, queue overwrite), not formatting. **Weekly job also raised low → medium (2026-10-05)** by the user, read back as `payload.thinking: "medium"`. Reason: per docs/tools/subagents.md, native sub-agents inherit the caller turn's thinking unless `agents.*.subagents.thinking` is set (it isn't), so zuko-researcher was running URL verification, queue append and staged commits at low.
- **Scheduled check runs can't inspect other jobs:** the 13:10 verify run got "not found" on the daily job, because automation runs only see their own job. Run cross-job verification from an interactive chat turn instead.

## Operational Notes

- **Transient `model-resolution` stalls:** the weekly research job failed 4 consecutive runs (2026-09-25 onward) with `cron: isolated agent run stalled before execution start (last phase: model-resolution)`, ~12 min per stall. It recovered on its own at the 2026-10-02 run with no config change. Worth watching, not yet worth chasing.
- **Failure was invisible because the sibling job was healthy:** daily lessons kept arriving from the existing queue while the refill job was dead, so the outage only surfaced when the queue ran near-empty. When auditing, check *both* jobs' `lastRunStatus` and `consecutiveErrors`, not just whether lessons are showing up.

## Dead Ends / Avoided Approaches

- ~~Hourly random topic selection~~ → Too chaotic, no curriculum structure
- ~~External cron + webhook delivery~~ → Unnecessary complexity when OpenClaw automations exist
- ~~Single combined job~~ → Separation of concerns: research vs delivery


## Version Tracking

- Initial version: VSCode 1.136.1, Copilot built-in (2026-09-11)
- 2026-10-02: 1.136.1 → 1.139.1 — first real version change detected; brought Agentic Workflows, dynamic workflows, CLI `/fleet` and `/research`, BYOK, agentic code review, CLI extensions
- Current value: see `state/version_history.json` (full log) and `state/progress.json` → `currentVersion` (active baseline)
- Version check: Fridays at 12:30 PM during research phase

## Learning Phases

1. **Phase 1 - Core Copilot Features:** Inline suggestions, chat, slash commands, context management
2. **Phase 2 - VSCode Integration:** Terminal, debugging, refactoring, test generation
3. **Phase 3 - Advanced Workflows:** Custom instructions, Copilot Edits mode, PR workflows, enterprise features
4. **Phase 4 - Deep Customization:** API usage, extensions ecosystem, workflow automation

### Open: end-of-curriculum behaviour (noted 2026-10-05, deferred)

Nothing defines what happens once all 4 phases are `completed`. Current payloads would: keep delivering lessons while the queue has topics (priority-ordered, not phase-gated); keep researching and forcing new topics into one of the 4 finished phases; leave `currentPhase` undefined (the "lowest non-completed phase" rule has no answer, so the agent would guess); and send no completion notice. Decide before Phase 4 nears completion. Options discussed: (1) maintenance mode — `currentPhase: "maintenance"`, research queues only new-version features and doc changes to covered topics, one-time curriculum summary (recommended); (2) stop — final summary, disable both jobs; (3) add a Phase 5 for ongoing updates.
