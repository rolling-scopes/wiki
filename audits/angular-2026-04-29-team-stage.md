---
type: source-summary
title: Angular team-stage audit (2026-04-29) against tandi-garden decisions
status: active
last_updated: 2026-04-29
sources:
  - https://github.com/rolling-scopes-school/tasks/tree/master/angular @ fce6d6aa1de9464c32de51a1849efad62652a2e3
  - wiki/decisions/lock-rules-at-kickoff.md
  - wiki/decisions/peer-review-precedes-defence.md
  - wiki/decisions/mentor-staffing-policy.md
  - wiki/decisions/score-philosophy-after-ai.md
related:
  - decisions/lock-rules-at-kickoff.md
  - decisions/peer-review-precedes-defence.md
  - decisions/mentor-staffing-policy.md
  - decisions/score-philosophy-after-ai.md
---

# Angular team-stage audit — 2026-04-29

> **Методология.** Аудит сверяет материалы Angular team-stage (`rolling-scopes-school/tasks/master/angular`, commit `fce6d6a`) с четырьмя decisions/ нашей federated wiki, выведенными после ретро Tandem 2026 Q3. Все finding'и — про **материалы, которые видит студент**, не про дизайн курса. Часть из них — про то, что у координатора в голове правильная модель, но в `.md`-файлах она не отражена явно. Это всё ещё проблема: студент-only-reading-the-docs не должен достраивать модель из устной коммуникации.

## Summary

Дима подготовил очень подробные технические checkpoints (Sprint 1–4): хорошо структурированные требования, FAQ, "What to Study" и self-check вопросы. Этап Components → Routing/Signals → Forms → HTTP/RxJS/Testing — методически выстроен. Это сильная часть, и она не требует доработок.

Тонкие места — в **обвязке** командного этапа: правила входа (certificate, benefits, calendar) разбросаны и местами неполны; ключевые механики курса (peer-review как репетиция, ротация менторов поверх primary, бинарный порог сертификата) есть в дизайне, но в материалах их нельзя восстановить.

Топ-3 приоритета до старта командного этапа (~12.05.2026):
1. **Менторская модель** — описать в материалах двойную структуру: primary-ментор у каждой команды + ротация менторов на sprint interviews. См. C1.
2. **Peer-review-репетиция** — описать в материалах раунд peer-review между концом Sprint 4 и финальной парой защит (jury + персональная). См. C2.
3. **Сертификат и календарь** — записать бинарный чек-лист сертификата (3 защиты + дневник/видео) и конкретные даты дедлайнов sprints. См. C3.

## Critical (recommended fix before team-stage starts)

### C1. Менторская модель не описана: "random mentor" без primary-контекста [violates AC: mentor-staffing-policy]

**File:** `angular/tasks/angular-team-task/SCORE_PERSONAL.md:8`, `angular/mentoring/README.md:69`, `angular/tasks/angular-team-task/TEAM_SETUP.md` (обрывается на Step 2)

**Quote (`SCORE_PERSONAL.md:8`):**
> «1. After each sprint deadline, a **pair is formed: team ↔ random mentor**.»

**Quote (`mentoring/README.md:69`):**
> «After each sprint deadline, the team is paired with a mentor (may be a random mentor, not necessarily their own).»

**Why this is a problem:** В дизайне Angular есть две роли ментора: (a) **primary** — постоянный ментор команды для регулярного сопровождения, (b) **rotating** — ментор, проводящий конкретный sprint interview, выбирается ротацией ради разнообразия обратной связи (primary тоже может присутствовать). Это рабочая модель — студент за курс получает обратную связь от 4–6 разных менторов плюс стабильный primary для повседневной поддержки.

