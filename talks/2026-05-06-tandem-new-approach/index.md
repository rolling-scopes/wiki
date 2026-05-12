---
type: talk
title: Tandem & The New Approach
language: en
date: 2026-05-06
speaker: Dzmitry Varabei
status: draft
last_updated: 2026-05-12
siblings:
  - index.ru.md   # Russian translation of this document
  - slides.html   # reveal.js deck (English only)
---

# Tandem & The New Approach

> **Sibling artifacts** in this folder (kept in sync; last sync 2026-05-12):
> - `index.md` — English source (this file)
> - `index.ru.md` — Russian translation
> - `slides.html` — reveal.js deck (English only; not translated)

### Slide 1 — Title

**RS Tandem**
A new format for project-based learning at Rolling Scopes School.

---

### Slide 0 — Engineers, in the AI era

For many years, RS School has been training engineers — people you can rely on,
people you can trust to solve real problems.

In the era of AI, Andrej Karpathy puts the distinction sharply:

> **Karpathy: vibe coding ≠ agentic engineering**
>
> Vibe coding raises the floor — anyone can vibe code anything.
>
> Agentic engineering **preserves the quality bar** — you're still
> responsible for your software, but you can go faster.
> — Andrej Karpathy

---

### Slide [new] — The question we ask ourselves

How do we **maintain the quality of our graduates**, when AI can solve any coding task quickly?

---

### Slide 2 — The problem we started from

Most school tasks are **formal requirements**.

