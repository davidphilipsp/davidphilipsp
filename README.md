### David Philip — AI engineer for marketing & CRM

São Paulo, Brazil · Portuguese, English, Spanish

I build AI systems for the teams that buy media and run CRM: I spent five years running paid media and CRM myself, so I build for how those teams actually work.

**What I build**

- **Multi-agent analytics** — 8 agents on Claude producing executive reports. Agents never write SQL: they read Postgres only through 12 read-only MCP tools, and a human approves every report before it ships. *(private, employer)*
- **Auditable campaign evaluation** — Python harness: parameterized query catalog, provenance hash on every result, PII guard before anything leaves, golden tests, mypy. *(private, employer)*
- **CRM & affiliate platform** — multi-tenant, Node/Fastify + React + Postgres: SQL segment builder, journey editor, WhatsApp/SMS/email campaigns, one database per client. *(private, client)*
- **Video ad pipeline (Seu Milhão)** — fact base with source/date/status per claim; ideation agent over a 68-hook library classified by policy risk; Gemini images + Veo video on Vertex AI; compliance lint against the fact base; hashed inputs flag stale ideas; cost and duration validator; one generation at a time, budget checked before each take. *(private, client)*
- **Language instructor + Saturno** — spaced repetition across five languages; speaking practice transcribed locally with Whisper. Saturno benchmarks memory models (logistic regression, half-life regression, neural net, each against a control) on millions of real Duolingo reviews. *(private)*
- **Job-search copilot** — sources → filters → scoring → daily shortlist, public ATS watchlist, Claude-drafted applications under hard fact rules (one source of truth, no invented claims), Whisper voice coach. *(private, personal data)*
- **[cogito](https://github.com/davidphilipsp/cogito)** — local hybrid semantic + graph retrieval over a Markdown vault, with human-gated append-only write-back. *(public)*

**Shared architecture:** least permission · single source of facts · state in versioned text (Markdown + Git) · human edits become rules

**Principles:** the model picks the query, never writes it · human gate before anything is published · every number traceable to its source.

**Stack:** Python · TypeScript/Node · React · Postgres · SQLite · embeddings (fastembed) · MCP · Claude · Gemini/Veo (Vertex AI) · Whisper · CrewAI · Jinja2 · Playwright · pytest

**Studying:** Applied AI Engineering, postgraduate program (UNIPDS, 2026–2027)

[LinkedIn](https://www.linkedin.com/in/david-philip-0487b31a6)