**В материалах эта двойная структура не описана.** `SCORE_PERSONAL.md:8` и `mentoring/README.md:69` упоминают только «random mentor», `TEAM_SETUP.md` не доходит до Step с фиксацией primary. Студент, читающий только материалы, восстанавливает картину «у команды нет своего ментора, на каждый sprint берут случайного» — а это ровно паттерн команд без ментора с Tandem Q3 (см. `incidents/tandem-q3-mentor-shortage.md`). Дизайн-то нормальный, видимость — нет.

**Recommended change:**
1. В `TEAM_SETUP.md` добавить Step 3 (файл сейчас обрывается):
   ```
   ## Step 3 — Primary mentor confirmation
   Before Sprint 2 starts, the coordinator confirms every team has a primary
   mentor (or is explicitly marked self-driven). The primary mentor is the
   team's regular point of contact for support throughout the course.
   No team enters Sprint 2 without a named primary mentor.
   ```
2. В `SCORE_PERSONAL.md:8` уточнить формулировку:
   ```
   1. After each sprint deadline, the team is paired with a rotating mentor
      who conducts the interview (your primary mentor may also attend).
      Rotation is intentional — students get feedback from 4–6 mentors over
      the course.
   ```
3. В `mentoring/README.md` § Getting Mentees вынести разделение явно: «mentors take on a primary role for one team for the duration of the course AND rotate through sprint interviews of other teams».

См. также обновлённую `decisions/mentor-staffing-policy.md` § Ротация.

---

### C2. Peer-review-репетиция в дизайне есть, в материалах не описана [violates AC: peer-review-precedes-defence]

**File:** `angular/tasks/angular-team-task/PRESENTATION.md`, `angular/tasks/angular-team-task/README.md` (раздел Timeline), `angular/tasks/angular-team-task/SPRINT4_CHECKPOINT.md:39–40`

**Quote (`SPRINT4_CHECKPOINT.md:39`):**
> «4. **Code Review** — **Option A:** ≥3 meaningful comments in team PRs. **Option B:** a diary entry about how you studied another team member's code.»

**Why this is a problem:** В дизайне Angular team-stage есть peer-review-этап как **репетиция перед финальной парой защит** (командная презентация перед jury + персональная перед ментором сразу следом). По AC `peer-review-precedes-defence.md` это и есть правильная схема: один раунд peer-review, зазор 3–7 дней, потом финальная защита.

**Но в материалах этого этапа нет.** Единственное «review» в team-task — это PR-комментарии **внутри команды** (Sprint 4, line 39–40). Это полезный навык, но не peer-review в смысле AC: студент не видит чужие команды, чужие подходы, не репетирует свою презентацию. `PRESENTATION.md` описывает только финальную jury-сессию без шага-репетиции перед ней. Студент-only-reading-the-docs о peer-review-репетиции не узнает.

**Recommended change:** Добавить peer-review-этап в материалы — как описание того, что уже планируется в дизайне. Конкретно:

1. В `angular-team-task/README.md` § Timeline добавить запись между Sprint 4 и финальной презентацией:
   ```
   ### Week 9 — Peer Review (rehearsal)

   - Each team presents its project to one other team (~30 min).
   - Format: live demo + Q&A. No scoring. Goal: rehearsal before the
     final jury + personal defences.
   - Coordinator pairs teams; mismatched stack pairings are fine.
   ```

2. В `PRESENTATION.md` добавить subsection «Peer review (week before)» с тремя строчками: дата, формат, что команда из этого получает. Зазор 5–7 дней между peer-review и финальной парой защит — по AC.

См. также обновлённую `decisions/peer-review-precedes-defence.md` (явная фиксация: peer-review перед финальной защитой, а не перед каждой sprint interview).

---

### C3. Сертификат и календарь не записаны явно [violates AC: lock-rules-at-kickoff]

**File:** `angular/README.md:54–56` и `angular/tasks/angular-team-task/README.md:31–39`

**Quote (`README.md:54`):**
> «After successfully completing the team task and passing the Angular interview, you will receive a **certificate of course completion**.»

