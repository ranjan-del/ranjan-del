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

## Built from scratch

Three systems of my own. Not a fork, not a tutorial build, not somebody else's idea rebuilt: for each
one I started from a problem I ran into, wrote the problem statement and the architecture, and built
against my own roadmap. The design decisions, the trade-offs and the phasing are mine to defend.

### RagFabric

> Self hosted, measurement first RAG platform. Four retrieval architectures behind one interface.

**Why it exists.** Most RAG tools give you one retrieval approach and a chat box. RagFabric is built
to answer a harder question: given *your* documents, *your* questions and *your* budget, which
retrieval architecture should you run, and why? It measures accuracy, latency and cost on your own
corpus, then serves the winner behind a query router.

| | |
|---|---|
| **Engineering areas** | Traditional, Vectorless, Agentic and Graph retrieval · query routing · evaluation harness · ingestion and chunking · groups, grants and scoped API keys · OpenTelemetry tracing · cost and latency accounting · pluggable providers and stores |
| **Stack** | Python 3.13 (uv) · FastAPI · PostgreSQL + pgvector · Redis workers · Chroma · PostgreSQL recursive CTEs for the graph (no Neo4j) · Angular + Tailwind · Docker Compose · Apache 2.0 |
| **Working today** | Offline single strategy assistant: ingestion for PDF/DOCX/PPTX/TXT/CSV with page numbers and character spans surviving into citations, hybrid retrieval with relevance floors, JWT auth and RBAC, Alembic migrations tested against real PostgreSQL, 390 Python tests plus 20 frontend tests. Phase 2 added shared ingestion, the access filter applied inside every store query, Redis backed index fan out, the CLI and tracing spans. Phase 3 added the Traditional RAG serving path: pgvector with an HNSW index and a pinned 768 dimension, Chroma as a second store, optional LLM and cross encoder reranking, a citation contract that checks every quote against its chunk before an answer is returned, streaming and non streaming `/v1/ask`, and a Python SDK. It answers with citations on a machine with no API key. Phase 4 added Vectorless RAG (BM25 and PostgreSQL full text search with fusion, exact phrase and identifier boosting) and console v1. Phase 5 added Agentic RAG as a plain Python state machine with a bounded repair loop, budgets and traces. Phase 6 added Graph RAG: entity extraction behind a confidence floor, reversible entity resolution, and an access checked traversal written as PostgreSQL recursive CTEs. |
| **Not built yet** | Phases 7 to 10: the adaptive query router and terminal experience, the evaluation framework, the Compare and Trace UI with a TypeScript SDK, and production hardening. |
| **Releases** | v0.1.0 (traditional and vectorless) · v0.2.0 (agentic) · v0.3.0 (graph) · v0.3.1 (PyPI packaging), all September 2026 |
| **Status** | **Actively building** · alpha on PyPI |

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
| **v0.2.0 (Sep 2026)** | The macOS desktop panel with an Assistant tab (a chat backed by one warm Claude Code process, turns routed to Haiku, Sonnet or Opus, and risky actions held for an inline Approve or Cancel), background capture, and a weekly to-do list stored as one Markdown file per ISO week with a month calendar. |
| **Not built yet** | Open and Resume in Claude, Windows and Linux builds, and installers on GitHub Releases (v0.3.0), then PR status and calendar connectors (v0.4.0). |
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

## Major featured projects

Substantial builds where the infrastructure around the model is the point.

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

---

## Open Source

I contribute to open source AI and developer infrastructure, focusing on correctness, reliability,
testing, security and maintainability. Each PR starts from a bug I reproduced on the project's own
`main`, and carries a regression test or a measurement that shows the fix.

| Status | Count |
|---|---|
| ✅ Merged | 1 |
| 🟡 Open, awaiting review | 2 |
| ⚪ Closed without merge | 2 |

### ✅ OpenAI Agents SDK, [#4941](https://github.com/openai/openai-agents-python/pull/4941): merged

**fix(sandbox): keep UnixLocal workspace root removal off the event loop**

| | |
|---|---|
| **Problem** | `UnixLocal` sandbox teardown called `shutil.rmtree` directly inside an `async def`, so the whole event loop froze for the full tree walk. Streamed responses, MCP sessions and tracing exporters on the same loop all stalled. Sibling operations in the same file already used the blocking IO helper. |
| **Fix** | Run the removal through the existing `run_blocking_workspace_io` helper, so the tree walk happens on a worker thread and the loop keeps running. |
| **Evidence** | A benchmark the maintainer asked for, comparing the released code path with this change on a real `npm install` workspace, with nothing patched or delayed. At 52,554 files the worst event loop stall fell from **2,276 ms to 1.0 ms**, and a 5 ms ticker got 476 of 474 due ticks instead of 1. Delete time rose 1 to 6 percent (the cost of the thread hop), and I reported that too. Also measured: cancellation mid removal and how it affects resume state serialisation. |
| **Area** | asyncio correctness · blocking calls in async code · benchmarking |
| **Outcome** | Approved and squash merged on 27 Sep 2026 into the `0.22.x` milestone. |

### 🟡 LiteLLM, [#39894](https://github.com/BerriAI/litellm/pull/39894): open

**test: drop the `__file__` relative `sys.path.insert` calls from the unit tests**

| | |
|---|---|
| **Problem** | 42 unit tests still called `sys.path.insert` to add the repo root (test quality rule TQ003). pytest already puts the repo root on `sys.path` and `litellm` is installed, so the inserts were dead code. |
| **Fix** | Removed the 41 dead call sites and their unused imports, kept the one legitimate case with a documented exemption, and ratcheted the TQ003 budget from 62 to 20 so new ones fail the gate. |
| **Area** | Test hygiene at scale · quality gates |

### 🟡 LiteLLM, [#39891](https://github.com/BerriAI/litellm/pull/39891): open

**test(logging): undo the stdout patch inside the test so capsys never restores a closed stream**

| | |
|---|---|
| **Problem** | A teardown ordering bug: `monkeypatch` restored `sys.stdout` to capsys's already closed stream, so the test errored on every run. |
| **Fix** | Scoped the patch inside the test body, so it is undone before the fixtures are torn down. |
| **Area** | pytest fixture lifecycle · test reliability |

### ⚪ Gemini CLI: closed without merge

Both PRs were closed automatically by the project's bot after 14 days, because the issues they fixed did
not carry the `help wanted` label. No maintainer reviewed the code. The reproductions and the fixes are
still readable on the PR pages.

| PR | Problem found | Fix | Area |
|---|---|---|---|
| [#29249](https://github.com/google-gemini/gemini-cli/pull/29249) | The `get_internal_docs` path guard used a `startsWith` check with no path component boundary, so `../docs-private/secret.md` escaped the docs root and was read back to the model | Replaced the prefix check with the repo's existing `isSubpath()` helper (a `path.relative()` comparison that also handles case insensitive filesystems), plus a regression test that fails on `main` | Path traversal · security boundary |
| [#29244](https://github.com/google-gemini/gemini-cli/pull/29244) | Parallel tool execution let two `replace` calls on one file silently discard each other's edit while both reported success | Atomic tool file writes, with read modify write serialised per path. Four races reproduced on `main`, with one regression test each | Concurrency · atomic writes · lost updates |

All my pull requests: [github.com/pulls?q=author:ranjan-del](https://github.com/search?q=is%3Apr+author%3Aranjan-del&type=pullrequests)

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
