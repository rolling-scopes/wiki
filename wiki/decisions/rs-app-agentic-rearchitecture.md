---
type: concept
title: "RS App: agentic-first rearchitecture (open ADR)"
status: draft
last_updated: 2026-05-27
sources:
  - https://github.com/rolling-scopes/rsschool-app
  - https://github.com/rolling-scopes/rsschool-app/blob/master/DOMAIN.md
  - https://github.com/rolling-scopes/rsschool-app/blob/master/AGENTS.md
  - chat with Maxim Shilov (admin), Andrei (admin), Margaret G., May 2026
related:
  - decisions/score-philosophy-after-ai.md
  - decisions/mentor-as-defence-listener.md
  - decisions/short-track-via-framework-courses.md
---

# RS App: agentic-first rearchitecture (open ADR)

> **Статус: ОТКРЫТО.** Решение не принято. Документ — основа для обсуждения с контрибьюторами RS App. Цель — выровнять язык и сущности до того, как кто-то начнёт писать код.
>
> **Owners:** Воробей (постановка вопроса), Максим Шилов и Андрей (контрибьюторы RS App, инициаторы разговора).
> **Триггер:** разговор в community-чате в мае 2026. Margaret попросила интерфейс через Claude Code; Шилов сформулировал «агент = твой интерфейс»; Андрей предложил `SystemUser` как сущность для агентов вроде Dementor.
> **Целевая дата принятия рамки:** 2026-07-01. Конкретные эпики — после.

---

## TL;DR

1. **Это не рефакторинг RS App, это пересборка платформы.** RS App десятилетие был tool для проведения курсов. Новая модель: **комьюнити, где курсы — одна из активностей**, профиль человека шире участия в когорте, а агенты — полноправные акторы.
2. **Интерфейс — не страница, а агент.** UI остаётся (для тех, кто хочет), но первичный контракт — это **OpenAPI** + **MCP-сервер** + **Context Packs**. Каждый член комьюнити приходит со своим Claude Code/Codex/Gemini и говорит с платформой через свой агент.
3. **Сущности расширяются** в трёх направлениях: (а) **Person + Profile + Contribution + TrustSignal** как ядро, не Student/Mentor; (б) **SystemActor** как first-class, а не «фейковый GitHub-юзер»; (в) **MentoringLink** как явная гибкая связь, а не вшитый `student.mentorId`.
4. **Auth: PAT + scopes** для людей, **SystemActor** с собственным набором прав для агентов. Никаких сессионных хаков. Принцип «least privilege» на уровне actor, не роли.
5. **Монетизация** — отдельный раздел, не блокирует архитектуру. Четыре модели с trade-offs, выбор позже.

Документ самодостаточен. Связь с Tandi Garden — в разделе «См. также».

---

## 0. Контекст: почему этот разговор сейчас

Маргарет в community-чате спросила: «А почему её агент не научится пользоваться UI RS App?» Шилов ответил коротко: «интерфейсы не молодёжно». Воробей расшифровал: интерфейс задаёт строгие рамки — формы, кнопки, маршруты, — а **процесс обучения** в школе живой и разный. Команды работают со своим ментором, или каждые две недели идут на кросс-ментор-ревью, или защищаются перед жюри из соседнего курса. Делать UI под каждый вариант — невозможно. Делать API, которым умеет говорить агент, — наоборот, дешевеет с каждым месяцем.

Параллельно Андрей зафиксировал отдельный enabler: автоматический **Agent Dementor**, который дёргает неактивных студентов, **не должен** заводиться под фейковым GitHub-аккаунтом. Это **сущность другого класса** — у неё свой набор прав, свой аудит, своя зона ответственности. Шилов в этой ветке согласился: на уровне схемы это юзер с флажком, но **функционал — в отдельном модуле**, чтобы случайно не сминяться с обычными юзерами.

Эти два разговора — про одно и то же. **RS App десятилетие проектировался как админка для курсов** (Student + Mentor + Course + Task), а нужен — как **платформа комьюнити**, где люди учатся и учат, агенты работают рядом, а UI — один из клиентов, не центр гравитации.

Этот документ — попытка снять рамку с одного слова за раз.

---

## 1. Что есть сейчас (короткий аудит)