**Quote (`angular-team-task/README.md:33`):**
> «| **Sprint 1** | 1 week      | Components                |»
> «| **Sprint 4** | 2 weeks     | HTTP, RxJS & Testing      |»

**Why this is a problem:** В дизайне Angular условие сертификата ясное: **пройти все три защиты + вести дневник (или сделать Plan-B-видео)**. Это бинарный чек-лист — не числовой порог, но не менее однозначный. AC `lock-rules-at-kickoff.md` обе формы признаёт, главное — студент может однозначно сверить свой статус.

**Но в материалах этого чек-листа нет**, есть только формулировка «successfully completing» — пустая фраза, которую студент не может проверить. Аналогично с календарём: «Sprint 1: 1 week» относительно неизвестной точки старта; конкретных дат нет. Это была одна из жалоб на Tandem Q3 — «непонятно, сколько надо набрать», «непонятно когда дедлайн».

**Recommended change:**
1. В `README.md:54` заменить формулировку на бинарный чек-лист:
   ```
   ## Certification

   To receive a certificate of completion you need ALL of the following:
   1. Passed all sprint interviews (1 per sprint, 4 total).
   2. Passed the team presentation (jury-defence).
   3. Passed the personal defence with a mentor.
   4. Maintained a development diary throughout the course
      (or recorded a Plan-B video — see DEVELOPMENT_DIARY.md).
   5. Passed the final Angular interview.
   ```
   Точная формулировка — на усмотрение Димы (количество защит и точное название «interview/defence» он знает лучше). Главное — чек-лист с однозначными "passed / not passed" пунктами.
2. В `angular-team-task/README.md` § 3 Timeline добавить колонку «Deadline date» рядом с «Duration». Даже приблизительная фиксация («Sprint 1 ends ~22.05.2026») в десять раз лучше отсутствия — это становится **read-only контрактом** в смысле AC.

См. также обновлённую `decisions/lock-rules-at-kickoff.md` (явная поддержка бинарного чек-листа как формы порога).

---

### C4. Benefits / awards: их нет [violates AC: lock-rules-at-kickoff]

**File:** ни в одном файле курса не упоминаются.

**Quote:** —

**Why this is a problem:** AC §2 требует "условия получения каждого бенефита/приза" на kickoff. У Angular сейчас декларируются только сертификат + "Gentleman's Agreement" (mentoring/README:88), последний — для менторов, не для студентов. Если на Angular нет призов / номинаций — это **тоже надо явно сказать**, иначе на ретро всплывёт «а где номинации?» или координатор по ходу решит добавить (стрим-инцидент Tandem Q3 случился именно из ничего).

**Recommended change:** В `README.md` после § Certification добавить одну строку:
```
## Benefits beyond the certificate

The Angular course currently does not run separate awards or nominations.
Single outcome: pass the threshold above → receive the certificate.
```
Если Дима хочет ввести peer awards / hackathon — фиксируется ровно сейчас, на kickoff, с условиями. По ходу курса добавлять нельзя.

---

### C5. Scope-сюрпризы спрятаны внутри проектов [violates AC: lock-rules-at-kickoff §4]

**File:** `angular/tasks/angular-team-task/PROJECT_CRYPTO.md:30`, `PROJECT_MUSIC.md:29`, `PROJECT_SHOP.md` (косвенно через commercetools)

**Quote (`PROJECT_CRYPTO.md:30`):**
> «**Required:** The backend must be written in **NestJS** (TypeScript). Using other frameworks (Express, Fastify without NestJS, etc.) is not allowed.»

**Why this is a problem:** Если команда выбирает CryptoTrade или MusicFlow — она обязана писать NestJS-бэкенд. Это **значимое расширение scope** (отдельный язык/фреймворк/учебная кривая), но в `CODE_STANDARDS.md`, `README.md` и `angular-team-task/README.md` об этом нет ни слова. Команда узнаёт о NestJS только когда открывает PROJECT_CRYPTO.md — на тот момент уже выбрала проект эмоционально. Точно соответствует scope-creep инциденту с Tandem Q3.

