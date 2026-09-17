<h1 align="center">Ranjan G</h1>

<p align="center">
  <b>AI Engineer · Full Stack Developer · AI Systems &amp; Infrastructure</b><br/>
  I build AI systems, developer tools and backend infrastructure.
</p>

<p align="center">
  <a href="https://github.com/ranjan-del"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-ranjan--del-181717?logo=github&logoColor=white"></a>
  <a href="https://www.linkedin.com/in/iamranjan7204"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-iamranjan7204-0A66C2?logo=linkedin&logoColor=white"></a>
  <a href="mailto:ranjang140503@gmail.com"><img alt="Email" src="https://img.shields.io/badge/Email-ranjang140503%40gmail.com-D14836?logo=gmail&logoColor=white"></a>
</p>

---

## About

I started in full stack engineering, backend APIs, Angular frontends and the infrastructure around
them, and I am now working deeper into AI engineering: retrieval systems, agents, the Model Context
Protocol, inference runtimes and the tooling and observability that sit around all of it.

What I care about is the layer underneath the API call. Most of my repositories run offline with no
API key on purpose, because a deterministic stand-in makes the *system* visible instead of the model,
and it means a behaviour can be pinned down by a test. Every project carries an explicit status, a
limitations section and a roadmap, so nothing reads as finished before it is.

> I learn by understanding systems, building them, measuring them, breaking them, and contributing back.

## What I build

My work spans the AI application layer and the infrastructure layer beneath it.

```
   AI applications           assistants, agents, developer tools
           |
   LLMs / RAG / agents       retrieval strategies, routing, tool use, MCP
           |
   Inference / runtime       queueing, scheduling, model placement, budgets
           |
   Infrastructure            storage, workers, access control, tracing, cost
           |
   Measured AI systems       evaluation, benchmarks, honest status
```

---

## Featured projects

### RagFabric

> Self hosted, measurement first RAG platform. Four retrieval architectures behind one interface.

**Why it exists.** Most RAG tools give you one retrieval approach and a chat box. RagFabric is built
to answer a harder question: given *your* documents, *your* questions and *your* budget, which
retrieval architecture should you run, and why? It measures accuracy, latency and cost on your own
corpus, then serves the winner behind a query router.

| | |
|---|---|
| **Engineering areas** | Traditional, Vectorless, Agentic and Graph retrieval · query routing · evaluation harness · ingestion and chunking · groups, grants and scoped API keys · OpenTelemetry tracing · cost and latency accounting · pluggable providers and stores |
| **Stack** | Python 3.13 (uv) · FastAPI · PostgreSQL + pgvector · Redis workers · Chroma · Neo4j · Angular + Tailwind · Docker Compose · Apache 2.0 |
| **Working today** | Offline single strategy assistant: ingestion for PDF/DOCX/PPTX/TXT/CSV with page numbers and character spans surviving into citations, hybrid retrieval with relevance floors, JWT auth and RBAC, Alembic migrations tested against real PostgreSQL, 221 Python tests plus 20 frontend tests. Phase 2 added shared ingestion, the access filter applied inside every store query, Redis backed index fan out, the CLI and tracing spans. |
| **Not built yet** | Phases 3 to 10: pgvector serving path, BM25 lexical retrieval, agentic and graph strategies, the adaptive router and the evaluation framework are on the roadmap, not in main. |
| **Status** | **Actively building** · pre-alpha |

