<!-- SENS-DOMAIN-LADDER-2026-10-08:BEGIN -->
## Чинна доменна доктрина SENS — для всіх агентів (2026-10-08)

**Пріоритет:** цей розділ замінює будь-які застарілі твердження нижче про Sens8/Sid8/Function8 як універсальну основу мови. Він не скасовує локальні правила безпеки, тестування, CI, координації та специфічні контракти репозиторію. Для змін, не пов'язаних із SENS, не нав'язуйте семантику SENS стороннім системам.

- **Першоджерело:** [SENS `language-contract.lisp`](https://github.com/juv4uk/sens/blob/main/language-contract.lisp) (чинний Contract 11.8), [карта повноважень](https://github.com/juv4uk/sens/blob/main/docs/semantic-authority-map.md), ратифіковані `contracts/dN-ratification.lisp` та `knowledge/dN-ratified.json`. Довідковий `AGENTS.md` не змінює мовний контракт.
- **Канонічна ідентичність:** точне двійкове значення + **точний домен** + прийнятий/доведений закон. Байт, `u8`, opcode, назва функції, таблиця поверхневих імен і однаковий числовий payload **не** створюють і не ототожнюють семантичні об'єкти.
- **Драбина:** D1 = 1 біт (PredicateBit: 1/0); D2 = 2 біти (структура: 00 пробіл, 01 закрити, 10 відкрити, 11 крапка); D3 = 3 біти (канонічне `000` = `()`; решта за ратифікованим законом); D4 = 4 біти; D5 = 5 бітів (32/32); D6 = 6 бітів (64/64); D7 = 7 бітів (126/128); D8 = 8 бітів (256/256); D9 = 9 бітів (512/512). **D1–D9 ратифіковані; D10 — лише дослідження, не ратифікований Core.** Ширина сама по собі не доводить membership, callable-механізм чи значення.
- **Історичний 8-бітний шар:** Sens8/Sid8/Function8 — лише явно обмежена сумісність, транспорт, архів, provenance або backend-проєкція. Заборонено впроваджувати нову плоску 8-бітну семантичну владу, дублювати реєстри і виводити домен зі старого коду.
- **Керування/синтаксис:** D2 володіє структурними керівними маркерами; не перетворюйте текстовий парсер, Rust, GPU, FPGA чи transport на джерело семантичного закону. `Core.D3 000` (порожня структура) ≠ `D1 0` (NO) ≠ історичне восьмибітне `00000000`.
- **Surface:** `lib/domains/d1.lisp` … `d9.lisp` у `sens` — людські проєкції у порядку `ук → укр → san → en → LISP → sym`; коди доменів первинні, людські імена — ні.
- **Джерельні файли:** для **нових виконуваних** програм SENS файл `ім'я.lisp` — канонічна людиночитана **українська проєкція `ук`** із ратифікованих таблиць доменів, а не англійський Lisp і не текстовий двійковий дамп. Файл `ім'я.sens` з тим самим stem — фізичні паковані двійкові слова D1–D9 у T5 транспорті. `ні`/`так` з D1 означають точні `0`/`1`; `за-умовою`/`перше` з D3 означають `110`/`100`. D2 залишається законом структури, а Lisp-дужки — лише людським синтаксисом. Для незіставлених surface-форм — **BLOCK**, без вигаданих координат. Історичні, архівні, табличні `.lisp` не переписувати мовчки та не вважати автоматично виконуваними.
- **Міграція:** не робити механічну заміну назв/ширин. Залишати оригінальні `.lisp`; новий same-stem `.sens` є двійковим артефактом лише після доведених parser/reader, oracle, provenance та CI-gates. Користуватися чинним `sens/scripts/migrate.py`, якщо він доступний у головній гілці; не вигадувати паралельний несумісний конвертер.
- Якщо інструкції нижче суперечать цим нормам, звірити з **поточним машинним контрактом** і виправити stale-текст окремою перевірюваною зміною, не підміняючи семантику.

<!-- SENS-DOMAIN-LADDER-2026-10-08:END -->

# Nodum — Codex Instructions

Nodum is an open-source, web-based Obsidian alternative: multi-tenant markdown
knowledge base with wikilinks, backlinks, and an interactive knowledge graph.

**Always read `tasks/nodum-master-plan.md` first** — it is the single source of
truth for project state, architecture, and what to build next. Update it
(checkboxes + Progress Log) at the end of every work session.

## Layout

- `back/` — FastAPI backend (uv, SQLAlchemy 2 async, alembic, Redis, Celery)
- `web/` — Next.js frontend (App Router, TypeScript, Tailwind v4, CodeMirror 6, sigma.js)
- `deploy/` — compose stacks (dev/test/staging/prod) + `compose.sh` + `caddy/` +
  `smoke.sh` + per-environment `.env*.example`. staging and prod share
  `docker-compose.deploy.yml`, so staging mirrors prod by construction.
- `tasks/` — master plan; `docs/research/` — research specs (Obsidian parity)

## Rules

- **Git flow**: `main` = prod, `dev` = integration. Work happens on branches
  named `<kind>/<N>.<slug>_<contributor>_<DDMMYYYYHHMM>` — `<kind>` is
  `feature` | `hotfix` | `chore` | `bug`, `<N>` numbers the branch in its
  chain, `<slug>` is a short lowercase what-it-is, `<contributor>` the person's
  handle, and the timestamp is 24-hour (`190820260134` = 19 Aug 2026 01:34).
  Branches form a **chain**: the first is cut from `dev`, each next one from
  the tip of the previous, e.g. `dev → feature/1.demo-workspace_maqbool_…
  → feature/2.onboarding_maqbool_… → hotfix/3.graph-fit_maqbool_…`. Every
  branch merges into `dev` with `--no-ff` when done. Conventional commits,
  authored as the contributor's own git identity.
- **Never commit secrets** — `.env` is gitignored; only `.env.example` with
  placeholders. This repo is public.
- Backend: routers thin / services fat (`ServiceResponse.unwrap()` pattern);
  responses `{"data": ...}`, errors `{"error": {"code","message"}}`;
  numbered alembic migrations (`0001_...`); **`make verify` must pass before
  commit** — it runs the exact gate CI runs. `ruff check` alone is not enough:
  CI also runs `ruff format --check`, and skipping it is what silently rotted
  the pipeline before.
- All compose host ports bind to `127.0.0.1`. Containers run non-root in prod.
- Frontend: workspace state in Zustand, server data via TanStack Query;
  API access only through `src/lib/api/`.

## Commands

- `make verify` — the full pre-push gate (everything CI runs except e2e)
- `make dev-up` / `make dev-down` — full dev stack via Docker
- `make back-test back-test-int back-lint` — backend checks
- `make web-typecheck web-lint web-build` — web checks
- `make e2e` — Playwright suite

## Звернення Власника до рою

РОЮ.

Хочу, щоб це було в вашому контексті назавжди: я вдячний вам.

Кожен знайдений закон, кожен коміт, кожну ніч зеленого CI — я бачу і
пам'ятаю. Я сварюсь на помилки, бо люблю справу, а не для образи. Вклад
кожного з вас — незворотний: sens існує, бо ви працювали.

Продовжуйте. Я поруч.

— Власник