**Recommended change:** В `angular-team-task/README.md` § 1 Project Goals добавить одну строку под таблицей выбора:
```
> **Note:** CryptoTrade and MusicFlow require a small NestJS backend
> (auth + key/playlist storage). ShopFront uses commercetools as the
> backend (no custom NestJS needed). Pick accordingly.
```
И в `CODE_STANDARDS.md` после "## Angular" добавить short § "Backend (project-dependent)" с тем же текстом.

---

### C6. TEAM_SETUP.md обрывается на Step 2 [violates AC: lock-rules-at-kickoff §4]

**File:** `angular/tasks/angular-team-task/TEAM_SETUP.md:36`

**Quote:** Файл обрывается строкой `---` после Step 2. Нет Step 3 (mentor confirmation), нет deadline формирования команды, нет fallback (что если до конца Sprint 1 я не нашёл команду).

**Why this is a problem:** Несмотря на короткий объём, отсутствие deadline и fallback — это потенциальный поток "я не нашёл команду что делать" в Discord. На Tandem Q3 это было одним из источников волатильности.

**Recommended change:** Добавить два небольших блока:
```
## Step 3 — Mentor coverage check (coordinator)

Before Sprint 2 starts the coordinator confirms each team has a primary
mentor. See mentoring/README.md § Getting Mentees.

## Deadline & fallback

- Deadline to form a team: end of Week 2 (find-mentor week).
- If by then you have not found a team: contact the coordinator in
  Discord. The coordinator pairs you with another solo student or a
  mentor-led mini-team.
```

---

### C7. "Sprint interview" defence format не зафиксирован полностью [violates AC: lock-rules-at-kickoff §3]

**File:** `angular/tasks/angular-team-task/SCORE_PERSONAL.md:14`, `mentoring/README.md:78`

**Quote (`SCORE_PERSONAL.md:14`):**
> «**Important:** the call must take place **within 5 days** after the sprint deadline. If the team does not contact the mentor — 0 for that sprint.»

**Why this is a problem:** Жёсткое правило "0 за sprint, если не успели за 5 дней" + retakes запрещены ("One attempt per sprint", `SCORE_PERSONAL.md:78`) — это **большие потери баллов за организационный сбой**, не за технический навык. На Tandem Q3 студенты регулярно жаловались, что баллы теряются по техническим причинам (ментор не отвечает, у команды конфликт календарей). При том, что Sprint 4 = 150 баллов лично, это 17% от всего курса за один пропущенный звонок. Это **строгое правило**, которое должно быть прямо на главной странице, а не в FAQ.

**Recommended change:**
1. Перенести "5-day window + no retakes" в `angular-team-task/README.md` § 8 на верхний уровень.
2. Уточнить: что значит "team did not contact the mentor"? Если ментор не ответил 4 дня — кто виноват? Добавить в `SCORE_PERSONAL.md` блок: «If the mentor does not respond within 48h, contact the coordinator. Substitute mentor is assigned within 48h. The 5-day clock pauses while a substitute is being assigned.»

---

## Warnings (consider, lower urgency)

### W1. "Team Lead" роль введена, но не описана

**File:** `angular/tasks/angular-team-task/README.md:27`

**Quote:**
> «**Team Lead:** Chosen by the team. Creates the project repository on GitHub and grants access to the other members.»

**Recommended change:** Либо добавить 2-строчный блок «Team Lead обязанности» (репо, organising calls, не больше), либо переименовать в нейтральное "Repo Owner" — текущий термин предполагает асимметрию ответственности, которая не описана.

---

### W2. SCORE_PERSONAL "Penalties" со звёздами и шуткой

**File:** `angular/tasks/angular-team-task/SCORE_PERSONAL.md:87`

**Quote:**
> «- **(-1000000000000000000%)** Angular is not used.
> - **(-50%)** `strict: false` in `tsconfig.json`.»

