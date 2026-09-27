# Course roadmap

> **Status as of 2026-09-27:** five courses are built and live. Seven remain from the original shortlist of ten. This document is the build plan for those seven: the order, the reasoning, and a one-page brief per course that a `COURSE_PLAN.md` can be written from.

Every course reuses the platform's strengths: a running fictional world with names that never change, at least six tiny examples per lesson, Eve reading the open lesson, multiple choice graded in code, and written work graded against a binary rubric. Every course fills a gap that a shipped course leaves open. See `docs/ADDING_A_COURSE.md` for the mechanics and `docs/AUTHOR_BRIEF.md` for the lesson format.

---

## 1. What is built

| Order | Course | Slug | Level | Lessons | Glossary | World |
|---|---|---|---|---|---|---|
| 1 | Spec-Driven Development for Dummies | `sdd-for-dummies` | Beginner | 9 | 59 | Bramble Books |
| 2 | AI-Augmented Engineering: Working with Coding Agents | `working-with-coding-agents` | Beginner | 7 | 68 | Bramble Books |
| 3 | Prompt Engineering for Engineers | `prompt-engineering-for-engineers` | Beginner | 7 | 69 | Bramble Books |
| 4 | Building and Evaluating AI Agents | `ai-agent-evals` | Beginner | 11 | 139 | Pip's Plant Shop |
| 5 | Security and Red-Teaming for Agents | `agent-security-and-red-teaming` | Intermediate | 10 | 92 | Pip's Plant Shop |

Two worlds carry everything. **Bramble Books** (the bookshop, its app Shelf, the coding agent Quill, the model-backed features Intake and Draft, the cast Omar, Nadia, Priya, Jun, Tessa, Mr. Hale) is where software gets specified, built, and prompted. **Pip's Plant Shop** (the support agent Sprout, its six tools with risk tiers T0, T1, T2, the cast Pip, Maya, Dev, Rosa, the customers Alex, Jordan, Sam, the supplier Fernworks, the attacks A-1 to A-6) is where an agent gets evaluated, attacked, and operated. New courses join one of these two worlds; none invents a third.

## 2. The build order

| Build | Course | Slug | For | Level | Catalog `order` |
|---|---|---|---|---|---|
| 6 | Tools and MCP Servers for Agents | `tools-and-mcp-servers` | Engineers | Intermediate | 6 |
| 7 | Writing Specs for AI Features | `writing-specs-for-ai-features` | Product managers | Beginner | 7 |
| 8 | Observability for LLM Apps: Traces, Spans, and Dashboards | `observability-for-llm-apps` | Engineers and SREs | Intermediate | 8 |
| 9 | AI Governance for Product Managers | `ai-governance-for-product-managers` | PMs and team leads | Beginner | 9 |
| 10 | Evals for RAG and Search | `evals-for-rag-and-search` | Engineers building retrieval | Intermediate | 10 |
| 11 | Cost and Latency Engineering for LLM Apps | `cost-and-latency-engineering` | Engineers who own a bill | Intermediate | 11 |
| 12 | Synthetic Data and Simulation for Testing | `synthetic-data-and-simulation` | Engineers and QA | Intermediate | 12 |

### Why this order