Этот раздел — фактический скелет, не претензия. Аудит проведён по [`rolling-scopes/rsschool-app`](https://github.com/rolling-scopes/rsschool-app) на 2026-05-27. Подробности — в `DOMAIN.md` репозитория; здесь — выжимка, нужная для разговора.

**Стек.** TypeScript-монорепо (Turbo). Три воркспейса: `client/` (Next.js, ~545 файлов, 34 модуля), `nestjs/` (новый API на `/api/v2`, 31 модуль, ~46 контроллеров, OpenAPI/Swagger), `server/` (легаси Koa, deprecated, но всё ещё держит TypeORM-entities и часть auth). PostgreSQL в RDS, миграции в `server/src/migrations/`.

**Сущности.** 56 TypeORM-entities в `server/src/models/`. Сгруппировать можно так:

- **Identity** — `User`, `Session`, `ProfilePermissions`, `ExternalAccount`, `LoginState`, `Contributor`, `Resume`.
- **Course shape** — `Course`, `Discipline`, `DiscordServer`, `CourseEvent`, `Event`, `Alert`.
- **Per-course роли** — `Student`, `Mentor`, `CourseUser`, `CourseManager`, `Registry`, `MentorRegistry`, `UserGroup`, `Certificate`.
- **Task domain** — `Task`, `CourseTask`, `TaskCriteria`, `TaskResult`, `TaskChecker`, `TaskArtefact`, `TaskVerification`, `TaskSolution`, `TaskSolutionChecker`, `TaskSolutionResult`, `TaskInterviewStudent`, `TaskInterviewResult`.
- **Защиты** — `StageInterview`, `StageInterviewStudent`, `StageInterviewFeedback`.
- **Команды** — `TeamDistribution`, `TeamDistributionStudent`, `Team`.
- **Обратная связь** — `Feedback`, `PrivateFeedback`, `StudentFeedback`, `CourseLeaveSurveyResponse`.
- **Уведомления и аудит** — `Notification*`, `Alert`, `History`, `RepositoryEvent`.
- **AI-инфра** — `Prompt` (uses `RSSHCOOL_OPENAI_API_KEY` в env, не в сущности).

**Auth.** GitHub OAuth (Passport) → JWT (2 дня) → `auth-token` cookie. Принимает также `Authorization: Bearer <jwt>`. Roles enum в `Session.ts`: `admin`, `hirer`, `manager`, `supervisor`, `student`, `mentor`, **`dementor`**, `activist`, `taskOwner`. Гарды: `DefaultGuard`, `RoleGuard + @RequiredRoles`, `CourseGuard`.

**Что важно — чего НЕТ:**
- Нет таблицы API-токенов / PAT.
- Нет понятия SystemUser / Agent / Bot.
- Нет explicit `MentoringLink` — связь mentor↔student зашита в `Student.mentorId` (один ментор на студента, навсегда внутри курса).
- Нет event log как контракта (есть `History` для аудита, но это не event sourcing для агентов).
- Нет MCP-сервера.

**Что важно — что УЖЕ ХОРОШО:**
- `DOMAIN.md` существует и поддерживается — половина работы по выравниванию языка сделана.
- `AGENTS.md` устанавливает протокол для LLM-агентов: что читать, как тестировать, не запускать deploy. Контрибьюторы уже работают рядом с агентами.
- OpenAPI генерируется автоматически из `@ApiProperty`, есть Swagger UI. Это огромный задел.
- `GUIDELINES.md` фиксирует чистую модульность NestJS: контроллеры тонкие, DTO отдельно, гарды декларативно.
- Role `dementor` уже есть в коде. То, что Андрей описал в чате, — не новое понятие, а formalisation того, что уже бытует.

Итог: фундамент пригоден. Перестраивать с нуля не нужно. Нужно явно выделить три новые оси (Identity, Agents, Relationships) и постепенно вести существующие сущности к ним.

---

## 2. Философия: комьюнити-first, курс — активность

Сначала позиционирование, потом сущности. Если зайти с сущностей, мы получим хороший CRM. Если зайти с философии — получим основу для социальной сети для learning-and-teaching.

### 2.1. Что мы продаём (или раздаём)

**Сейчас:** курс. Студент приходит, проходит, получает сертификат. RS App обслуживает этот цикл.

**Целевая модель:** **участие в комьюнити**. Курс — одна из активностей внутри. Другие активности: ведение собственного курса, менторство в чужих курсах, peer-review, защиты, экспертные сессии, публикации. Профиль человека описывает **всё это вместе**, не только последнюю когорту.

Это сдвиг масштаба. До: «я был студентом RS Tandem 2026 Q3». После: «у меня вклад в RS School за 4 года: 2 завершённых курса, 12 проведённых менторских защит, 3 публикации, 2 спонсорства тестовых заданий».

### 2.2. Профиль как сумма вклада

Профиль становится агрегатом `Contribution[]` — событий, в которых человек участвовал с подтверждаемой ролью. Профиль не вычисляет «уровень» (ladder, level, ранг). Профиль показывает **что и где сделано**, плюс **что про это сказали другие** (`Endorsement`).

Это согласуется с открытым ADR [`score-philosophy-after-ai.md`](score-philosophy-after-ai.md): после LLM лидерборд становится производной от подписки на агента. Вклад в комьюнити — нет. Замена лидерборду — не «новый ranking», а **видимый портрет участия**.

### 2.3. Агенты как полноправные акторы

Сейчас агенты — внешние ассистенты. Они обращаются к RS App снаружи как анонимы или через украденный пользовательский JWT. Это значит:
- Школа не знает, кто делает запрос (человек или его агент).
- Агент не может действовать «от своего имени» — только от имени владельца.
- Любой автоматизированный сервис (Dementor, Tandi-ingest, ночной сборщик баллов) сегодня — это либо cron на сервере школы, либо хак с фейковым GitHub-аккаунтом.

**В новой модели агенты — первого класса:**
- **Personal Agent** живёт у каждого члена комьюнити (Margaret подключила Claude Code → у неё есть PAT, scope `read:my-profile + write:my-diary`). Школа знает, что запрос идёт от агента Маргарет, и логирует это.
- **System Actor** живёт на стороне школы (Agent Dementor, Tandi-Skaut, Tandi-Pero). У него отдельная identity, отдельные scope, владелец-Person, который отвечает.
- **Verified Course Agent** — агент, которого зарегистрировал координатор курса для своей когорты (например, агент, который собирает peer-review для Angular Q2 2026).

Все три типа — экземпляры одной абстракции с разной зоной доверия.

### 2.4. Контракт, не интерфейс

UI больше не предполагается единым источником взаимодействия. Контракт — **OpenAPI** + **MCP**. Веб-UI становится одним из клиентов наряду с Claude Code, Codex, Gemini, кастомными скриптами координаторов курсов.

Из этого вытекает дисциплина:
- **Никакой логики в client/.** Если фронт сейчас вычисляет нечто, чего нет в API, — это баг. Агент должен мочь сделать всё то же, что делает человек через UI, через тот же API.
- **OpenAPI — лицо платформы.** Меняешь API без правки описания — ломаешь агентов всех контрибьюторов. Это нарушает CONTRIBUTING жёстче, чем сломать тест.
- **MCP — не альтернатива OpenAPI, а слой над ней.** MCP-сервер генерируется/синхронизируется из OpenAPI + дополняется «доменными» tools (см. §5.2).

---

## 3. Бизнес-сущности (ER-map для целевой модели)

Здесь — **карта**, не итоговый dbml. Названия сущностей и связи фиксируются для разговора; конкретные поля прорабатываются на этапе implementation после согласия на рамку.

### 3.1. Identity & profile (ядро)

| Сущность | Что описывает | Как соотносится с текущим RS App |
|---|---|---|
| **Person** | Универсальная identity. Один человек = одна Person, через все курсы и роли. | Существующий `User` + слияние с `Contributor` |
| **ExternalIdentity** | GitHub-логин, Discord, Telegram, email, в будущем другие OAuth-провайдеры. Person 1—N. | Сейчас GitHub-логин — primary key всего; нужно ослабить |
| **Profile** | Публичный фасад Person: имя, краткое био, видимость секций. | `User` (часть полей) + `ProfilePermissions` |
| **Contribution** | Атомарное событие вклада: «выполнил task X», «провёл защиту Y», «написал peer-review Z», «опубликовал статью». Immutable. | Сейчас разбросано: `TaskResult`, `StageInterview`, `StudentFeedback`, `Feedback`. Все они генерируют Contribution-записи. |
| **Endorsement** | Подтверждение/благодарность от Person к Person в контексте. Семантически как `Feedback`, но не «оценка» — простой факт «X отметил вклад Y». | Существующий `Feedback` (gratitude) — кандидат на переосмысление |
| **TrustSignal** | **Вычислимый**, не сохраняемый: производная от Contribution + Endorsement + длительности участия. Не «карма», не score. | Новая абстракция, не существует |
| **Resume** | Структурированное CV + видимость работодателю. Уже есть, оставляем. | Существующий `Resume` |

**Принцип:** Person — стабильна, ExternalIdentity — расходный материал. Если человек завтра удаляет GitHub-аккаунт, его Person не теряется (теряются только linked contributions).

### 3.2. Активность: курсы, когорты, задания

| Сущность | Что описывает | Как соотносится с текущим RS App |
|---|---|---|
| **Course** | Шаблон курса: программа, дисциплина, сроки в среднем. | Существующий `Course` (упрощается) |
| **Cohort** | Конкретный запуск курса с датами, координатором, набором участников. | **Нет explicit сущности.** Сейчас `Course` смешивает шаблон и instance. Нужно разделить. |
| **CourseMembership** | Person × Cohort × Role. Role — `student`, `mentor`, `coordinator`, `jury`, `external`. | Заменяет `Student`, `Mentor`, `CourseUser`. Per-cohort, не per-course. |
| **Team** | Команда внутри Cohort. | Существующий `Team` |
| **TeamMembership** | Person × Team × period. Person может перейти из одной команды в другую. | `TeamDistributionStudent` + расширение |
| **Task** | Шаблон задания. | Существующий `Task` |
| **CohortTask** | Task в контексте Cohort: дедлайны, тип проверки, баллы. | Существующий `CourseTask` (переименование под cohort) |
| **TaskSubmission** | Что студент сдал. | Объединяет `TaskSolution`, `TaskResult`, `TaskArtefact` под одной зонтичной сущностью с typed payload |
| **TaskReview** | Кто проверял. Reviewer — Person (студент при peer-review, ментор при ментор-ревью, агент при auto-check). | Объединяет `TaskChecker`, `TaskSolutionChecker`, `TaskSolutionResult`, `TaskVerification` |
| **DefenceSession** | Любой формат защиты: peer, mentor, cross-mentor, jury, finale. Person — participant с typed role. | Обобщает `StageInterview`, `TaskInterviewStudent`, `StageInterviewFeedback` |

**Принцип консолидации:** в текущей модели **12 сущностей** описывают разные стадии одного потока (сдал → проверили → дали фидбек → провели защиту). Это исторически выросло, и каждый раз новая фича добавляла сущность вместо обобщения. Новая модель — четыре сущности (`TaskSubmission`, `TaskReview`, `DefenceSession`, и переносной `Contribution`).

### 3.3. Связи между людьми (новое)

| Сущность | Что описывает | Зачем |
|---|---|---|
| **MentoringLink** | Person × Person × context × policy. Context: `Cohort` или `Team` или `Course-wide`. Policy: `dedicated` (классика — один ментор на студента), `team-bound` (ментор связан с командой), `rotating` (раз в две недели меняется), `cross-mentor` (защищаются у соседа). | **Главный сдвиг.** Сейчас `Student.mentorId` — единственная форма связи. Это вшивает «один ментор на студента» как догму. Воробей в чате прямо назвал это блокером: «команды ходят на ревью кросс-менторов» — этого в схеме нет. |
| **CommunityFollow** | Person → Person или Person → Cohort. Слабая связь типа «я слежу за вашим курсом». | Основа для агентского feed: «что нового у тех, за кем я слежу» |
| **AccessGrant** | Person или SystemActor → Resource → scope → expiry. | Заменяет смесь Roles + per-resource permissions; см. §6 |

`MentoringLink` сразу даёт три бонуса: (а) можно описывать гибкие схемы наставничества без хаков; (б) можно менять менторов внутри курса (например, ментор пропал — `MentoringLink.status = 'transferred'`); (в) Agent Dementor выбирает по `MentoringLink`, к кому стучаться, а не по `Student.mentorId`.

### 3.4. Агенты как first-class

| Сущность | Что описывает |
|---|---|
| **SystemActor** | Не-человек, действующий в системе. Имеет `name`, `owner: Person`, `description`, `scopes[]`, `audit_level`. **Не User с флажком, а отдельная сущность с такими же rights/roles механизмами.** |
| **PersonalAgent** | Подвид: SystemActor, владелец — Person, действует от его имени с подмножеством его прав. |
| **CourseAgent** | Подвид: SystemActor, владелец — координатор Cohort. Scope ограничен этой когортой. |
| **CoreAgent** | Подвид: SystemActor, владелец — core team RS School. Глобальный scope. Сюда попадают Agent Dementor, Tandi-Skaut, Tandi-Pero. |
| **AccessToken** | PAT для Person ИЛИ для SystemActor. Hash + prefix + scope + expiry + last_used. |
| **AgentRun** | Запись о действии SystemActor: timestamp, action, target, result, latency. Immutable, основа для аудита и rate-limit. |

Андрей в чате попросил **изоляцию модуля**: «весь related код в одно место». В терминах NestJS это значит — `nestjs/src/system-actors/` отдельный модуль, который шарит `AccessGrant` с `users/`, но не смешивается в коде.

**Что Agent Dementor может и чего не может — на уровне scopes:**
- ✅ `read:cohort.students.activity`
- ✅ `write:cohort.student.status` (`active → inactive`)
- ✅ `write:notification.send`
- ❌ `write:cohort.student.score` (баллы не его дело)
- ❌ `delete:cohort` (Шилов в чате: «дементор не должен мочь удалить курс случайно»)
- ❌ `read:person.private_feedback` (не его контекст)

Scopes — не enum, а декларация. См. §6.

### 3.5. Социальный слой

| Сущность | Что описывает |
|---|---|
| **ContentPiece** | Унифицированный тип для diary, war-story, шпаргалки, видео, заметки координатора. Поля: author Person, visibility, attached_to (Contribution, Cohort, Team), tags. |
| **Activity** | Append-only event log. Создаётся на любом значимом действии (Contribution, Endorsement, AccessGrant, AgentRun). Источник для feed-агентов. |
| **Subscription** | Person → план (free/premium). См. §7. |
| **Payment** | Одноразовый платёж или recurring. См. §7. |

Activity — отдельно от Contribution. Contribution — «что человек сделал ценного». Activity — «что произошло в системе». Activity включает Contribution-события и многое другое (логины, отказы от ревью, изменения профиля). Не всё Activity — это Contribution.

### 3.6. Что выпадает из этой карты

Сознательно не вношу:
- **Reputation как число.** Если возникает — это `TrustSignal` (вычислимо, не хранится).
- **Karma / level / ranking.** Не нужны до тех пор, пока community не попросит.
- **Permissions как enum.** Только scope-based (см. §6).
- **Notification entities.** Существующая структура `Notification*` адекватна, не требует пересборки. Подключается через `Activity` listener.

---

## 4. ER-карта целевой модели (главные рёбра)

```
                              ┌─────────────┐
                              │   Person    │
                              └─────┬───────┘
                                    │ 1—N
                ┌───────────────────┼────────────────────┐
                │                   │                    │
        ┌───────▼─────────┐  ┌──────▼──────┐    ┌────────▼─────────┐
        │ ExternalIdentity│  │   Profile   │    │  CourseMembership│
        │ (github, ...)   │  │             │    │ (role: student|  │
        └─────────────────┘  └─────────────┘    │  mentor|coord|..)│
                                                └────────┬─────────┘
                                                         │ N—1
                                                  ┌──────▼──────┐
                                                  │   Cohort    │
                                                  └──────┬──────┘
                                                         │ N—1
                                                  ┌──────▼──────┐
                                                  │   Course    │
                                                  └─────────────┘

  Person 1—N MentoringLink N—1 Person
         (context: Cohort | Team | Course-wide)
         (policy: dedicated | team-bound | rotating | cross-mentor)

  Person 1—N TeamMembership N—1 Team N—1 Cohort
  Person 1—N Contribution N—1 (Cohort | Team | ContentPiece)
  Person 1—N Endorsement →   Person  (in context)
  Person ⇨   TrustSignal (computed)

  Person 1—N AccessToken
  Person 1—N SystemActor (own personal agents)
  SystemActor 1—N AccessToken
  SystemActor 1—N AgentRun

  Cohort 1—N CohortTask N—1 Task
  Cohort 1—N DefenceSession
  CohortTask 1—N TaskSubmission N—1 Person
  TaskSubmission 1—N TaskReview N—1 (Person | SystemActor)

  Person OR SystemActor 1—N AccessGrant N—1 Resource
```

---

## 5. API surface

Три уровня контракта, каждый — для своего класса клиентов.

### 5.1. Уровень 1 — OpenAPI как контракт первого порядка

Уже существует. Доводим до состояния «можно посадить агента и работать»:

- **Полные `@ApiProperty` на каждом DTO.** Сейчас уже частично соблюдается через `GUIDELINES.md`.
- **Идемпотентность на write.** Header `Idempotency-Key` обязателен для `POST` и `PATCH`; сервер хранит дедуп-кэш на 24 часа.
- **ETag + If-Match** для оптимистичной блокировки при конкурентных правках.
- **Структурированные ошибки.** Тело ошибки — `{ code: SCREAMING_SNAKE, message: human, fields?: [...], retry_after?: int }`. Агент читает `code`, человек — `message`.
- **Pagination — `cursor`, не `page`.** Курсорная пагинация устойчива при изменениях, страничная — нет.
- **Discoverability.** Каждый ответ ресурса включает блок `_links` с возможными next actions для агента-новичка: «после `TaskSubmission` агент может вызвать `GET /reviews/available?for=submission/{id}`».
- **API versioning через path** (`/api/v2/...` → `/api/v3/...`), не через header. Понятнее для агентов.

### 5.2. Уровень 2 — MCP-сервер как переносной интерфейс агента

`rs-school-mcp` — wrapper над OpenAPI плюс доменные tools, которых нет в REST.

Принципы:
- **Tools соответствуют доменным операциям, не CRUD.** `assign_mentor_to_team(team_id, mentor_person_id, policy)`, а не `POST /mentoring-links` + три follow-up запроса.
- **Tools принимают и возвращают «понятные» формы.** `get_my_students(as_of?: date)` возвращает массив с уже подтянутыми Contribution-summaries, а не список ID.
- **`list_my_capabilities()`** — мета-tool. Возвращает контракт «что я могу сделать прямо сейчас», основываясь на scope текущего токена. Агент-новичок вызывает первым.
- **Tools документируются на естественном языке** в descriptions; это литература для LLM.
- **Один MCP-server на инстанс.** Не плодим `tandi-data` + `tandi-voice` + `rs-app-mcp` без необходимости — у LLM ограниченное число подключённых MCP. Делим по нагрузке, не по «эстетике слоёв».

### 5.3. Уровень 3 — Context Packs (сценарные эндпоинты)

«Дай мне всё, что нужно агенту X для роли Y прямо сейчас». Эти эндпоинты — оптимизация под agent workflow, а не нарушение REST.

Примеры:
- `GET /context/me-as-student?cohort=...` — мой статус в курсе, ближайшие дедлайны, последние review, мой ментор, его последние комментарии, что от меня ждут на этой неделе.
- `GET /context/me-as-mentor?cohort=...` — мои студенты с их статусами, ближайшие защиты, кто из них «застрял», suggestions.
- `GET /context/me-as-coordinator?cohort=...` — health-check когорты, кто из менторов пропал, какие команды без активности, top-3 проблемы.
- `GET /context/person/{id}/public-profile` — то, что агент работодателя видит про кандидата.

Каждый Context Pack возвращает `freshness` (когда последний раз пересчитан) и `_links` на действия. Кэшируется агрессивно (5–60 секунд).

Context Packs **не заменяют REST**. Они — concierge для частых сценариев. Если агент хочет необычной композиции — идёт через MCP/REST.

### 5.4. Чего НЕ делаем

- **GraphQL.** Соблазн есть, но: усложняет auth-scope сильно (на уровне поля), MCP уже даёт композицию, плюс OpenAPI у нас уже есть и работает. Если потом появится конкретный use case — обсудим.
- **WebSockets / SSE как обязательный канал.** Push-уведомления — отдельная история. Сначала pull через OpenAPI/MCP.
- **gRPC.** Внутренний API между сервисами — возможно. Внешний контракт — REST.

---

## 6. Auth, scopes, system actors

### 6.1. Личные пользователи

GitHub OAuth остаётся (DNA RS School). Сессия для UI — JWT в HttpOnly cookie, как сейчас.

**Добавляется:**

- **Personal Access Tokens.** UI: «Connect Claude Code», «Connect Codex», «Generate API token». Token — opaque строка, сервер хранит хеш. На каждом токене — набор `scopes[]` и `expiry` (по умолчанию 90 дней, можно установить меньше; на больше — нельзя).
- **Scope-формат.** `<verb>:<resource>[:<qualifier>]`. Примеры: `read:profile.me`, `read:cohort.public`, `write:diary.me`, `read:cohort.X.students`, `write:cohort.X.review.peer`.
- **Token issuing flow:** OAuth-style для машин (PKCE), либо UI-кнопка для интерактива. Никаких long-lived tokens по умолчанию.
- **Audit:** каждый запрос с PAT логируется в `Activity` (без тела, только метаданные).

### 6.2. SystemActor

Андрей в чате сказал главное: **«это не то что системный, у него тоже должны быть права и роли»**. То есть SystemActor — не привилегированный супер-юзер. Это **нормальный actor**, у которого scopes такие же, как у Person, плюс owner и audit.

**Различия SystemActor vs Person:**

| | Person | SystemActor |
|---|---|---|
| Auth | OAuth + PAT | только Token |
| Default scopes | по роли в Membership | пусто, явная декларация |
| Owner | себе | другой Person |
| Audit | по `Activity` | по `AgentRun` (более детально) |
| UI presence | есть | опционально |
| Can own SystemActors? | да | нет |

**Регистрация SystemActor:**
1. Person с правом `create:system-actor` (изначально — только core team) открывает Form/API: name, owner, description, requested scopes.
2. Если scopes выходят за стандартный набор — нужно явное одобрение второго core-member.
3. Получает первый AccessToken; токен ротируется владельцем.
4. SystemActor попадает в `index.md` Wiki (см. Tandi Garden `schema.md`) как entity-страница: что делает, owner, audit-summary.

**Принцип least privilege:** Agent Dementor не должен иметь `delete:cohort` даже если кажется, что «всё равно админ под ним стоит». Если придёт scenario, где Dementor должен иметь больше — это новый actor с расширенным scope, не патч существующего.

### 6.3. AccessGrant как замена сложных permission-таблиц

Сейчас в RS App permissions — это enum в `CourseUser.roles` + ad-hoc checks. Сложно расширять. Новая модель:

```
AccessGrant {
  subject: Person | SystemActor
  resource: Cohort | Team | Course | Person | "*"
  scope: string  // e.g. "read:students" or "write:score"
  granted_by: Person
  granted_at: timestamp
  expires_at: timestamp | null
  reason: text | null
}
```

Все checks идут через `hasGrant(subject, scope, resource)`. Это даёт:
- Аудит «кто кому дал что и когда» в одной таблице.
- Истечение прав по умолчанию.
- Простое снятие: `expires_at = now()` — и пропало.

Существующие per-course roles мигрируются в AccessGrant за один pass.

---

## 7. Монетизация — четыре модели

Это **не решение**, а карта вариантов с trade-offs. Решение принимается отдельно, после выравнивания по архитектуре.

### Модель A: платные курсы, RS School берёт fee

**Как работает:** координатор курса ставит цену, школа берёт процент. Студенты платят за участие.

**Плюсы:** простая бизнес-модель, понятна стейкхолдерам, поддерживает координаторов курсов.
**Минусы:** **ломает DNA**. RS School 10+ лет — бесплатное волонтёрское комьюнити. Платные курсы переключают мотивацию учить с «отдавать» на «зарабатывать», что меняет тон сообщества.
**Что меняет в схеме:** `Cohort.tier`, `Payment`, `Payout`, `Subscription.scope=cohort`.

### Модель B: marketplace курсов

**Как работает:** комьюнити-площадка, где любой Person с достаточным TrustSignal может запустить свой курс с бесплатной или платной моделью. Школа предоставляет инфру (RS App, Tandi-федерация, реестр студентов) и берёт thin fee за платные потоки. Бесплатные потоки — бесплатно.

**Плюсы:** масштабируется без core team. Решает [`short-track-via-framework-courses.md`](short-track-via-framework-courses.md) органично. Поддерживает выпускников, готовых вести.
**Минусы:** требует quality control (как Apple App Store) — иначе marketplace тонет в plagiat и низком качестве. Это **дорогой институт сам по себе.**
**Что меняет в схеме:** `Cohort.creator: Person` (не координатор RS School, а любой member), `Course.template` отделяется от `Cohort.instance`, `ReviewPolicy` для запуска нового курса.

### Модель C: премиум-агенты

**Как работает:** все школьные агенты бесплатны на базовом уровне. «Tandi Pro», «Tandi for Teams», «Mentor Copilot Pro» — за плату. Подписку платит координатор курса или сама команда.

**Плюсы:** прозрачно — деньги идут на оплату дорогого LLM-токенажа, не на «членский взнос». Естественный билинговый каркас: чем больше используешь, тем больше платишь. Не ломает community-vibe.
**Минусы:** маржа низкая (большая часть денег уходит провайдеру LLM). Зависимость от стоимости моделей. Опасность «бесплатно станет хуже, чтобы продать платное».
**Что меняет в схеме:** `Subscription.scope=agent`, `AgentRun.cost_estimate`, `SystemActor.tier`.

### Модель D: corporate sponsorship + recruitment

**Как работает:** компании платят за доступ к рекрутинговому слою (Resume + TrustSignal + Endorsement), за спонсирование курсов, за брендирование заданий («это задание от Wargaming»). Студенты — бесплатно.

**Плюсы:** ближе всего к текущей модели RS School. Деньги извне, комьюнити не «платит». Согласуется с тем, что Resume и hirer-роль уже есть в коде.
**Минусы:** требует sales-функцию. Меняет голос школы при близком партнёрстве. Зависимость от состояния рынка найма.
**Что меняет в схеме:** `Sponsor` (организация), `SponsoredItem` (course/task/cohort), доступ для `hirer` к `Resume` через AccessGrant.

### Какая комбинация наиболее вероятна

Сейчас Воробей сформулировал это как открытый вопрос. Гипотеза для обсуждения: **D + C** — основная модель «как сейчас, но шире» (D), плюс премиум-агенты для тех, кто получает от агентов измеримую пользу (C). A и B — позже, если D+C не покрывают расходы.

Этот вопрос требует отдельного ADR с финансовыми оценками, не только архитектурными.

---

## 8. Эскиз миграции (без сроков)

Greenfield не нужен. Мы постепенно добавляем новые сущности и постепенно демонтируем переплетения. Принципы:

1. **Не ломать существующее.** Все текущие endpoints остаются работающими; новые поверх.
2. **Парные движения.** Каждая новая сущность приходит с adapter, который пишет/читает в существующую таблицу до перехода.
3. **Сначала API, потом UI.** UI переезжает на новые контракты только когда контракты обкатаны хотя бы одним внешним агентом.
4. **Feature flags не «временные», а долгосрочные.** Чтобы Angular-курс мог пройти полностью на «старой» модели, а AI Systems — экспериментировать с новой, без конфликтов.

Возможные фазы (последовательность, не сроки):

- **Phase 1 — Foundation.** AccessToken, AccessGrant, SystemActor. Без ломки UI. Audit log переезжает в Activity. Первый PAT-flow в UI для самих контрибьюторов.
- **Phase 2 — Agents first-class.** Регистрация SystemActor через UI. Перенос cron-задач (вкл. Dementor) под SystemActor. MCP-server в proof-of-concept.
- **Phase 3 — Identity & Profile reshape.** Cohort выделяется из Course. CourseMembership заменяет Student/Mentor/CourseUser (с обратной совместимостью). MentoringLink выделяется из Student.mentorId.
- **Phase 4 — Tasks consolidation.** TaskSubmission/TaskReview/DefenceSession поглощают существующие 12 task-сущностей.
- **Phase 5 — Context Packs.** Первые agent-flow эндпоинты.
- **Phase 6 — Profile depth.** Contribution + Endorsement + TrustSignal. Profile становится первичным экраном.
- **Phase 7+ — Marketplace / monetization.** Только когда решена модель.

Каждая фаза — самостоятельный milestone с собственным RFC.

---

## 9. Open questions

Эти вопросы — не блокеры рамки, но требуют ответа до соответствующих фаз.

1. **Идентичность без GitHub.** Можно ли быть Person без GitHub External Identity? (Будущие курсы для UX-дизайнеров, продуктологов могут не требовать GitHub.)
2. **Public-by-default vs private-by-default.** Профиль и Contribution-feed — что публично, что нет? Это сильно влияет на UX.
3. **Кто решает scope нового SystemActor?** Один core-member или коллегиально? Если один — кто и по какому критерию?
4. **Cross-course progression.** Если Person прошла RS Tandem, идёт в AI Systems — как её Contribution-история переносится? Нужна ли «градация» (Junior/Mid/Senior комьюнити-член)?
5. **Federation совместимость.** Tandi Garden движется к федерации (per-course repos, отдельные узлы). Целевой RS App — единый или тоже федеративный? Что хранится централизованно, а что — на стороне координатора?
6. **Lifecycle курсов.** Когда Cohort «закрыта» — Contribution остаются, но что с MentoringLink? AccessGrant?
7. **Платное за что конкретно.** Раздел 7 даёт модели; нужен следующий ADR с цифрами.
8. **Совместимость с правовыми ограничениями.** GDPR-удаление Person → что делать с Contribution, в которых она участвовала?
9. **Анти-злоупотребление.** Если каждый member может зарегистрировать SystemActor, что мешает заспамить систему 1000 агентами? Rate-limit на регистрацию + минимальный TrustSignal-порог.
10. **MCP в проде vs локально.** MCP-server один для всех или каждый координатор разворачивает свой? Аутентификация через PAT решает; но эксплуатация — отдельный вопрос.

---

## 10. Что НЕ в scope этого документа

- Конкретный код: классы, DTO, миграции.
- Решение по монетизации (только trade-offs).
- Миграция с TypeORM на другое — это отдельный слой, никак не пересекается с этой рамкой.
- Frontend redesign. UI меняется только после стабилизации контрактов.
- Конкретный выбор LLM-провайдеров для core-агентов. Это решается per-actor.

---

## 11. Что мы хотим от этого документа

Не одобрения «делаем как написано». Нужна **общая рамка**: согласие на:

- ✅ или ❌: «комьюнити-first, курс — активность» (раздел 2).
- ✅ или ❌: новое ядро Identity (Person, ExternalIdentity, Profile, Contribution, Endorsement, TrustSignal).
- ✅ или ❌: SystemActor — first-class сущность, не User с флажком.
- ✅ или ❌: OpenAPI + MCP + Context Packs как три слоя контракта.
- ✅ или ❌: AccessGrant + scope-based вместо текущего roles enum.

Если по всем пяти ✅ — следующая встреча планирует Phase 1. Если по какому-то ❌ — раздел переписывается.

---

## См. также

- **`vision.md`** (Tandi Garden) — почему федерация курсов и почему агенты-активисты со своей подпиской.
- **`architecture.md`** (Tandi Garden) — кольца доступа Ring 0–3, MCP-роадмап для Tandi-данных (раздел 5.3). RS App — отдельный узел в этой картинке.
- **`constitution.md`** (Tandi Garden) — голос Tandi (один из CoreAgent-ов в новой схеме). Раздел 8 о разделении ролей (Tandi vs Статист vs Жюри) — прямой источник для §3.4.
- **`decisions/score-philosophy-after-ai.md`** — open ADR про роль баллов; затрагивает TrustSignal.
- **`decisions/mentor-as-defence-listener.md`** — пример того, что роль ментора уже сейчас определяется policy, не только Membership. Мотивирует `MentoringLink`.
- **`rsschool-app/DOMAIN.md`** (внешний) — текущая модель сущностей.
- **`rsschool-app/AGENTS.md`** (внешний) — протокол для LLM-агентов на стороне контрибьюторов RS App.

---

*Документ живой. Правки — через PR. Существенные изменения раздела «бизнес-сущности» или «scopes» — тянут запись в `log.md` с action `amendment`. Финальное закрытие ADR — отдельная запись `update`.*