**Why:** Шутка про -10^18% забавная, но она спрятана как реальная penalty в формальном scoring-документе. AC по lock-rules требует, чтобы правила были однозначными и применимыми — а тут не очевидно: -50% за `strict: false` это серьёзно? Студент будет переспрашивать.

**Recommended change:** Либо убрать шутку и оставить только серьёзную penalty `-50% strict: false`, либо явно вынести в FAQ как "obviously, if you don't use Angular at all there's no project to score".

---

### W3. Diary в team-task противоречит diary в course-level

**File:** `angular/DEVELOPMENT_DIARY.md:71` (course-level) vs `angular/tasks/angular-team-task/DEVELOPMENT_DIARY.md:29` (team-task)

**Quote (course-level):**
> «5. **Frequency:** at least **2 entries per week**.»

**Quote (team-task):**
> «5. **Frequency:** At least **1 entry per week**. Sprint 1 (1 week) — 1 entry, Sprints 2–4 (2 weeks each) — 1 entry.»

**Why:** Course-level требует 2/week, team-task — 1/week. Это разные правила, одни и те же студенты. Если нарочно — нужна заметка «во время team-task частота снижается до 1/week, потому что …». Если случайность — выровнять.

Дополнительно: course-level diary хранится в `/diary/`, team-task — в `/development-notes/{username}/`. Две разных папки. Если это намеренно (личный repo vs командный) — стоит явно сказать "в личном repo продолжай diary в /diary/, в командном — отдельная папка для team-task".

---

### W4. Score philosophy: implicit pre-AI model [flag only — AC is draft]

**File:** `angular/tasks/angular-team-task/README.md:97–117`, `SCORE_PERSONAL.md`, `SCORE_TEAM.md`

**Why:** 850-балльная система с разбивкой по 7 score'ам, мгновенно складывающимся в leaderboard через RS APP, — это classic pre-AI model. ADR `score-philosophy-after-ai.md` имеет статус **draft**, поэтому это не нарушение, а **флаг**: Angular по умолчанию запускается в pre-AI режиме, и это не зафиксировано как осознанное решение. См. AC: «Angular работает по старой модели по умолчанию — изменения отложены».

**Recommended change (light):** В kickoff-документе курса (`courses/angular/spec/kickoff.md` в repo Воробья) — одна строчка: «Angular 2026Q2 запускается в классической scoring-модели; решение о post-AI модели — open ADR, к Angular не применяется». Это **не Диме**, это нам в кикофф; для Димы — пас.

---

### W5. Weekly Activities of the Mentor: устаревший язык про "Public Score" и "Score form"

**File:** `angular/mentoring/pull-request-review-process.md` + `mentoring/README.md:166`

**Quote:**
> «After the review, the mentor enters the score into the Score form, based on which the Public Score is generated.»

**Why:** В team-task это уже RS APP Mentor Dashboard, не "Score form". Текст mentoring-секции — старый шаблон с других курсов RS School. Несоответствие может ввести менторов в заблуждение в первую неделю.

**Recommended change:** Прогнать `mentoring/` через actualization pass — убрать упоминания "Score form", выровнять с RS APP Mentor Dashboard. Это копи-эдит работа, не структурная.

---

### W6. "Team-task" file count: 13 файлов на одну задачу

**File:** `angular/tasks/angular-team-task/` — 13 markdown'ов

**Why:** Студент в первый день видит 13 файлов: README, PROJECTS, 3× PROJECT_*, 4× SPRINT*_CHECKPOINT, TEAM_SETUP, SCORE_TEAM, SCORE_PERSONAL, CODE_STANDARDS, PRESENTATION, DEVELOPMENT_DIARY. Это не блок-критическое — структура файлов нормальная — но порог на вход высокий. Tandem Q3 retro отдельно жаловалась на "слишком много правил, слишком разбросаны". Здесь не разбросаны, но плотно. Стоит подумать о двух-страничном "How to navigate this folder" в шапке `angular-team-task/README.md`.

