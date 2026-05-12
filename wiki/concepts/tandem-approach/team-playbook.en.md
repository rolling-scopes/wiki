---
type: concept
title: Team playbook (EN)
status: active
last_updated: 2026-05-12
sources:
  - retros/2026-Q3-rs-tandem-retro.md
  - retros/2024-Q4-js-fe-school-feedback.md
  - audits/angular-2026-04-29-team-stage.md
related:
  - concepts/tandem-approach/team-playbook.md
  - concepts/checkpoint.md
  - decisions/mentor-as-defence-listener.md
  - decisions/mentor-staffing-policy.md
  - decisions/peer-review-precedes-defence.md
  - decisions/lock-rules-at-kickoff.md
  - decisions/team-warmup-before-final.md
---
# Team playbook: checklists and protocols

*Languages: [Русский](team-playbook.md) · English*

---

In this document, **W1, W2, W3 ...** are the numbers of weeks of team work (W1 — the first week of the team project). All artifacts and rituals are anchored to this scale.

## Week 1 checklist

By the end of W1 (the first week of team work), every item below should exist in the team.

### Team and roles

- [ ] Team lead is explicitly assigned. **Recommended model — a student from the team** who moves the meetings and the repository along; rotation every 2 weeks is fine, but at any given moment it's one person. By default, **the mentor is not the team lead** — under `decisions/mentor-as-defence-listener.md` the mentor's role is different (listening to defences once every 1–2 weeks, not running the project in between). If the mentor themselves wants the team-lead role and the team is happy with that — that also works.
- [ ] Task allocation is stated publicly in the chat. Not "everyone does everything", but "A — routing and shared, B — state, C — design and styles, D — CI and tests".
- [ ] A screenshot with everyone's time zones is pinned in chat.
- [ ] Each participant has written at least one diary entry.

### Repository

- [ ] Repository is public.
- [ ] `README.md` exists and is maintained as a living document: project description, team roster (with GitHub logins), stack, how to run locally, deploy link, dashboard link. Updated every time anything in this list changes — not "we'll fill it in before the defence".
- [ ] `DECISIONS.md` exists at the repo root — a log of the team's decisions: stack choice, architecture forks, scope changes, process agreements. The first entry is made in week one: "we chose X because Y, alternatives considered: Z". Every subsequent meaningful decision is a new entry with date and author.
- [ ] `main` branch is protected: direct push forbidden, merge only via PR with at least 1 approval. We follow GitHub Flow: feature branch → PR → `main`. A separate `develop` or release branches aren't needed for a course-length project; a staging branch for a live demo is optional if the team wants one.
- [ ] PR template (`.github/PULL_REQUEST_TEMPLATE.md`) with a minimal checklist: what was done, how it was tested, what the risks are.
- [ ] Branch-naming convention is fixed in README or CONTRIBUTING.md.
- [ ] Husky / pre-commit hook is set up: linter, formatter, types.
- [ ] CI runs on every PR: build, lint, tests.
- [ ] If you work with a coding agent: AGENTS.md or CLAUDE.md at the repo root with project base conventions.
- [ ] PR review by a coding agent is set up (**if anyone on the team has access to a paid subscription**). Which agent — your call: Claude Code in CI, a Codex review bot, GitHub Copilot (often free for students via the GitHub Student Developer Pack), any other. The agent provides the first line of review: types, tests, obvious bugs, convention violations. See `decisions/mentor-as-defence-listener.md` — why an agent, not the mentor or coordinator. If no one on the team has access to a subscription — this item isn't blocking: a human from the team holds the first review layer (see "Code review agreement" below).

### Rituals

- [ ] One weekly team call is fixed (day, time in time zone, length). No one skips it without warning.
- [ ] A ritual of regular one-line signal in the chat ("today I'm doing X, no blockers / there's Y"). Not for reporting, but so the team can see who's alive.
- [ ] Each team has its own Telegram channel or a dedicated Discord server: common, no DMs about project topics. DMs are for "let's hop on a call", not for discussing the project. All decisions and discussions should be visible to everyone.