1. **Tools first, because three shipped courses point at the hole.** Agent Evals L1 introduces tool contracts, Security L3, L6, and L7 attack, guard, and gate tools, and Coding Agents L4 configures permissions from the consumer's side. Nobody teaches building a tool well. Sprout's toolset already exists with exact names and tiers, so this course needs no new world.
2. **Alternate engineer and PM courses.** The home page promises "engineers and PMs" and only SDD really serves the PM. Builds 7 and 9 are the PM courses; interleaving them keeps the catalog balanced as it grows instead of adding a PM track at the end.
3. **Follow the dependency chain.** Observability needs tools worth tracing (build 6). Cost and Latency profiles from traces (build 8). RAG Evals and Synthetic Data both deepen one lesson of Agent Evals and can wait. Specs for PMs sits upstream of SDD, which is already built.
4. **Put the date-sensitive course where it can be maintained.** Governance leans on regulation that moves (the EU AI Act's phase-in dates have shifted more than once). It is built fourth, not first, and its plan isolates every regulatory specific in one lesson with an "as of" date so updates touch one file.
5. **Prefer courses whose examples are code.** The platform's format is six tiny examples per lesson. Schemas, spans, metrics, and eval cases are tiny examples; policy is harder to make small. The engineering courses lead for that reason too.

### Dependencies at a glance

```
sdd-for-dummies ──────────────► writing-specs-for-ai-features (7)
                └──► agent-security L8 ──► ai-governance-for-product-managers (9)
ai-agent-evals L1 ─┐
agent-security L3/L6/L7 ─┴──► tools-and-mcp-servers (6) ──► observability-for-llm-apps (8) ──► cost-and-latency-engineering (11)
ai-agent-evals L2 ────────────────────────────────────────┘
ai-agent-evals L3 ──► synthetic-data-and-simulation (12)
ai-agent-evals L4/L5 + Sprout's search_care_guide ──► evals-for-rag-and-search (10)
```

Arrows mean "reuses and deepens", not "required first". Every course still starts with an L0 that stands alone.

---

## 3. Course briefs

Each brief is the seed of `docs/courses/<slug>/COURSE_PLAN.md`. Lesson lists are proposals: the plan may split or merge lessons, but it should keep the verbs, the world, the homework count, and the gap statement.

### Build 6 · Tools and MCP Servers for Agents

- **Slug:** `tools-and-mcp-servers` · **Level:** Intermediate · **Time:** about 8 h
- **Audience:** backend engineers who give a model tools, or are about to. You can read TypeScript and JSON Schema. You have seen an agent call a tool, in this platform's evals course or elsewhere.
- **Gap it fills:** Agent Evals L1 introduces tool contracts and Security L3, L6, L7 attack and guard them; nobody teaches designing, building, and testing a toolset, or exposing it over MCP.
- **Verbs:** **Design** (what should this tool promise, and to whom?) · **Build** (how is the promise kept in code, and how is it served?) · **Verify** (how do you know a tool, and the model's use of it, is right?)
- **World:** Pip's Plant Shop. Sprout's six tools and the two the security course added (`read_customer_notes`, `save_customer_note`), reused exactly. New, marked new in the plan: an MCP server `sprout-mcp` that serves the same tools to Maya's desk client so staff and agent share one contract; and one new analyst tool, `orders_report(range)`, borrowed from Agent Evals L3, as the running example for a badly designed tool that gets redesigned.
- **Platform tie-in:** B1 dissects Eve's own five zod-typed tools in `agent/tools/`, `defaultTools: false` in `agent/agent.ts`, and the per-session cost cap, the way the evals course's CI lesson points at the platform's own CI gate.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | What a Tool Is to a Model | 0 · Start Here | Design | 35 min |
| L1 | The Tool Contract: Name, Schema, Result, Errors | 1 · Design the Contract | Design | 55 min |
| L2 | Designing a Toolset: Granularity, Tiers, and Names | 1 · Design the Contract | Design | 50 min |
| L3 | Implementing a Tool: Validation Against the Real World | 2 · Build and Serve | Build | 60 min |
| L4 | Permissions in Code: Who Is Calling and What May They Do | 2 · Build and Serve | Build | 55 min |
| L5 | An MCP Server from Scratch | 2 · Build and Serve | Build | 60 min |
| L6 | Testing Tools in Isolation | 3 · Verify | Verify | 55 min |
| L7 | Tool-Selection Evals and Evolving a Toolset Safely | 3 · Verify | Verify | 50 min |
| B1 | Bonus: Dissecting Eve's Tools | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L2 (contracts for two new tools, with error cases and tiers). HW2 after L5 (build `sprout-mcp`, connect a client, submit the transcript and the `tools/list` output).
- **Glossary target:** about 70 terms (tool contract, tool loop, JSON Schema, idempotency, risk tier, least privilege, BOLA, MCP, transport, `tools/list`, `tools/call`, resource, prompt, tool-selection accuracy, deprecation).
- **Decide before writing:** which MCP SDK and transport shapes go in plan §7 (stdio for local, streamable HTTP for remote), and how much protocol detail L5 carries versus links.

### Build 7 · Writing Specs for AI Features

- **Slug:** `writing-specs-for-ai-features` · **Level:** Beginner · **Time:** about 6 h
- **Audience:** product managers and product-minded leads who own a feature that calls a model. You can read a user story and a simple table. No engineering or AI background is assumed.
- **Gap it fills:** SDD teaches the engineer's `spec.md`. The PM writes the document upstream of it, and today nothing on the platform teaches that document or how it turns into evals.
- **Verbs:** **Frame** (what outcome, for whom, and what is out?) · **Define** (what does correct mean, in a way someone can grade?) · **Launch** (what gates the release, and what keeps the spec true afterward?)
- **World:** Bramble Books. The student is **Nadia**, the product owner, for the first time. Omar, Priya, and Tessa keep their roles. The features are Intake and Draft from the prompt-engineering course, reused exactly, plus one new feature to spec from scratch, marked new: **Recommend**, a "you might also like" line added to Draft replies, which touches purchase history (privacy, Priya) and tone (Tessa). SDD's artifact chain (`intent.md`, `spec.md`, `plan.md`) is the hand-off target.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | Why an AI Feature Needs a Different Spec | 0 · Start Here | Frame | 35 min |
| L1 | Intent and Non-Goals | 1 · Frame | Frame | 45 min |
| L2 | Users, Inputs, and the Unhappy Paths | 1 · Frame | Frame | 50 min |
| L3 | Acceptance Criteria You Can Grade | 2 · Define | Define | 55 min |
| L4 | Metrics, Baselines, and Targets | 2 · Define | Define | 50 min |
| L5 | Risk, Policy, and Human Gates | 2 · Define | Define | 50 min |
| L6 | From Spec to Evals | 3 · Launch | Launch | 55 min |
| L7 | Launch Gates and the Living Spec | 3 · Launch | Launch | 45 min |
| B1 | Bonus: Reviewing an Engineer's spec.md as a PM | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L3 (a complete spec for Recommend: intent, non-goals, inputs, acceptance criteria). HW2 after L6 (turn that spec into ten eval cases with expected outcomes).
- **Glossary target:** about 55 terms (intent, non-goal, acceptance criterion, binary rubric, golden example, baseline, target, escalation rate, human gate, launch gate, kill criterion, eval case, living spec).
- **Decide before writing:** how much of SDD's vocabulary to import verbatim versus re-teach in PM terms; the plan should list the shared glossary ids explicitly.

### Build 8 · Observability for LLM Apps: Traces, Spans, and Dashboards

- **Slug:** `observability-for-llm-apps` · **Level:** Intermediate · **Time:** about 8 h
- **Audience:** engineers and SREs who run a model-backed feature in production. You can read TypeScript and a latency chart. No tracing background is assumed.
- **Gap it fills:** Agent Evals L2 ("Designing for Evaluability") is one lesson on logging and traces. Teams need a week: a data model, instrumentation, a backend, dashboards that do not lie, and alerts.
- **Verbs:** **Instrument** (what do you record, and how?) · **Inspect** (how do you read one trace and a thousand?) · **Alert** (what wakes someone up, and what closes the loop?)
- **World:** Pip's Plant Shop. Sprout in production, with the tools, tiers, guard log, and approval log from the security course. New, marked new: **Kofi**, the on-call engineer who owns Sprout's dashboards and alerts and pairs with Dev. Rosa's red-team findings become alert conditions.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | What Happens Between the Question and the Answer | 0 · Start Here | Instrument | 35 min |
| L1 | The Trace Data Model for LLM Apps | 1 · Instrument | Instrument | 55 min |
| L2 | Instrumenting with OpenTelemetry | 1 · Instrument | Instrument | 60 min |
| L3 | Shipping Traces: Backends, Sampling, Retention, Redaction | 1 · Instrument | Instrument | 50 min |
| L4 | Reading a Trace: Finding the Failing Step | 2 · Inspect | Inspect | 55 min |
| L5 | Dashboards That Do Not Lie | 2 · Inspect | Inspect | 55 min |
| L6 | Alerts, SLOs, and On-Call for an Agent | 3 · Alert | Alert | 55 min |
| L7 | Closing the Loop: From Traces to Evals | 3 · Alert | Alert | 50 min |
| B1 | Bonus: The Observability Checklist | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L2 (instrument Sprout's model call and two tools; submit one full trace). HW2 after L5 (a dashboard spec: six panels, the query behind each, and the lie each one avoids).
- **Glossary target:** about 75 terms (trace, span, attribute, event, span kind, context propagation, exporter, sampling, head vs tail sampling, retention, redaction, percentile, corrected prevalence, SLO, error budget, burn rate, feedback capture, drift).
- **Decide before writing:** the backend named in examples (the original shortlist says Langfuse; keep vendor-specific steps to L3 so the rest stays portable).

### Build 9 · AI Governance for Product Managers

- **Slug:** `ai-governance-for-product-managers` · **Level:** Beginner · **Time:** about 6 h
- **Audience:** product managers and team leads in regulated or customer-facing teams. You can read a policy and a risk table. No legal or engineering background is assumed.
- **Gap it fills:** SDD's adoption lesson and Security L8 each give governance one lesson, for an engineer. The PM who has to answer a partner's AI risk questionnaire, or an auditor, has no course.
- **Verbs:** **Assess** (what could go wrong, and what does the law call it?) · **Control** (which controls exist, and how does a PM verify them?) · **Attest** (what do you write down, and what do you show?)
- **World:** Pip's Plant Shop, as the original shortlist chose, because the controls this course points at (guards, approval queue, kill switch, governance record, guard and approval logs) are all security-course artifacts. New, marked new: **Grace**, the compliance lead at a garden-center chain that wants to resell Pip's plants and sends a 40-question AI risk questionnaire before signing. Pip is the student's stakeholder; Rosa and Maya supply the evidence.
- **Maintenance rule (part of the plan):** every regulatory specific (a date, a threshold, a tier definition) lives in L1 only, carries an "as of" date in the lesson text, and is reviewed each quarter. Other lessons refer to "the tier L1 assigns", never to a date.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | What Governance Is For | 0 · Start Here | Assess | 35 min |
| L1 | The Regulatory Floor in Plain English | 1 · Assess | Assess | 55 min |
| L2 | Risk Tiers for Your Own Features | 1 · Assess | Assess | 50 min |
| L3 | Controls That Exist in Code | 2 · Control | Control | 50 min |
| L4 | Transparency, Disclosure, and Consent | 2 · Control | Control | 45 min |
| L5 | The Governance Record | 3 · Attest | Attest | 50 min |
| L6 | Evidence Packs and Audits | 3 · Attest | Attest | 55 min |
| L7 | Incidents, Reviews, and Change Control | 3 · Attest | Attest | 45 min |
| B1 | Bonus: Grace's Questionnaire, Answered | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L2 (a risk register for Sprout: each tool, each entry point, a tier and an owner). HW2 after L6 (an evidence pack index for one control, with the log, the test, and the reviewer named).
- **Glossary target:** about 60 terms (risk register, risk tier, NIST AI RMF, govern/map/measure/manage, EU AI Act, transparency obligation, high-risk system, provider vs deployer, control, evidence, attestation, audit trail, change control, incident disclosure).
- **Decide before writing:** whether L1 names other regimes (sector rules, consumer protection, privacy law) or links out; and who reviews L1 each quarter.

### Build 10 · Evals for RAG and Search

- **Slug:** `evals-for-rag-and-search` · **Level:** Intermediate · **Time:** about 7 h
- **Audience:** engineers building or owning a retrieval feature: search, question answering over documents, or an agent tool that looks things up. You can read TypeScript and a table of scores. You have met the idea of an eval, here or elsewhere.
- **Gap it fills:** the evals course is agent-shaped. Retrieval fails in its own two stages, did not retrieve and retrieved but did not use, and has its own metrics, judges, and experiments.
- **Verbs:** **Retrieve** (what does the index return, and how do you know it should?) · **Judge** (is the answer grounded, correct, and honest about gaps?) · **Tune** (which change helped, and how do you avoid fooling yourself?)
- **World:** Pip's Plant Shop. The retrieval feature is Sprout's existing `search_care_guide(query)` over Pip's own guides and the Fernworks PDFs. **Jordan** (order #1077, repotting) is the recurring user. Security attack **A-2**, the white-text instruction in a Fernworks PDF, returns in L7 as a retrieval-quality problem. New, marked new: a second corpus, Pip's internal returns policy, so lessons can show retrieval choosing the wrong source.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | Why Retrieval Fails Differently | 0 · Start Here | Retrieve | 35 min |
| L1 | Anatomy of the Care-Guide Search | 1 · Retrieve | Retrieve | 50 min |
| L2 | Golden Sets for Retrieval | 1 · Retrieve | Retrieve | 55 min |
| L3 | Retrieval Metrics with Tiny Numbers | 2 · Judge | Judge | 55 min |
| L4 | Grounding and Faithfulness Judges | 2 · Judge | Judge | 55 min |
| L5 | End-to-End Answer Quality and Honest Refusals | 2 · Judge | Judge | 50 min |
| L6 | Chunking and Index Experiments | 3 · Tune | Tune | 55 min |
| L7 | Freshness, Drift, and Poisoned Sources | 3 · Tune | Tune | 45 min |
| B1 | Bonus: The RAG Eval Checklist | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L2 (a 30-query golden set with relevance labels and a labeling rubric). HW2 after L6 (an experiment report: hypothesis, change, before and after scores, decision).
- **Glossary target:** about 70 terms (chunk, overlap, embedding, hybrid search, reranker, hit, relevance label, golden set, recall@k, precision@k, MRR, nDCG, groundedness, faithfulness, citation check, judge calibration, freshness, re-indexing).
- **Decide before writing:** whether L1 shows one concrete stack in §7 or stays library-neutral; keep metric definitions independent of it.

### Build 11 · Cost and Latency Engineering for LLM Apps

- **Slug:** `cost-and-latency-engineering` · **Level:** Intermediate · **Time:** about 7 h
- **Audience:** engineers who own a model bill or a latency budget. You can read TypeScript and a percentile chart. You have seen a trace, ideally from the observability course.
- **Gap it fills:** Agent Evals L9 ("Improving Cost") is one lesson. Any LLM app, not just an agent, needs profiling, caching, routing, shaping, budgets, and a drill for the next model.
- **Verbs:** **Profile** (where do the tokens and the milliseconds go?) · **Cut** (what removes them without hurting quality you can measure?) · **Budget** (what keeps them from coming back?)
- **World:** Pip's Plant Shop. **Pip** opens the monthly bill in L0. Sprout's traces from the observability course are the raw material. Kofi returns for budgets and alerts. The evals suite from Agent Evals is the quality guardrail for every optimization. No new cast or tools.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | Where the Money and the Milliseconds Go | 0 · Start Here | Profile | 35 min |
| L1 | Profiling from Traces | 1 · Profile | Profile | 50 min |
| L2 | Prompt Caching and Context Hygiene | 2 · Cut | Cut | 55 min |
| L3 | Model Routing and Cascades | 2 · Cut | Cut | 55 min |
| L4 | Reasoning Effort, Output Shaping, and Batching | 2 · Cut | Cut | 50 min |
| L5 | Latency: Streaming, Parallel Tools, Timeouts | 2 · Cut | Cut | 50 min |
| L6 | Budgets, Caps, and Alerts | 3 · Budget | Budget | 45 min |
| L7 | The Upgrade Drill | 3 · Budget | Budget | 45 min |
| B1 | Bonus: The Cost Review Checklist | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L1 (a cost and latency profile of ten Sprout conversations, with the top three spans named). HW2 after L5 (an optimization plan with measured before and after on cost, p95 latency, and eval pass rate).
- **Glossary target:** about 60 terms (input vs output tokens, cache hit, cache prefix, compaction, routing, cascade, reasoning effort, max tokens, batch API, time to first token, streaming, parallel tool calls, per-session cap, cost per conversation, upgrade drill).
- **Decide before writing:** how prices appear in examples (use relative costs, "a third of", so lessons do not go stale when list prices change).

### Build 12 · Synthetic Data and Simulation for Testing

- **Slug:** `synthetic-data-and-simulation` · **Level:** Intermediate · **Time:** about 6 h
- **Audience:** engineers and QA who need test data and test users for a model-backed feature and cannot use production data. You can read TypeScript and JSON. You have written a test.
- **Gap it fills:** Agent Evals L3 ("Synthetic Data and Scenarios") is one lesson. Building a fictional world on purpose, generating personas and scenarios, simulating users, and keeping synthetic data from lying deserve a short course.
- **Verbs:** **Generate** (how do you make data that is fake and still true?) · **Simulate** (how do you run a thousand conversations nobody had?) · **Audit** (how do you know the fake data is not fooling you?)
- **World:** Pip's Plant Shop, whose own world spec (zones, the 30-day return rule, tiers, the cast) becomes the worked example of a fictional world built on purpose. The analyst tools from Agent Evals L3 (`orders_report(range)`) return. New, marked new: **Theo**, the QA engineer who owns the simulation suite and its smoke report.

| # | Lesson | Module | Verb | Time |
|---|---|---|---|---|
| L0 | Why Fake Data Is Necessary and Dangerous | 0 · Start Here | Generate | 35 min |
| L1 | Building a Fictional World on Purpose | 1 · Generate | Generate | 50 min |
| L2 | Personas and Scenario Generation | 1 · Generate | Generate | 55 min |
| L3 | Simulating Users Against Sprout | 2 · Simulate | Simulate | 60 min |
| L4 | Smoke Reports and Coverage | 2 · Simulate | Simulate | 50 min |
| L5 | Keeping Synthetic Data Honest | 3 · Audit | Audit | 50 min |
| L6 | Privacy: Synthetic Instead of Real | 3 · Audit | Audit | 45 min |
| B1 | Bonus: The Synthetic Data Checklist | 4 · Bonus | Bonus | 30 min |

- **Homework:** HW1 after L2 (a 20-scenario set across intents, tiers, and personas, with seeds). HW2 after L4 (a smoke report from a simulation run: coverage matrix, failures, flakes, and what to retire).
- **Glossary target:** about 50 terms (world spec, invariant, deterministic seed, persona, scenario, difficulty ladder, user simulator, stopping rule, coverage matrix, smoke report, flake, distribution check, leakage, de-identification, synthesis).
- **Decide before writing:** whether the user simulator is a model or a script in §7; the lesson should show both and say when each is right.

---

## 4. What every build produces

In this order, per `docs/ADDING_A_COURSE.md`:

1. `docs/courses/<slug>/COURSE_PLAN.md`, written from the brief above with the same sections as the existing plans: big idea, verbs, running example (reused names exact, additions marked new), course map, page standard, lesson-by-lesson plan, code and file shapes, glossary term list.
2. `content/courses/<slug>/course.json`: slug equal to the folder, `level`, `audience`, `order` from the table in §2, `verbs`, `modules`, `shortTitles`, `runningExample`, and `tutorNotes` written as a briefing for Eve.
3. For each lesson: `lessons/<slug>.md`, `quizzes/<slug>.json`, `notes/<slug>.md`, following `docs/AUTHOR_BRIEF.md` (six or more examples, numbered sections, a five-bullet summary, no quiz text in the markdown).
4. `content/courses/<slug>/glossary.json`, with every `keyTerms` id resolving to a term.
5. `npm run validate`, then `npm run content:bundle`, and commit the regenerated `agent/lib/content.generated.ts` (CI checks it is current).
6. Add the course to the "What's inside" table in `README.md`, and move its row from §2 of this file to §1.

**Sizing.** A core lesson is 2,000 to 3,200 words plus a quiz and notes. A nine-lesson course is roughly 25,000 to 30,000 words of lesson text, 60 to 75 quiz questions, two graded homework projects, and 50 to 75 glossary terms. Write the plan before any lesson; the plan is the spec the lessons are derived from.

## 5. Rules that keep the catalog coherent

- **Two worlds, never three.** Engineering and operations courses live in Pip's Plant Shop. Specification and building courses live in Bramble Books. A new character, tool, feature, or corpus is allowed when a lesson needs it, is marked new in the plan's §3, and is reused exactly afterward.
- **Risk tiers are platform vocabulary.** T0 read-only, T1 reversible write, T2 irreversible or money and therefore human-approved. Every course that touches tools uses these words.
- **Evals are the quality guardrail everywhere.** Any course that changes an agent (tools, cost, retrieval) measures the change with the eval suite from Building and Evaluating AI Agents, so students see one method used many times.
- **One home for anything that goes stale.** Prices, regulatory dates, vendor steps, and protocol details each live in one named lesson per course, with an "as of" line, so refreshes are local.
- **Cross-course links go through glossary ids.** A lesson may say "as the security course showed", but its `keyTerms` come only from its own course's glossary.