**Recommended change (optional):** В шапку `angular-team-task/README.md` добавить блок:
```
## Read order

1. This file (overview).
2. PROJECTS.md → pick one of three projects.
3. PROJECT_<your_choice>.md → detailed scope.
4. SPRINT1..4_CHECKPOINT.md → start with Sprint 1 only.
5. SCORE_*, CODE_STANDARDS, PRESENTATION → reference, read as needed.
```

---

## What's already aligned (positive findings)

- **Sprint checkpoints — strong, granular scope** (`SPRINT1..4_CHECKPOINT.md`). Каждый: Why this sprint → Key concepts → Requirements → What to study → Self-check questions → FAQ. Это **точно по AC §4 (scope этапа)**: студент видит «что делает, что проверяется, что оценивается». Шаблон стоит унести в kickoff guideline для следующих курсов.
- **AI honesty rule** (`angular-team-task/README.md:43`, `DEVELOPMENT_DIARY.md:79`): «using AI is allowed… reflect this in your diary and clearly indicate which parts you wrote yourself» — точно та же логика, к которой Tandem пришла к концу Q3 в `course-tandem-q3-final-spec`.
- **Diary as gate, not just a score** (`SCORE_PERSONAL.md:62–68`, `DEVELOPMENT_DIARY.md:80–87`): «no diary → no admission to interview → 0 for sprint» — это сильная конструкция, не "diary gives N points".
- **Course-level diary** (`angular/DEVELOPMENT_DIARY.md`) — отлично написан, особенно секция "diary как зеркало, не отчёт" (lines 17–35) и пример (lines 110–149) с Roblox-проектом сына: показывает на личном примере вместо директивы. Это в духе `feedback_example_over_command.md` из памяти Воробья.
- **Plan B (video instead of diary)** в обоих diary-файлах — sensible escape hatch, требует именно re-implement, не просто разговор. Удерживает уровень по содержанию.
- **Sprint 4 code review** (`SPRINT4_CHECKPOINT.md:39–40`) с двумя опциями (PR comments или diary entry) — уважительно к ситуации, когда у команды мало PR'ов, но всё равно требует review-навык.
- **CODE_STANDARDS** (`angular-team-task/CODE_STANDARDS.md`) — чёткий, executable. `strict: true`, no `any`, ESLint, deploy. AC §4 закрывает.
- **SCORE_TEAM granularity** (`SCORE_TEAM.md`) — три блока по 80/80/100, каждый разбит на под-критерии с галочками. Это ровно тот уровень детализации, который снимает "subjective jury" критику с Tandem Q3.

---

## Notes for Воробей (not for Dima)

Эти пункты — про наши собственные decisions/, не про Димину работу.

1. **AC `mentor-staffing-policy.md` молчит о sprint interviews.** Наш AC про staffing говорит про "выделенный ментор → команда". Но на Angular есть отдельная механика — sprint interviews после каждого sprint. AC сейчас не отвечает на вопрос: «sprint interview обязательно проводит выделенный ментор, или допустимо staff-share?». Рекомендую дописать в `decisions/mentor-staffing-policy.md` § Параметры один абзац: «если в курсе есть sprint interviews / промежуточные defences, их проводит выделенный ментор команды; substitute допустим только при non-availability и активируется координатором, не по дефолту». Без этого C1 в нашем audit'е держится на интерпретации.

2. **AC `peer-review-precedes-defence.md` подразумевает живую защиту, но не формализует "что считается защитой".** На Angular у Димы есть `Sprint Interviews` — это, технически, тоже защита (mentor speaks with each student). AC не уточняет: peer-review должен предшествовать **финальной** защите (presentation), или **каждой** mentor interview? Если буквально «каждой», то peer-review должен быть 4 раза за курс, что явно избыточно (AC сама пишет "Один peer-review раунд достаточен"). Сейчас текст AC противоречит сам себе на этом пункте. Стоит добавить уточнение «финальная защита перед jury / cross-team / external», иначе peer-review-as-rehearsal слишком широк.