### Decisions and architecture

The first three items are a **conversation among the whole team**, not a document from the team lead. Not a presentation, not an architecture artifact — the answers live wherever the team finds convenient (a page in `DECISIONS.md`, in the README, or in `docs/design.md`). If the answers diverge between participants — the conversation didn't happen, repeat it. By W3 it's already too late: you'll be redoing code that's already written.

- [ ] What we're building — described in 2–3 paragraphs: which user gets what from the product, the main scenario, what matters now, what doesn't matter in this version. Not "a task-management app"; be concrete.
- [ ] What parts it consists of — a list of the main modules or screens and how they connect. As a list or as a picture; the point is that the team walked through "how this will be put together" **together**, rather than one person thinking for everyone.
- [ ] What data we store — a list of main entities with their main fields (5–7 of them: "user", "order", "team" — what they have). Not a DB schema with types; a shared understanding of what the application operates on.
- [ ] Stack is chosen and fixed by an entry in `DECISIONS.md`. If the choice was contested — the entry lists the alternatives considered and why this one was chosen.
- [ ] Scope is broken into parallel modules without hard dependencies. Sanity check: if any one person drops out, the project still reaches the defence in a reduced form.
- [ ] Code review agreement is fixed in CONTRIBUTING.md — three-layer scheme:
  - **Linter automatically** — formatting, types. Not discussed in the PR.
  - **Coding agent (if the team has access to a subscription)** — first line of review: tests, obvious bugs, convention violations, simple logic issues.
  - **A human from the team** — on top of the agent, or instead of it if no agent: product logic, architecture, readability, names, domain nuances. Doesn't repeat what the agent already said.

---

## Every-week checklist

Once a week on the call:

- [ ] Has every participant made at least one diary entry? If someone hasn't — talk to them directly, not publicly.
- [ ] Task allocation for the coming week is spoken out and fixed in an issue or on a board.
- [ ] Is `README.md` still current? If the stack, team composition, how-to-run, or links changed this week — update right away, not "later".
- [ ] Are this week's decisions logged in `DECISIONS.md`? Any decision the team spent more than half an hour arguing over should end up recorded.
- [ ] Any PRs hanging open more than 3 days without review? If so — assign someone.
- [ ] No abandoned branches older than 2 weeks in the repository.
- [ ] Is the deploy working? When was it last verified live?
- [ ] Who's lagging? Who's ahead? Does the workload need rebalancing?

Once every two weeks, or after each defence:

- [ ] What will we change in how we work after this defence? One concrete item, not "we'll improve everything".

---

## Anti-patterns (things that look fine but lead into a wall)

| Anti-pattern | Why it's dangerous | What to replace it with |
|---|---|---|
| "We're all equal, no leader needed" | No one moves meetings, decisions stall, the team loses a week on every choice | Assign a team lead explicitly; can rotate every 2 weeks |
| "Let's code first, figure it out later" | By W3 different participants are working by different rules; reviews turn into flame wars | A minimal baseline in W1: branches, PR, linter |
| "Let's decide architecture by vote on a call" | Whoever lost the vote later works half-heartedly or sabotages | One person with a mandate and a deadline (2–3 days) brings an ADR entry to `DECISIONS.md` and a working prototype. The team can suggest edits and raise specific risks — final word is with that person. A failed decision is reopened with a new ADR, not "rolled back to a vote" |
| "Lecture from the team lead: I figured it out — let me explain" | Half-knowledge transfers poorly; the audience is shy to push back | Pair work on the problem when it actually arises |
| "Closed repo until we polish it" | Mentor, coordinator, and Tandi can't see if you're alive; first feedback lands at the defence | Public from day one; an imperfect README beats an empty one |
| "Six on the team — we'll go faster" | Coordination grows quadratically; more time goes into syncs than into code | Scope sized for the minimum working part of the team (for 4 — sized for three; for 6 — for four); the rest extend quality, tests, documentation. Criterion — losing one person doesn't break the defence |
| "We don't have a designer, we'll figure it out as we go" | Mockup gigantism and rebuilding the layout in W4 | Live design handoff, or an explicit UI owner with the right to decide |
| "Diaries are bureaucracy" | The silent person on the team isn't visible until the defence; burnout doesn't get caught | The diary is the script of your own defence |

