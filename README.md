# Tandi Garden — federated wiki of RS School

Эта репа — **L0 federation layer**: общая идентичность, ценности и фактический слой, на который опираются все курсы RS School и их AI-агенты. Содержимое читают студенты на GitHub, AI-агенты студентов и координаторов, и любой, кому нужны факты о школе.

## Где что лежит

| Папка / файл | Что внутри |
|---|---|
| `vision.md` | зачем существует Tandi Garden, federation-модель, дорожная карта |
| `constitution.md` | кто такая Tandi, голос, ценности |
| `schema.md` | правила wiki: 6 категорий, 4 типа страниц, ingest/query/lint workflows |
| `architecture.md` | техническое устройство federation |
| `curator.md` | гайд для координатора нового курса |
| `wiki/` | **источник правды о фактах школы** — people / courses / infrastructure / decisions / concepts / incidents |
| `retros/` | summary-карты ретроспектив (raw приватен, gitignored) |
| `audits/` | проверки конкретных курсов на соответствие decisions/ |
| `sources/` | summary-карты federation-level событий (митинги, документы) |
| `ingest/` | drop-zone для новых raw-материалов через PR |

## Как читать

1. **Студент / посторонний.** Открыть `wiki/index.md` — это каталог всех страниц. Нужная тема — переход по ссылке.
2. **AI-агент.** Прочитать `schema.md` (правила), `wiki/index.md` (каталог), идти по `related:` ссылкам в frontmatter каждой страницы.
3. **Координатор нового курса.** `curator.md` → `vision.md` → `wiki/decisions/lock-rules-at-kickoff.md` (что должно быть в kickoff-документе) → `wiki/decisions/` целиком.

## Как контрибьютить

- **Raw-материал** (ретро CSV, транскрипт открытого митинга, чат-экспорт): PR в `ingest/`. См. `ingest/README.md`.
- **Готовая wiki-страница**: PR прямо в нужную категорию `wiki/` или в sibling-view (`retros/`, `audits/`, `sources/`). Соблюдать формат frontmatter из `schema.md` §4.
- **Изменение `schema.md` или `constitution.md`**: PR с записью `amendment` в `wiki/log.md`.

Privacy-правила перед PR: raw с PII (имена студентов, прямые цитаты с авторством, GitHub-handles в фидбеке о людях) **не публикуется**. Структура `*/raw/` исключена из коммитов автоматически (`.gitignore`).

Mерджит PR координатор федерации (по умолчанию — Dzmitry Varabei). Это сознательный гейт, чтобы LLM-генерация не попадала в публичные факты без проверки.

## Lint

Раз в месяц — lint-pass: противоречия, устаревшие страницы, orphan'ы, битые ссылки. Первый — 2026-05-15. Подробности — `schema.md` §9.

## Связь с курсами федерации

Каждый курс RS School (Tandem, Angular, AI Systems, …) живёт в собственном репо. В этот wiki-репо едут только federation-уровня обобщения (commonly-cited facts about the school). Course-specific артефакты — в репо курса.

---

*Reference: Karpathy's «LLM Wiki» pattern — https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f*
