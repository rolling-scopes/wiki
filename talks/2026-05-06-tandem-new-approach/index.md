---
type: talk
title: Tandem & The New Approach
date: 2026-05-06
speaker: Dzmitry Varabei
status: draft
last_updated: 2026-05-06
---

# Tandem & The New Approach

### Slide 1 — Title

**RS Tandem**
A new format for project-based learning at Rolling Scopes School.
Dzmitry Varabei — coordinator, leader of the Rolling Scopes community.

---

### Slide 2 — The problem we started from

Most school tasks are **formal requirements**.

Example: [async-race task](https://github.com/rolling-scopes-school/tasks/blob/master/epam/async-race.md)
- Create a repository.
- Build a specific widget / view.
- Use React.
- Use Conventional Commits.

What this means for the student:
- I am graded on whether the requirements are checked off.
- Finishing the checklist is the goal.
- An AI assistant can finish the checklist for me.

What this means for us, the school:
- We cannot tell whether the student wrote the code, copied it,
  or generated it.
- A passing grade no longer proves the student learned anything.

> *"Maybe not today, but soon every student will have their own
> personal AI agent. So what, exactly, are we measuring?"*
> — internal mentor weekly, April 2026

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

**What Tandi does not do: grading.**
Scoring is a separate deterministic component, by design.
Tandi notices and writes; rules assign points.
A student's score never depends on an LLM's mood.

Tandi is not a replacement for mentors. Tandi is the layer that
**lets** mentors stop doing checklist review and start doing the
work only humans can do — defences, judgement, mentorship.

---

### Slide 11 — What we learned

> Before, RS School produced people who could write code.
> Code was the scarce thing.
> Code is no longer scarce.
> What's scarce now is people who can think like engineers.

Two beliefs we are testing in production:

1. **You cannot grade output anymore. You can only grade understanding.**
   AI broke the old contract. Diaries + defences are our answer.

2. **Self-paced is a euphemism for "left alone".**
   Teams, public diaries, and a calendar of defences put students
   in a room with other people again — even online.

**One honest failure along the way.**
We also tried to make AI grade the diaries themselves — to score
"depth of reflection." It gave a 0 to a student who wrote two entries
every week and had passed every prior checkpoint. The students
pushed back. We listened. The lesson stuck: judgement lives with
humans. AI helps them see — it does not replace seeing.

**What our latest cohort retro tells us** (56 respondents):
- Mentor / student technical interviews — **4.88 / 5** (highest of any component, 49/56 gave full marks).
- Automated tests — **3.73 / 5** (lowest). Direct quote: *"LLMs killed the point of tests."*
- The signal is consistent: students value human dialogue. They distrust
  formats that AI can quietly solve for them.

---

### Slide 12 — Thank you