Example: [async-race task](https://github.com/rolling-scopes-school/tasks/blob/master/epam/async-race.md)

<small>

- Create a repository.
- Build a specific widget / view.
- Use React.
- Use Conventional Commits.

</small>

What this means for the student:
- I am graded on whether the requirements are checked off.
- Finishing the checklist is the goal.
- An AI assistant can finish the checklist for me.

What this means for us, the school:
- We cannot tell whether the student wrote the code, copied it,
  or generated it.
- A passing grade no longer proves the student learned anything.

---

### Slide 3 — What we want to measure instead

We want to evaluate **understanding, not output**.

- Not "did you ship a working component?"
- But "do you understand the technology you used,
  and the engineering problem it solved?"

The shift in one line:
**from knowing the answer → to explaining the answer.**

This is what AI cannot fake for the student.

---

### Slide 4 — How we evaluate understanding

We changed three things at once.

**1. Mentors stop reviewing PRs.**
AI agents do code review now. Human time is too valuable for that.

**2. Students keep a public diary, twice a week.**
What they did. What they studied. What confused them.
Where they separate "I know the term" from "I understand why it exists".

**3. Students defend what their diary claims.**
Multiple defences across the course — different audiences,
different formats. The diary becomes the script. The defence proves it.

---

### Slide 5 — The defences (real examples)

Every student goes through several formats:

- **Solo video** — explain a component you built.
  [example](https://www.youtube.com/watch?v=vFyB-l9kRy8)
- **Peer presentation** — to 5–6 classmates, in a small group.
- **Team presentation** — your team presents the project together.
  [example](https://www.youtube.com/watch?v=MCu5qHn87BE)
- **Mentor / jury defence** — technical interview with engineers
  who have already been through the same course themselves.
  [example](https://www.youtube.com/watch?v=LhSEHGZ-Hpk)
- **Optional public talk** — record for YouTube.
  [example](https://www.youtube.com/watch?v=_29uh9IrVms&t=2s)

Same project, retold to four different audiences.
By the fourth retelling, the student is no longer reciting — they own it.

---

### Slide [new-6] — Engineers and their journals

The diary is not a school invention. Serious engineers and scientists
have kept working journals for centuries. Six figures, six approaches —
chronological.

*(In the deck: six vertical sub-slides under this umbrella.)*

#### Leonardo da Vinci (1452–1519)

The great polymath of the Renaissance. Designed concepts for the tank,
the parachute, the spotlight, the helicopter — centuries ahead of his time.

**The journals:** over 7,000 surviving pages — the famous "Codices."
Written in mirror script (right to left). A single page might hold complex
engineering diagrams, anatomical sketches, philosophical musings,
and an everyday shopping list, side by side.

#### Thomas Edison (1847–1931)

Inventor (phonograph, the practical incandescent lamp), founder of the
first industrial research laboratory at Menlo Park.

**The journals:** about 5 million pages of lab notebooks. Required himself
and every engineer in his lab to document every idea and every failed
experiment. Kept in numbered notebooks.

#### Nikola Tesla (1856–1943)

Electrical engineer and inventor. AC power systems, the induction motor,
early radio work.

**The journals:** in the lab, daily and meticulous — exact circuit
parameters, diagrams, calculations, observations from experiments.
Tesla trusted his phenomenal memory outside the lab, but on the workbench
he demanded numbers on paper.

#### Richard Feynman (1918–1988)

Theoretical physicist, Nobel laureate, founder of quantum electrodynamics
and the famous Feynman diagrams.

**The journals:** treated paper not as a storage of thought but as the
work itself. *"I actually did the work on the paper. The paper isn't
a record of my thinking — it IS my thinking."*

#### Edsger Dijkstra (1930–2002)

Pioneer of computer science. Structured programming, the shortest-path
algorithm, the discipline of program correctness.

**The journals:** the **EWDs (Edsger W. Dijkstra Manuscripts)** —
numbered handwritten essays (over 1,300, from EWD0 to EWD1318).
Hand-wrote rigorous proofs of algorithm correctness before code ever
ran on a machine.

#### John Carmack (1970– )

Programmer behind Wolfenstein 3D, Doom, Quake. Pioneer of real-time 3D
graphics, aerospace engineer (Armadillo), VR pioneer (Oculus).

**The journals:** the famous `.plan` files. Through the 1990s, daily
plain-text log of his work, public over the Finger protocol — what was
fixed, what was optimized, what he was thinking that day.

---

### Slide 6 — Problem #2: loneliness

Self-paced courses have a quiet failure mode: the student
is alone with the material, and most students do not survive that.

People are motivated by other people.

A student who:
- works in a team,
- is accountable to teammates,
- writes a public diary,
- knows several defences are coming —

…is dramatically more engaged than a student studying alone.
Either they prepare and learn, or they self-select out early.
Both outcomes are honest.

---

### Slide 7 — How we fight loneliness

Four structural choices:

- **Team work.** Real teams of 3–6, real shared codebase, real conflict.
- **A mentor for every team.** We work hard to assign a dedicated
  mentor to each team — and our mentors are usually recent
  graduates of the school, not senior engineers from outside.
  A mentor's role is not to grade. It is to be a **coach and
  partner** who works through problems alongside the team.
  Because the mentor has just lived through the same course,
  they know — better than anyone — what actually matters and
  where students get stuck. Teams without a mentor consistently
  score lower in our retros.
- **Cross-team peer presentation.** A student presents their work
  to students from a different team — fresh audience, real questions.
- **Mentor / jury technical interview.** A panel of engineers —
  most of them course graduates themselves — asks the questions
  a real interviewer would ask.

Nobody finishes the course alone.

---

### Slide 8 — The project: RS Tandem

The course is built around one shared **theme** — but the project
itself is the team's to design.

- **The theme (we set):** an application to help engineers
  prepare for technical interviews.
- **Everything else (the team decides):**
  - What features the app actually has — flashcards, mock interviews,
    a question bank, an AI tutor module, voice practice, anything
    they can argue for.
  - What technology stack to use — framework, database, hosting,
    AI provider. We don't prescribe React or any specific tool.
  - How to split the work across 3–6 people.
  - What "done" looks like for their version of the product.
- **Who:** teams of 3–6 students.
- **How long:**
  - 6 weeks of development,
  - 3 weeks of presentations and defences.
- **Output:** working app, public diaries, recorded defences.


---

### Slide 9 — Timeline (the ideal flow)

How the course is meant to run, end to end:

| Phase | Duration | What happens |
|---|---|---|
| **Onboarding & team formation** | 2+ weeks | Intro, tooling, students pick teammates, teams stabilise. |
| **Development** | 6 weeks | Weekly challenges, mandatory diaries (×2/week), AI-assisted PR review. |
| **Presentation prep** | 1 week | Teams rehearse, polish demos, prepare individual stories. |
| **Peer-to-peer presentation** | — | Each student presents to a different team. Fresh audience. |
| **Team presentation** | — | The whole team presents the project together. |
| **Mentor / jury defence** | — | Technical interview with a panel of engineers, most of them course graduates themselves. |

---

### Slide 10 — Meet Tandi, our AI co-pilot

Mentors stopped reviewing PRs. Something had to fill the gap.

**Tandi** is an AI agent that:
- reads student diaries every week and writes editorial notes,
- publishes a weekly issue — like a school newspaper, but generated,
- responds to student GitHub issues directly.

*Live: [rollingscopes.github.io/rs-tandem-observer](https://rollingscopes.github.io/rs-tandem-observer/)*

Tandi is not a replacement for mentors. Tandi is the layer that
**lets** mentors stop doing checklist review and start doing the
work only humans can do — defences, judgement, mentorship.

---

### Slide [new-teaching] — How do we actually teach AI?

Our answer: **we lead by personal example.**

Our tools are open-source — **RS App**, our analog of `learn.epam.com`.
We build them with AI, in public, and students watch the work.

A few examples — not the full list:

- **Building our tools with AI** —
  [walkthrough](https://www.youtube.com/watch?v=ONisMK84C-Q&t=1s)
- **Live coding with Claude Code** — recorded sessions:
  [one](https://www.youtube.com/watch?v=gM1qxx9c_9g&t=4564s) ·
  [two](https://www.youtube.com/watch?v=CQodPVwj2ig)
- **Guest talks** — *tomorrow:* "Welcome the Dark Factory"
  by **Uladzimir Klyshevich**, Director, AI Technology Solutions at EPAM Systems

*Full archive:* [youtube.com/@RollingScopesSchool/streams](https://www.youtube.com/@RollingScopesSchool/streams)

Students apply any of these technologies in their own projects — and grow into better engineers.

---

### Slide 12 — Thank you
