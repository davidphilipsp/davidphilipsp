### David Philip — AI engineer for marketing, CRM & data

São Paulo, Brazil · Portuguese, English, Spanish · [LinkedIn](https://www.linkedin.com/in/david-philip-0487b31a6)

I spent five years running paid media and CRM myself (Meta, Google, TikTok; lifecycle, loyalty, retention). Now I build the systems those teams need: agents that analyse data without inventing it, pipelines that produce ads under regulation, and platforms that run CRM end to end. I build with Claude Code as a pair programmer; I own the architecture, the contracts and the review.

**How I build**

- **The model chooses, it never writes the query.** Agents read data through fixed, read-only tools or parameterised query catalogues.
- **Human gate before anything ships or spends.** Reports, ads, edits and paid generations wait for approval.
- **Every number has a source.** Provenance hashes, fact bases with source/date/status, data vs. inference marked apart.
- **State in versioned text.** Markdown/JSON + Git as the source of truth; append-only ledgers; nothing silently overwritten.
- **Tests that guard the rules,** not just the code: golden files, isolation tests, PII guards, determinism checks.

---

#### Marketing & creative AI

**[Seu Milhão creative pipeline](https://github.com/davidphilipsp/seu-milhao-creative-pipeline)** — AI video-ad production for my own brand (a SUSEP-regulated sweepstakes).
- 18-step pipeline (Python, Typer CLI) with 4 human approval gates and a local approval panel. Principle: "reflex before reasoning": mechanical steps are plain Python commands that cost zero tokens.
- 10 Claude Code agents with separate roles: strategist, scriptwriter, director, auditor with veto power, and a video operator who is the only one allowed to spend.
- Generation on Vertex AI: `gemini-3.1-flash-image` for frames, `veo-3.1` (standard / fast / lite) for video; whisper.cpp and ffmpeg for audio and assembly.
- Fact base where every claim carries value, source, date and status; deterministic compliance lint (forbidden claims, divergent facts, missing legal footer, wrong SUSEP data).
- Cost control: zero-cost dry run, explicit confirmation to spend, a lock allowing one paid creative at a time, cost ledger with a per-creative cap and an 80% alert.
- **Script Room**: isolated module with no media spend and its own CLI. Inputs are SHA-256 hashed into a manifest, so a script built on outdated sources is flagged *stale* with the diff. Validator for coherence, duration (calibrated words/second), AI-shot length and cost. Hook library of 68 hooks in 9 families, each classified by ad-policy risk with the reason and a compliant version.
- 69 pytest tests (pipeline, lint, lock, dry run, Script Room isolation).

**[Seu Milhão CRM research](https://github.com/davidphilipsp/seu-milhao-crm-research)** — evidence-tagged PRD ([E]/[C]/[I]) and market research behind the CRM: regulation, competitors, channels, CRM UX decisions.

#### CRM & data platforms

**[Bar do Milhão platform](https://github.com/davidphilipsp/bar-do-milhao-platform)** — affiliate + CRM platform for my brand: sales links, commissions and regional leagues for bars and resellers, plus a backoffice with CRM, attribution, WhatsApp/Telegram support and finance.
- Node 20 (ESM) + Fastify 5, React 19 + Vite, Tailwind, Postgres 16 behind PgBouncer; workers for stats pre-aggregation and server-side conversions to Meta, Google and TikTok; deployed per client with systemd.
- Multi-tenant by isolation: one install per client company (own database, env, domains and Linux user), provisioning and publishing via scripts; inside a tenant every query is scoped and tested for isolation.
- CRM: segments stored as structure (never as SQL), visual journey editor (React Flow), WhatsApp/SMS/voice/email campaigns, RFM and cohorts.
- Invariants: append-only commission ledger, soft deletes only, missing data is null and never zero, deterministic A/B assignment, separate read/write credentials, admin money actions audited, personal data never logged; 215 immutable checksummed migrations.
- ~100 HTTP-level test scripts against a real server, a guard that kills tests that touch production, and a 45-rule engineering guide kept as living ADRs. 941 commits in two months.

**[CRM campaign harness](https://github.com/davidphilipsp/crm-campaign-harness)** — decides whether a CRM campaign result can be called *causal*, and refuses when the method doesn't hold.
- Python 3.11 with zero runtime dependencies; frozen dataclasses; mypy strict, ruff, 313 passing tests, byte-deterministic golden tests, pre-commit hook.
- Zoned funnel: source adapters behind one Protocol (CSV, Postgres) → digest + 7 method lints → hash-chained ledger with `verify`; a preflight checks the campaign design *before* go-live.
- Holdout vs. comparison vs. treatment as a typed concept: no holdout, no causal claim. Every number labelled measured / estimated / absent; provenance (source + sha256) in every result; PII guard on data and on free text before publishing.

**[CRM orchestrator](https://github.com/davidphilipsp/crm-orchestrator)** — Claude (`anthropic` SDK) scoring copy by channel and segment, a 9-agent squad config (copy, journeys, segments, calendar, gamification, compliance), hypothesis log kept append-only; the API receives aggregates only. pandas + DuckDB.

**[iGaming CRM automations](https://github.com/davidphilipsp/igaming-crm-automations)** — 1:1 behavioural email through the Smartico events API with an email-safe Liquid widget, an HTTP 500 root-cause write-up, and an anti-churn split that assigns users deterministically (MD5 mod 100).

#### Analytics, risk & reporting

**Risk Squad** — multi-agent analytics for two iGaming brands. *(source not public: employer system)*
- 8 agents on Claude: orchestrator, six domain analysts (KPIs, revenue, player risk, acquisition, payments, CRM) and a publisher.
- A Python MCP server over a read-only Postgres replica: 12 read-only tools, SQL guard, row caps and statement timeouts. Agents never write SQL.
- Fact vs. inference kept separate, PII masked in shareable output, human approval before publishing; reports rendered with Jinja2 and exported to PDF.

**[Sentinela risk lab](https://github.com/davidphilipsp/sentinela-risk-lab)** — a deterministic, offline rebuild of that risk analysis, with no AI in the loop, on synthetic data.
- Python, DuckDB, Jinja2, WeasyPrint; CSV contracts and YAML study definitions; 8 behavioural risk flags over 40+ per-player features.
- Keyed-hash pseudonyms, AES-256-GCM vault, run IDs that give byte-identical reruns, air-gapped install from a wheelhouse with checksums, hardened process (umask 077, no core dumps, RAM-only workspace) and a `doctor` command. 59 tests for contracts, privacy, security and reproducibility.

#### Retrieval & knowledge

**[cogito](https://github.com/davidphilipsp/cogito)** — local hybrid retrieval over a Markdown vault. *(public)*
- fastembed (`all-MiniLM-L6-v2`) embeddings in SQLite, cosine ranking; graph recall by BFS from the top semantic hits over typed wikilinks, re-ranked by proximity.
- The model can only *propose* append-only edits, shown as a diff and committed after human confirmation. 37 pytest tests.

**Decision circuit** — a typed knowledge graph in Obsidian (500+ notes, 100+ commits). *(private: personal content; the engine is cogito)*
- Typed notes with a fixed frontmatter schema and 13 templates; a 5-step reasoning pipeline; every finding marked data or inference with its own confidence.
- Two agents under a territorial contract: a local `qwen3:8b` scribe (via Telegram) may only write raw material; Claude Code is the analyst. Voice notes transcribed offline with whisper.cpp; Git is the source of truth.

#### Learning & ML research

**[Language instructor](https://github.com/davidphilipsp/language-instructor)** — a personal tutor for five languages (EN, ES, RU, JA, FR).
- 8 Claude Code agents (session planning, review scheduling, drills, script and sound, speaking coach, anti-abandonment guard) under 10 design principles and 8 routing rules.
- Vanilla-JS study app with 12 exercise types; spaced repetition on a fixed interval ladder that needs correct answers in 2+ modalities before an item counts as learned; weekly review floor per language.
- Speaking practice transcribed locally with whisper.cpp (`large-v3-turbo`). State lives in Markdown/JSON files with an allowlist of writable keys, atomic writes, an append-only log and a nightly Git backup. 86 `node:test` tests; branch per fix.

**[Saturno](https://github.com/davidphilipsp/saturno-memory-models)** — a pre-registered study of memory models for spaced repetition.
- Public Duolingo dataset (12.8M review traces, Settles & Meeder, ACL 2016); a ladder of models (Leitner and Pimsleur baselines → logistic regression → half-life regression → an MLP with lexeme embeddings in TensorFlow.js), each compared only to the step before.
- A model "wins" only if a learner-paired bootstrap 95% CI on the error difference excludes zero; feature ablations, a matched linear control for the neural net, rules fixed in advance with dated amendments, tests checked by mutation before each run.

#### Tools

**[Job-search copilot](https://github.com/davidphilipsp/job-hunt-copilot)** — public job APIs and company ATS feeds → filters → lane → additive scoring → a daily shortlist; Claude drafts applications that are rejected if they break the fact rules (single source of truth, banned phrases, only whitelisted numbers); a local tracking page; a Whisper voice coach (faster-whisper, low-confidence words flagged). 

---

**Stack:** Python · JavaScript (Node, React) · Postgres · DuckDB · SQLite · pandas · MCP · Claude / Claude Code · Gemini & Veo (Vertex AI) · Whisper (whisper.cpp, faster-whisper) · fastembed · TensorFlow.js · Jinja2 · Playwright · pytest · Git

**Studying:** Applied AI Engineering, postgraduate program (UNIPDS, 2026–2027)

<sub>Public repos are sanitized copies with fresh history: no credentials, personal data, client data or business figures. Employer-linked work is de-identified.</sub>