[github.com/ranjan-del/ragfabric](https://github.com/ranjan-del/ragfabric)

---

### Ledge

> A local first work assistant for people who use Claude Code. Tasks live in plain Markdown files.

**Why it exists.** AI assisted development loses the thread between sessions. Ledge keeps the current
task, the backlog and the unpushed git work in Markdown files that Claude Code edits directly, so a
session starts already knowing what the last one was doing. Nothing is hosted and nothing leaves the
machine.

| | |
|---|---|
| **Engineering areas** | CLI design · task state and a round trip safe Markdown parser · Claude Code plugin, slash command and SessionStart/Stop/PreCompact hooks · session linking · `git status --porcelain=v2` parsing for the pending view · OS file watching · low overhead desktop shell |
| **Stack** | Node 22 with native TypeScript (no build step) · Tauri 2 · Svelte 5 · Vite · Vitest · `objc2` AppKit bindings for the macOS window level · GitHub Actions · MIT |
| **Working today** | `@ledge/core`, the full `ledge` CLI (add, start, park, done, plan, note, when, today, link, sessions, memory, scan) and the Claude Code plugin with its three hooks. Tests run under Node's built in runner, plus a shell test that replays recorded hook payloads. |
| **Not built yet** | The floating desktop panel is phase 1 and in progress. There is no release yet. |
| **Status** | **Actively building** · pre-alpha |

[github.com/ranjan-del/ledge](https://github.com/ranjan-del/ledge)

---

### Loomrun

> A scheduler and governor for one shared inference machine. Sits in front of Ollama or vLLM.

**Why it exists.** A small team runs one inference box. A chat app, a nightly ingestion job and a
classifier all hit it at once, and nothing is in charge: the person at the screen waits behind the
batch, the machine thrashes swapping models, a looping script is never stopped, and nobody can say
what anything cost. Large platforms solve this inside their own infrastructure. Loomrun explores what
that looks like for one machine.

| | |
|---|---|
| **Engineering areas** | Admission control and queueing · priorities and fair share between tenants · budgets and runaway job termination · swap aware model placement · a request ledger with cost, tokens and time · pass through proxy semantics so callers keep speaking the engine's own API |
| **Stack** | Python (uv) · Ollama and vLLM as the engines it fronts · MIT |
| **Status** | **Planning.** Nothing is implemented yet. Phase 0 is the landscape study, the problem statement and baseline load tests against bare Ollama. The README describes the intended shape, and the roadmap rule is that no number enters the repository unless it came from a real run. |

[github.com/ranjan-del/loomrun](https://github.com/ranjan-del/loomrun)

---

### Agent Lab

> A policy governed workflow agent: it plans multi step work, then has to justify every action.

**Why it exists.** Most agent demos show an agent doing things. This one is built to show an agent
correctly *refusing*, and to prove with numbers how often it gets that right. Every proposed action is
checked against a deterministic policy engine, and the run leaves a traced, evaluated record.

| | |
|---|---|
| **Engineering areas** | Agent loop and planning over calendar and transcript data · deterministic policy engine · embeddings and chunk retrieval · execution tracing · evaluation harness · one command reproducible environment |
| **Stack** | Python · FastAPI · PostgreSQL with Alembic migrations · Docker Compose · Makefile driven workflow |
| **Working today** | The skeleton: one command bring up, health check with dependency status, migrations reproducible from zero, a stable cacheable system prompt, and a `make task` path that runs one task end to end by replaying a scripted model. |
| **Not built yet** | The agent loop, the policy engine and the eval harness. The README states it plainly: week 1 of 24, and the numbers section is empty on purpose. |
| **Status** | **Early build** |

[github.com/ranjan-del/agent-lab](https://github.com/ranjan-del/agent-lab)

---

### MCP Server &amp; Client

> A complete Model Context Protocol implementation, both halves of the conversation.

**Why it exists.** MCP is easy to describe in a paragraph and fiddly in practice. This implements all
three primitives rather than only tools, and the client watches progress notifications stream back
from a long running call, which is the part most examples skip.

| | |
|---|---|
| **Engineering areas** | MCP tools, resources and prompt templates · stdio transport (the client spawns the server, no ports) · the full `initialize` → `list` → `call` lifecycle · progress notifications · safe tool surfaces: AST whitelist arithmetic instead of `eval`, a resolved path sandbox, bound SQL parameters, config redaction |
| **Stack** | Python · official MCP SDK with FastMCP · asyncio · SQLite · Docker · 45 pytest tests on Python 3.11, 3.12 and 3.13 in CI |
| **Status** | **Working** · runs offline with no API keys |

[github.com/ranjan-del/mcp-server-client](https://github.com/ranjan-del/mcp-server-client)

---

### Enterprise AI Agent Platform

> A multi tenant agent workspace: the infrastructure around an agent, built honestly.

**Why it exists.** The interesting part of an agent product is not the model call, it is tenancy,
memory layering, tool sandboxing, approval gating, tracing and analytics. This builds that
infrastructure so that swapping the deterministic responder for a real model is a contained change
rather than a rewrite.

| | |
|---|---|
| **Engineering areas** | Deterministic `plan → memory → act → reflect → respond` state graph with an execution trace · tenant isolation re-checked on every id taking endpoint · RBAC via a dependency factory, with a clean 401/403 split · JWT access and refresh tokens · four layer memory (session, persistent, user facts, vector) · human in the loop approval that resumes mid graph without replaying side effects · SSE streaming over the graph's own `stream()` primitive |
| **Stack** | FastAPI · SQLAlchemy · Alembic · PostgreSQL (SQLite locally) · optional Redis · Angular 18 with signals · Docker Compose |
| **Status** | **Working** · boots, chats, calls tools and passes its suites with no API key and no external service |

[github.com/ranjan-del/enterprise-ai-agent-platform](https://github.com/ranjan-del/enterprise-ai-agent-platform)

---

## Other AI and systems work

Smaller repositories, each built to understand one thing properly. All run offline and are covered by
tests in CI.

| Repository | What it is | Notes |
|---|---|---|
| [rag-pipeline](https://github.com/ranjan-del/rag-pipeline) | End to end RAG, PDF upload to a grounded answer with citations | Every stage in its own module: parse → chunk → embed → store → retrieve → generate. The relevance threshold is what stops it answering confidently from nothing. 19 tests |
| [langgraph](https://github.com/ranjan-del/langgraph) | Ten LangGraph patterns, one folder each | State schemas, reducers, conditional edges, checkpointers, interrupts, `ToolNode`, stream modes. Deterministic stand-ins instead of a provider. 32 tests |
| [langchain](https://github.com/ranjan-del/langchain) | Ten LangChain building blocks, reimplemented | Each abstraction rebuilt with the same public shape and no package at runtime, so the workflow is visible rather than hidden behind an SDK. 57 tests |
| [prompt-engineering](https://github.com/ranjan-del/prompt-engineering) | A ten technique handbook | Same eight section treatment each: when it helps, when it is the wrong tool, before and after, failure modes. CI validates the structure |
| [websocket-chat](https://github.com/ranjan-del/websocket-chat) | Multi room real time chat, as a study of the protocol | Fan out that survives a dead socket mid broadcast, reconnect with backoff, id based de-duplication on replay. FastAPI + Angular 17 |
| [jwt-auth-lab](https://github.com/ranjan-del/jwt-auth-lab) | Token auth, the parts tutorials skip | Refresh rotation with reuse detection and token families, self sweeping revocation tables, shared in-flight refresh on the client |
| [earthquake-visualizer-LLM-tool](https://github.com/ranjan-del/earthquake-visualizer-LLM-tool) | Live USGS earthquake map with proximity based risk banding | React, Vite, Tailwind, React Leaflet, Firebase Hosting |

---

## Open Source

I contribute to open source AI and developer infrastructure, focusing on correctness, reliability,
testing, security and maintainability.

| Project | Contribution | Technical area | PR |
|---|---|---|---|
| **Gemini CLI** | Closed a sibling prefix bypass in the `get_internal_docs` path guard: a `startsWith` check with no path component boundary let `../docs-private/secret.md` escape the docs root and be read back to the model | Path traversal, security boundary, regression tests | [#29249](https://github.com/google-gemini/gemini-cli/pull/29249) |
| **Gemini CLI** | Made tool file writes atomic and serialised read-modify-write per path. Parallel tool execution let two `replace` calls on one file silently discard each other's edit while both reported success | Concurrency, atomic writes, lost update races (4 reproduced on main, one regression test each) | [#29244](https://github.com/google-gemini/gemini-cli/pull/29244) |
| **LiteLLM** | Removed 41 dead `__file__`-relative `sys.path.insert` calls across 43 test modules and ratcheted the test quality budget from 62 to 20 | Test hygiene at scale, quality gates | [#39894](https://github.com/BerriAI/litellm/pull/39894) |
| **LiteLLM** | Fixed a teardown ordering bug: `monkeypatch` restored `sys.stdout` to capsys's already closed stream, so the test errored on every run. Scoped the patch inside the test body instead | Fixture lifecycle, test reliability | [#39891](https://github.com/BerriAI/litellm/pull/39891) |
| **OpenAI Agents SDK** | Moved the `UnixLocal` sandbox workspace root removal off the event loop. `shutil.rmtree` inside an `async def` held the loop for a full tree walk while sibling operations already used the blocking IO helper | asyncio correctness, blocking calls in async code | [#4941](https://github.com/openai/openai-agents-python/pull/4941) |

All five were opened in September 2026 and are **under review, not yet merged**.

---

## Technical stack

Only what is used in public repositories here.

| | |
|---|---|
| **Languages** | Python · TypeScript · JavaScript · Rust · Bash · SQL |
| **AI engineering** | RAG (traditional, hybrid, lexical, graph, agentic) · embeddings and vector search · retrieval evaluation · query routing · agent loops and tool use · Model Context Protocol · LangGraph · prompt engineering · offline deterministic test doubles |
| **Backend** | FastAPI · Node.js · REST · Server Sent Events · WebSockets · JWT auth, refresh rotation and RBAC · async and concurrency · background workers |
| **Data** | PostgreSQL · pgvector · Redis · SQLite · Chroma · Neo4j · SQLAlchemy · Alembic · Prisma |
| **Infrastructure** | Docker and Docker Compose · GitHub Actions · OpenTelemetry · uv · Makefile driven workflows · Linux · Firebase Hosting |
| **Frontend** | Angular · Svelte 5 · React · Next.js · Tailwind CSS · RxJS · Vite |
| **Testing** | pytest · Vitest · Karma/Jasmine · Node test runner · migration drift tests · CI against real PostgreSQL |
| **Tooling** | Git · GitHub · Claude Code · Tauri · VS Code |

---

## Engineering interests

| Area | What I work on |
|---|---|
| **AI engineering** | LLM systems, RAG architectures, agents, MCP, evaluation |
| **AI infrastructure** | Inference serving, scheduling, model placement, resource governance, observability |
| **Backend and distributed systems** | APIs, queues, workers, caching, WebSockets, concurrency, access control |
| **Developer infrastructure** | CLI tools, git integrations, editor and agent plugins, CI/CD, developer experience |
| **Open source** | Testing, reliability, security, maintainability |

---

## Currently building

| | Project | Focus |
|---|---|---|
| 🧠 | **RagFabric** | Measurement first retrieval platform, four strategies behind one router |
| 🧰 | **Ledge** | Local first developer workflow tooling for AI assisted engineering |
| ⚡ | **Loomrun** | Inference scheduling and governance for a shared machine *(planning)* |
| 🤖 | **Agent Lab** | Policy governed agents, tool use and evaluation *(early build)* |

## Direction

The path I am actively working along:

```
Python → Machine Learning → Deep Learning → Transformers → LLMs
      → RAG → Agents → Inference → AI Infrastructure → Production AI
```

## How I work

```
Learn → Understand → Build → Measure → Break → Debug → Optimize → Contribute
```

A repository states what works, what does not and what is not built yet. No number is written down
unless a real run produced it.

---

## Explore my work

[RagFabric](https://github.com/ranjan-del/ragfabric) ·
[Ledge](https://github.com/ranjan-del/ledge) ·
[Loomrun](https://github.com/ranjan-del/loomrun) ·
[Agent Lab](https://github.com/ranjan-del/agent-lab) ·
[MCP Server &amp; Client](https://github.com/ranjan-del/mcp-server-client) ·
[Enterprise AI Agent Platform](https://github.com/ranjan-del/enterprise-ai-agent-platform) ·
[RAG Pipeline](https://github.com/ranjan-del/rag-pipeline) ·
[LangGraph patterns](https://github.com/ranjan-del/langgraph) ·
[Open source PRs](https://github.com/search?q=is%3Apr+author%3Aranjan-del&type=pullrequests)

<br/>

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-profile-summary-cards.vercel.app/api/cards/stats?username=ranjan-del&theme=github_dark">
    <img alt="GitHub stats" height="180" src="https://github-profile-summary-cards.vercel.app/api/cards/stats?username=ranjan-del&theme=github">
  </picture>
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-profile-summary-cards.vercel.app/api/cards/repos-per-language?username=ranjan-del&theme=github_dark">
    <img alt="Top languages" height="180" src="https://github-profile-summary-cards.vercel.app/api/cards/repos-per-language?username=ranjan-del&theme=github">
  </picture>
</p>