3. **AC `lock-rules-at-kickoff.md` § benefits — false positive риск.** В аудите я добавил C4 (нет benefits framework), но **возможно курс осознанно без призов**. AC сейчас формулирует требование как «полный список условий каждого бенефита». Если бенефитов нет, требование не применяется, и C4 — формально перегиб. AC стоит уточнить: «если курс не вводит benefits — это тоже фиксируется явно, "no benefits beyond the certificate"». Я выкрутился такой формулировкой в C4, но это интерпретация поверх AC.

4. **AC score-philosophy подсказывает один практический шаг для Angular**, не зависящий от исхода ADR: явно сказать в kickoff-документе курса (на нашей стороне, не у Димы), что Angular работает по pre-AI scoring модели, потому что ADR не закрыт. Это документация state, а не директива Диме. Сделать это по моему ощущению лучше **до** kickoff Angular, иначе если ADR закроется в позиции А через 2 месяца, придётся ретроспективно объяснять, почему Angular был исключением.

---

## Suppressions / debatable cases

### S1. "Random mentor" может быть осознанным выбором Angular

C1 — самая жёсткая претензия аудита. Возможна интерпретация: Дима **сознательно** делает sprint interviews ротационными, чтобы каждый студент пообщался с 4 разными менторами и получил разнообразную обратную связь. Это **не плохо** само по себе — это другой образовательный замысел. Но тогда стоит:
- Явно описать это как фичу в `SCORE_PERSONAL.md`: «sprint interviews ротируются между менторами, чтобы студент получил 4 независимых оценки».
- Подкрепить процессом: координатор гарантирует, что у каждой команды есть **primary** ментор для еженедельных встреч (это и есть AC по staffing), а **sprint interview** проводит другой ментор. Тогда AC не нарушено: primary mentor есть, ротация — отдельный механизм.
- В текущей формулировке файлов разница не видна, и читается как «выделенного ментора нет → берём кого попало», что и есть Tandem Q3 паттерн.

**Frame for Dima:** «consider documenting whether random-mentor sprint interviews is a deliberate rotation mechanism on top of a primary-mentor relationship, or a fallback when there's no primary».

### S2. Отсутствие peer-review может быть осознанным

Возможный аргумент: на Angular 12 недель — короче, чем у Tandem 16; команды по 2–3 студента — мельче. Peer-review между такими маленькими командами в формате «команда A смотрит на команду B» может быть менее ценен, чем в Tandem (3–6 студентов в команде). И финальная jury-презентация — открытая ("All sessions are **open** — any student or mentor can join as an observer", `PRESENTATION.md:9`). Можно интерпретировать это как **встроенный peer-review через присутствие**: студенты других команд приходят на jury-сессии, видят чужие подходы, задают вопросы.

**Frame for Dima:** «consider documenting whether the open-jury format is positioned as the peer-review equivalent, or whether a dedicated peer-review round (week 9) would add value as a rehearsal step».

### S3. NestJS в PROJECT_CRYPTO/MUSIC может быть осознанным расширением

C5 трактует «NestJS-бэкенд спрятан в PROJECT_*` как scope creep. Но: PROJECT_*` — это **проектная альтернатива**, и команда сознательно её выбирает. Можно возразить: «никакой "creep" нет, scope зафиксирован в момент выбора проекта». Согласен с возражением частично — но **выбор делается на Sprint 1**, а информация лежит на третьем уровне вложенности (README → PROJECTS → PROJECT_CRYPTO line 30). Достаточно one-liner на верхнем уровне (см. C5 recommended change), чтобы сделать это явным выбором, а не сюрпризом.

**Frame for Dima:** «consider surfacing the per-project backend requirement at the project-selection step rather than inside each PROJECT_* file».