---

## Protocols for recurring situations

### "One of us isn't responding"

1. Team lead writes them privately: "Hey, how are you? Anything I can help with?" Not publicly — going public reads as an accusation.
2. If no reply in 48 hours — write in the team chat @mentioning them: "{name}, let us know what's going on, we're worried about you".
3. If no reply in 4 days — the team lead writes the course coordinator. Until that point the team doesn't try to "fix it on its own".
4. In parallel, the team redistributes their tasks as if they weren't here. The returning person gets a short weekly digest of decisions and catches up.

**If the team lead is the one who vanished** — any participant kicks off the protocol as an acting team lead. At the next call (or in chat, if the call is too far off) the team picks one person to hold the team-lead role until the original returns. The coordinator is notified right away, even if the team lead turns up within a day — they need to know the role is temporarily held by someone else.

### "We have a 15-comment architecture argument in a PR"

1. Stop the thread in the PR. Someone on the team writes: "Moving this to a call / ADR".
2. One person is appointed to make the call on this question. They have 2–3 days and the right to bring a working prototype.
3. The decision is logged in `DECISIONS.md`: date, topic, what was chosen, why, what alternatives were considered, who decided.
4. Close the PR with this decision; remaining comments go into an issue.

### "We won't make it — what do we cut"

1. The feature list is split into three tiers: must (can't defend without this), should (without this the defence will be pale), nice (if time remains).
2. If in W6 it looks like you won't make must — cut should, and the team returns to defending must.
3. If you're cutting must — that's an immediate signal to the coordinator and mentor, not two days before the defence.
4. There's no such thing as "we'll catch up in the last week". Everyone who believed they'd catch up in the last week didn't.

### "A teammate left the team"

1. The team logs this explicitly (in chat, in README): who left, when, what of their tasks remains.
2. The team lead redistributes their tasks. If the remaining people can't handle the volume — they write the coordinator asking to trim scope.
3. Not "we're carrying for them" silently. Silent carrying = burnout in two weeks.

### "There's a conflict between two participants"

1. Not in code review. The PR isn't the place for relationship issues.
2. The team lead (or the mentor, if the team lead is one of the parties) sits down with both separately and listens. Doesn't try to "reconcile in one call".
3. If the conflict is about code — move it to `DECISIONS.md` with a clear owner decision. If it's about roles — renegotiate task allocation.
4. If the conflict is human — write the coordinator. This isn't "we have to handle it ourselves".

### "We don't understand what we're doing"

This is a normal state in W3–W4. The most common phrase in Tandem diaries: "I don't understand what we're doing". If one person feels this — it's normal; the diary is what that's for. If everyone feels it — that's a signal to call the mentor and rework scope.

---

## For the defence: what should be in the repo right now

Artifacts are set up in W1 (see the week-1 checklist). This section is a **second-pass** checklist before the defence: confirming the artifacts are alive right now, not just "were once".

- [ ] README is current: stack, team, how to run, deploy and dashboard links. Last commit to README is no older than a week.
- [ ] `DECISIONS.md` — entries span the whole project history (minimum 5–7 for a course-length project). At the defence the team can walk through it as a narrative.
- [ ] `CONTRIBUTING.md` describes the current gitflow and the current code-review scheme (including the coding-agent role, if used).
- [ ] The deploy works right now — opened in a browser before the defence, not yesterday, not "worked on Wednesday".
- [ ] Every participant has a diary entry from the last week.
- [ ] CI is green on `main` (at least the most recent build).
- [ ] The PR template is still in use; empty PRs haven't become the norm.

This isn't "required" — it's what makes the team legible to the mentor, the coordinator, the agent, and a new team member returning after illness.
