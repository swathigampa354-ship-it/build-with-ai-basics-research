# Build With AI: Basics — Hackathon Research Report

**Prepared:** Oct 21, 2026 · **Deadline:** Oct 26, 2026 5:00 pm EDT · **Time remaining:** ~5 days (incl. video + submission buffer)
**Hackathon:** https://learn-ai-basics.devpost.com/ · **Skill Pack:** https://github.com/challengepost/learn-ai-basics
**Evidence convention:** VERIFIED = read directly from GitHub API / raw source files / official Devpost pages fetched today. INFERRED = strong support but not directly confirmed. UNKNOWN = could not be verified. Everything unverified is labelled. No facts were invented.

---

## 1. Official competition requirements (all verified)

### Competition facts
| Item | Value (VERIFIED) |
|---|---|
| Sponsor | Devpost ("Build With AI: Basics") |
| Participation | 4,374 participants · $2,500 prizes ($1,250 / $750 / $500) |
| Submission window | Sep 22 – **Oct 26, 2026 5:00 pm EDT** |
| Judging | Oct 27–30 · Winners announced on/around Nov 4 |
| Eligibility | 18+ (age of majority), most countries; organizers excluded |
| Multiple entries | Allowed but each must be "unique and substantially different" |

### Mandatory deliverables (rules page, VERIFIED)
1. **New project only.** "Newly created during the Submission Period… started from an empty folder. Old ideas are fine, old code is not." Starter frameworks/libraries are allowed but **pre-existing code/work must be disclosed**.
2. **Built with the Devpost Learn Skill Pack** (`npx skills add challengepost/learn-ai-basics --all -y`), evidenced by the planning documents.
3. **Public repo** (GitHub/GitLab/Bitbucket) containing **all source + assets + run instructions**, with an **open-source license detectable in the About section**.
4. Repo must include the Skill Pack's planning docs: **scope.md, prd.md, spec.md**.
5. A **working proof of concept** performing **≥1 function end-to-end**, installable and running consistently on the platform declared in the submission.
6. **Demo video 1–3 min** on YouTube/Vimeo: "Reviewers will not clone or run code, so the video has to show it working." Judges watch up to 3 minutes max.
7. Submission text: name, short description ("what it does, who it's for, what you learned").
8. Two newly added required questions: **how you used the Skill Pack** + **age-of-majority checkbox** (required for prizes).

### Judging (VERIFIED)
- **Stage One (pass/fail):** project "reasonably fits the theme AND was built using the Devpost Learn skill pack, as evidenced by the planning documents in the repository."
- **Stage Two — four equally weighted criteria:**
  1. **Design** — "complete, coherent product experience — not just a technical proof of concept"
  2. **Potential Impact** — "credible, specific… real problem for a real audience — and the solution actually addresses that problem based on what's demonstrated"
  3. **Innovation/Idea** — "creative and novel… differs from existing concepts"
  4. **Presentation** — video shows it working end-to-end; pitch communicates problem/audience/why; easy to follow
- **Judges (all Devpost staff):** Darlyze Calixte (Developer Marketing), Joe Holmes (AI Learning Hackathon Architect), Michelle Brain (Head of PM & Customer Success).

### The Skill Pack contract (all VERIFIED from repo source)
MIT-licensed skill files drive a six-step pipeline; the repo states ~2–4 hours active work ending in "a small working proof of concept — not a product":

| Skill | Output |
|---|---|
| 1-start | `devpost/learner-profile.md` (kept out of commits) |
| 2-scope | `devpost/scope.md` — idea + **Unique Kernel** + Proof-of-Concept boundary + Explicitly Cut list |
| 3-prd | `devpost/prd.md` — screens/behavior/edge cases, "no code talk" |
| 4-spec | `devpost/spec.md` — components, data model, external services, failure modes, how to run/try it |
| 5-build | Sliced build: every slice **mechanically verified & committed**; `checklist.md`; final review; `devpost/app-map.html` |
| 6-ship | **1–3 min video** + **public repo**; submission copy is **learner-written** (agent may only fix spelling/grammar) |

---

## 2. Research Methodology & Search Queries

- GitHub REST API (token provided by the user; **read-only GET requests only**) with ~50 search queries in 11 batches; metadata pulled via `/repos`, `/git/trees`, `/commits`, `/issues`.
- Raw source inspection via `raw.githubusercontent.com` for every shortlisted repo (tree structure, LICENSE text, README claims, package manifests, CI configs, test files).
- Two parallel audit passes on ~22 repos (details §4, §6); build-feasibility check: fresh `create-next-app` scaffold **ran successfully in the local sandbox** (Node v26.4.0, npm 11.21.1; npm registry reachable). **No repo code was executed** anywhere — verification was read-only, as required.
- Web verification of competitor products, licenses, and the State of AI-code-review market (Tessl, CodeRabbit, Qodo, Cursor Bugbot, Stack Overflow 2026 survey figures).

**Search query groups (representative, all run against GitHub search API):**
- Learning/education: `ai tutor`, `socratic tutor`, `misconception learning`, `spaced repetition ai`, `knowledge graph education`, `flashcard generator llm`, `feynman technique app`, `ai worksheet generator`, `read my code check understanding`, `code comprehension check understanding`
- Literature/prompt: `prompt engineering`, `llm playground`, `prompt quality analyzer score`, `prompt anatomy` / `prompt diff` (very thin)
- Critical thinking: `devils advocate`, `steel man argument`, `debate app llm`, `premortem`, `adversarial premortem`, `cognitive bias checker ai` (**0 results**), `critical thinking trainer`
- Spec/verification: `acceptance criteria verify code ai`, `spec conformance check build` (**1 real result, 0★**), `spec drift build vs plan` (**0**), `prd to test cases ai verify` (1), `requirements traceability ai`
- Agent skills: `agent skills SKILL.md`, `claude code skill education`, `agent rules books`
- Components/design/tools: `shadcn ui`, `AI SDK`, design-skill packs, official IBM/Anthropic repos

**Honest scope note:** Devpost's project gallery is not published for this hackathon yet (verified live: "haven't published this gallery yet"), so *what other participants are actually building* could not be observed. Saturation conclusions rely on GitHub/commercial-product analysis instead. Remaining uncertainty is listed in §13.

---

## 3. Top 10 Repositories (with links)

Selected by evidence, not stars; every row audited via API + source read. Full scores in §4.

| # | Repository | ★ | License (VERIFIED) | What it really is | Last pushed |
|---|---|---|---|---|---|
| 1 | [challengepost/learn-ai-basics](https://github.com/challengepost/learn-ai-basics) | 65 | MIT | Mandatory 6-skill curriculum pack (markdown skills only) | active |
| 2 | [bergside/awesome-design-skills](https://github.com/bergside/awesome-design-skills) | 3,282 | MIT | 67 DESIGN.md/SKILL.md design-system packs for agents | 2026-06 |
| 3 | [microsoft/skills](https://github.com/microsoft/skills) | 3,137 | MIT | Official skills/agents/MCP reference patterns | 2026-10 |
| 4 | [responsibleai/ASSERT](https://github.com/responsibleai/ASSERT) | 344 | MIT | Requirement-driven eval harness for AI agents (Python + SvelteKit viewer, 103 test files) | 2026-10 |
| 5 | [ctrlaltwill/LearnKit](https://github.com/ctrlaltwill/LearnKit) | 181 | MIT | Obsidian spaced-repetition plugin (TS, FSRS, 111 test files, CI green) | 2026-10 |
| 6 | [ktaletsk/learn-codebase](https://github.com/ktaletsk/learn-codebase) | 89 | MIT | "Anti-vibe-coding" Socratic codebase-tutor SKILL.md | 2026-10 |
| 7 | [Calibre-Labs/reforge-ai-evals](https://github.com/Calibre-Labs/reforge-ai-evals) | 76 | MIT | Eval-rubric + dataset-writing skills for coding agents | 2026-10 |
| 8 | [artcc/freelingo](https://github.com/artcc/freelingo) | 192 | **AGPL-3.0** | Full self-hosted language-learning platform (FastAPI+Next+Docker+Stripe) | 2026-10 |
| 9 | [delip/autorubric](https://github.com/delip/autorubric) | 62 | MIT (fork-preamble → API says NOASSERTION) | Python library for rubric-based LLM grading (170 test files, CI on 3 OS × 3 Pythons) | 2026-09 |
| 10 | [chevrechou/explainitback](https://github.com/chevrechou/explainitback) | 0 | **NO LICENSE** (beware) | Feynman explain-back app — closest competitor to concept B | 2026-07 |

**Cautionary entry (studied, rejected):** [JushBJJ/Mr.-Ranedeer-AI-Tutor](https://github.com/JushBJJ/Mr.-Ranedeer-AI-Tutor) — 29.5k stars, **no LICENSE file** (default all-rights-reserved), prompt-only files, dormant since 2024. Proof that stars alone mislead.

---

## 4. Repository Comparison & Weighted Scores

**Rubric (60 pts core from the brief, re-based to 100):** Fit with judging criteria 25 · Feasibility in remaining time 20 · Genuine differentiation 20 · Verified code quality/functionality 15 · Design & UX potential 10 · License/dependency/deployment risk 10.

| Repository | Fit (25) | Feas (20) | Diff (20) | Quality (15) | UX (10) | Risk (10) | **Total** |
|---|---|---|---|---|---|---|---|
| challengepost/learn-ai-basics | 25 | 20 | 18 | 10 | 6 | 9 | **88**¹ |
| bergside/awesome-design-skills | 23 | 20 | 16 | 10 | 10 | 9 | **88**¹ |
| microsoft/skills | 21 | 19 | 15 | 13 | 6 | 9 | **83**¹ |
| responsibleai/ASSERT | 20 | 11 | 16 | 15 | 6 | 7 | **75** |
| ctrlaltwill/LearnKit | 18 | 14 | 15 | 14 | 7 | 8 | **76**¹ |
| ktaletsk/learn-codebase | 19 | 19 | 12 | 8 | 5 | 9 | **72** |
| Calibre-Labs/reforge-ai-evals | 19 | 17 | 14 | 8 | 4 | 9 | **71** |
| delip/autorubric | 18 | 10 | 15 | 15 | 4 | 6 | **68**¹ |
| artcc/freelingo | 16 | 5 | 16 | 13 | 7 | 2 | **59**¹ |
| chevrechou/explainitback | 17 | 13 | 8 | 9 | 7 | 1 | **55**¹ |

¹ Relative-grade labels: anything ≥80 is a strong study/reference asset; ≤70 is inspiration-only; AGPL/unlicensed entries are unsuitable as code bases.

**Key evidence behind the top rows**
- **learn-ai-basics:** 8 commits; MIT; 65★; six SKILL.md files confirmed; it IS the required toolchain.
- **awesome-design-skills:** 67 skill packs; each ships `SKILL.md` (tokens/component rules/a11y/quality gates) + `DESIGN.md`; pull with `npx typeui.sh pull <skill>`; MIT. Directly targets the **Design** criterion (25% of the score).
- **microsoft/skills:** 3,137★, MIT, tests + 245 scenario YAMLs; excellent patterns to imitate (not code to build on).
- **ASSERT:** the strongest *open-source verification engine* in the family (PyPI-published, 103 tests) but built for *AI-agent behavior* requirements, heavyweight for a 2–4 hr build.
- **LearnKit:** exemplary engineering (CI runs lint + 111 vitest suites) — a quality bar to emulate.
- **freelingo / explainitback / ThesisDefender:** teach "what NOT to ship" — AGPL entanglement, missing/expired licenses, README↔code drift (details §6).

---

## 5. Licence & Reuse Analysis

| Project | License | Reuse verdict |
|---|---|---|
| learn-ai-basics, microsoft/skills, bergside design skills, LearnKit, ktaletsk, ASSERT, proof-loop (11★), specmine (0★), agent-audits (7★), ThesisDefender, autorubric, hluaguo/learn-faster-kit, ogiberstein/claude-thinking-skills, kirill-markin flashcards | **MIT** | Safe to study/adapt/delete-and-rebuild **with disclosure + attribution in README/NOTICES** |
| shadcn-ui/ui (126k★) | **MIT** | Copy components freely |
| vercel/ai (AI SDK, 27k★) | API says NOASSERTION — tag typically Apache-2.0 in the repo header; **needs a 30-second LICENSE read before committing to it** (not verified in this pass) |
| artcc/freelingo | **AGPL-3.0** | Do **not** copy code unless the app is also AGPL-hosted or style inspiration fine; design inspiration fine |
| JushBJJ/Mr.-Ranedeer-AI-Tutor (29.5k★) | **NONE** | **Unusable.** All rights reserved by default |
| chevrechou/explainitback, jm19pa/Feynman-AI, FeynAI/FeynAI, Gayatri-Demo, badhon252/ChatInsight, Mr-Faizan-Asim/Contrarium | **NONE** | Concept references only — copying their source is legally risky |
| modelcontextprotocol/typescript-sdk | MIT (from earlier verified pass) | Safe |

**Rule adopted:** anything we ship must be (a) built from scratch under an OSI license (MIT), (b) contain only MIT/Apache-2.0/BSD closures, (c) attribute every copied/adapted component in `NOTICES.md`, and (d) disclose pre-existing work per Rule "New Projects Only".

---

## 6. Code Verification & Deployment Findings (anti-"README says so")

**Verified-true examples (green flags):**
- **challengepost/learn-ai-basics:** every README claim matches the source tree.
- **ctrlaltwill/LearnKit:** CI actually runs its 111 vitest files; FSRS + provider list grep-verified in code.
- **delip/autorubric:** 170 test files, CI on ubuntu/macos/windows × Python 3.11/3.12/3.13 — quality is real.
- **responsibleai/ASSERT:** 103 backend tests + 17 YAML regression configs; pip-installable.

**README-vs-source drift caught (i.e. "don't trust READMEs"):**
- **SreeNaresh1/ThesisDefender (0★):** README says "exactly 3 LLM calls per analysis" — source (`backend/agents/pipeline.py`) makes **4** sequential calls; "Foundry IQ retrieval" is env-var-only scaffolding; zero CI.
- **hluaguo/learn-faster-kit (391★, MIT):** says "4 learning modes" — source ships **5** (undocumented `programming` mode); AGENTS.md meanwhile says "no committed test suite" although `tests/` exists; README homepage points at a different fork (`cheukyin175`) — provenance unclear.
- **FeynAI/FeynAI:** "SQLite → PostgreSQL" claim: not implemented; leaderboard claimed as roadmap but partially shipped; unchanged since Dec 2024.
- **Gayatri-Education/Gayatri-Demo ("institutional-grade offline adaptive AI platform", 15★):** source contains **no LLM/network code at all** — "AI" is a keyword-match template engine; prompt bodies are XOR-obfuscated; intentional anti-RE obfuscation. "AI-powered" is marketing.
- **JushBJJ/Mr.-Ranedeer-AI-Tutor:** 29.5k★ prompt-only, no code, no license.
- **badhod252/ChatInsight (2★):** builds the OpenAI key into the browser bundle.
- **artcc/freelingo:** Docker+Postgres+Redis+Stripe weight; AGPL; study-only in our timeframe.

**Build feasibility (local sandbox test):** fresh `create-next-app` scaffold completed successfully today; Node v26 / npm 11 are healthy. **No repo code was executed.**

---

## 7. Competition Saturation & Differentiation

**Measured saturation (GitHub search totals):**
| Theme | Results | Verdict |
|---|---|---|
| `ai tutor` / `socratic tutor` | 29,329 / 634 | Red ocean — 3 out of 4 entries is another AI tutor |
| `flashcard + spaced repetition ai` | 1,357+ | Saturated |
| `prompt engineering` (repos) | 25,418 | Saturated |
| `feynman technique app` | 23 | Thin but non-zero (Chevrechou/FeynAI demos) |
| `ai literacy assessment quiz` | **0 on GitHub, 7 commercial SaaSes** (JobCannon, Iternal, Iro AI, AI CRED, AILAT, Prompt Guide, comcreate) | Commercially cluttered — clone risk on "Innovation" |
| `cognitive bias checker ai` | **0** | Truly white |
| `devils advocate` / `premortem` | 3 / 5 | Very thin (max 4★) |
| `spec conformance check build` | 1 real repo (specmine, 0★) | Emerging-but-thin |
| `spec drift build vs plan` | **0** | Empty |
| `acceptance criteria verify code ai` | 8 (proof-loop 11★, agent-audits 7★, cadence 2★ — all agent-CLI gates, none is a **user-facing web app**) | Emerging-but-thin |
| `requirements traceability ai` | 261 but mostly enterprise/linting |

**Market context (verified third-party articles):** CodeRabbit/Qodo/Bugbot all review **diffs and code quality only** — explicitly "cannot verify code against external specs or business requirements" (Qodo's own comparison); Tessl (closed-beta enterprise framework) acknowledges the "validation gap": users say specs get moved away from. Stack Overflow 2026 figures: **76% run AI-generated code locally**, ~10% use it without verification — i.e., "verification debt" is a recognized, currently-unnamed-by-great-tools problem.

**Gap identified:** there is a market of "plan before you build" skill packs (including the mandatory Skill Pack itself) that ends at build. **Nothing on GitHub — and no commercial product — gives a non-expert a user-facing app that answers: "does what I built actually match what I promised?"**

---

## 8. Three Possible Concepts

### Concept A — **"Spec Sentinel"** (recommended)
> *"Did the AI actually build what you asked for?"*

- **Problem/audience:** beginners & experienced builders using AI coding agents (this hackathon's 4,374 participants). Stack Overflow: only ~10% don't verify; their pain: no way to know if the code their agent shipped *matches the plan they wrote*. This hackathon literally produces scope.md/prd.md/spec.md — files that then get ignored.
- **Kernel (one clear mechanic):** paste/upload a PRD (acceptance criteria) + point at the project folder → return an **evidence-backed conformance report per criterion: MET / PARTIAL / MISSING / UNVERIFIABLE, each with file:line evidence quotes and a suggested fix.**
- **Why it wins with these judges:** Stage One "matches the theme" is real: it IS the plan-before-build mindset turned into a product; Joe Holmes (AI Learning Architect) owns curriculum-value; Michelle Brain owns process discipline; Darlyze wants a crisp demo story.
- **Demo reliability:** the tool ships with a bundled "sample project" whose PRD intentionally contains 7 criteria, 1 of which is *planted-missing* — the live report shows terminal-state evidence of the catch and the fix.
- **Design surfaces:** upload/paste → criteria checklist → interactive report (score ring, per-criterion cards, copyable fix list).

### Concept B — **"Explain-Your-Build" Feynman gate** *(sorry, this overlaps concepts B and D)*
> "Don't just let an app generator rob your understanding — prove you can explain your own build."

- **Mechanic:** the agent takes the built project's `app-map.html` / code and interviews you about what each slice does; wrong answers produce *targeted* micro-lessons instead of generic hints.
- **Why it's plausible:** Study-mate LearnKit/Freelingo target knowledge work; the gap is *self-assessment after an AI build* specifically in this skill-pack format (app-map.html exists in the skill pack).
- **Risk:** conceptually adjacent to the existing AI apps we found (mostly inactive, but still "a telling tool exists"); the "complete product experience" and demo wow-factor is lower than Concept A; harder to make genuinely novel mechanics.

### Concept C — **"Premortem Forge" / Idea Stress Tester** (devil's-advocate niche)
> "Your idea is a flag. The tool attacks it structurally."

- **Mechanic:** user types an idea/project; tool runs a fixed **adversarial framework** (assumption ledger, failure mode grid, second-order effects) producing a structured attack brief + severity-ranked "kill-list" of risks and one "survivor plan". 5 days of low-cost work.
- **Why NOT primary:** "Audience/Impact" muddier (who exactly is the user?), pitch could look like a chatbot ("the thing judges are sick of"), and "adversarial" features are already available inside commercial Gemini/ChatGPT workflows.

### Why Concept A is the top pick
1. It is the *only* idea that is simultaneously **novel vs existing repos**, **thematically auto-wired to this hackathon's own pedagogy** (uses scope.md/prd.md/spec.md — the *same* files Stage One checks for), and **trivially demonstrable in 90 seconds** ("We promised 7 things. Here are 7 verdicts. Here is the line of code that betrayed one of them.").
2. It has a **credible, specific, proven problem** (the "verification debt" article; SO 2026 stat; Qodo's admission that existing tools can't check specs).
3. The 2–4-hour skill-pack size is satisfied by the report feature; anything else becomes a "complete product experience" via a second screen only (fix list + score), not a sprawling product.

---

## 9. Final Recommendation — **"Spec Sentinel"** (working name)

**One-line pitch for the video (3s):** "You wrote a plan. Your AI built *something*. Did it build the thing you promised?"

**Approach:** build from scratch with an agent, ship a local web app + sample project, record a 90-second video, publish public repo with MIT license + full Notifier list.

---

## 10. Minimal Feature Scope & Architecture

**MVP (4 user-visible slices, each independently demoable and shaped like a complete product experience):**
1. **Ingest** — paste PRD (or auto-load `devpost/prd.md` when run from a repo); LLM extracts numbered acceptance criteria; user-adjustable checklist.
2. **Scan** — user points at a project folder (server reads local files; Node.js `fs`) OR clicks "Load Sample Project."
3. **Judge** — per criterion: deterministic file search (grep / keyword map) + one LLM call per criterion returning structured `{verdict, evidence_quote, file_line, one_line_fix}`.
4. **Report + Fix list** — score ring + criterion cards; copyable "gaps to fix before you ship."

**Early-stopping rules (what we will NOT build):** git integration, multi-language AST parsing, teammate roles, login, DB, SaaS deploy, re-verification loops, PDF export. Deployment: **optional** (rules); a local `npm run dev` is enough; a free-tier hosted demo is a *nice-to-have* if time remains.

**Stack (as VERIFIED as of now):** Next.js 14+ (create-next-app scaffold tested today), TypeScript, Tailwind + **shadcn/ui** (MIT, verified), **Vercel AI SDK** (license to be re-read before commit; if not Apache-2.0 in the header, fall back to plain `fetch` to provider APIs), any of Gemini/OpenAI/OpenRouter behind one env var called `LLM_API_KEY` + `MODEL`; **no database**; `.env.example` shipped with placeholders only; devpost/ files + `.env` never committed.

**Design references to lift the Design score:** bergside/awesome-design-skills typography/spacing tokens patterns; microsoft/skills for skill-file structure.

---

## 11. Estimated Time & Deployment Cost

| Item | Estimate | Notes |
|---|---|---|
| Skill-pack enforcement (scope→spec) | 2–4 hrs | mandated, non-negotiable, and it genuinely sharpens the scope |
| Build slices 1–4 + fix list | 14–18 agent-assisted hrs over 4–5 evenings | feasibility test today showed the toolchain works |
| Bundle sample project + planted miss | 2 hrs | makes the video deterministic |
| Record + edit 1–3 min video | 2–3 hrs | practices explicit in the rules |
| Slack for README, disclosure, submission form | 2 hrs | disclosure+Skill Pack question are **required** |
| **Total** | **~22–30 focused hrs** | comfortably inside Oct 26 5pm EDT |
| Cost | **$0** | Gemini free tier / OpenAI; Vercel Hobby; no paid subscriptions |

---

## 12. Compliance Checklist (submission readiness)

- [ ] Register + age checkbox on Devpost before Oct 26 5:00 pm EDT
- [ ] Describe in scope.md which Skill Pack skills we ran (new required question)
- [ ] Empty-folder start, original work, **disclosure of any scaffold/README/info referenced** (e.g., shadcn/ui, ai SDK mention)
- [ ] scope.md, prd.md, spec.md committed (and not git-ignored); learner-profile.md NOT committed
- [ ] Public repo, OSI license visible in About (MIT), all secrets in `.env` (not committed); `.env.example` only
- [ ] Video 1–3 min, on YouTube (public/unlisted-but-public link), shows the kernel working end-to-end on the target platform, no third-party trademarks/music
- [ ] Text description covers **what it does / who it's for / what you learned** — learner-written, agent only fixes spelling/grammar
- [ ] "Try it out" link must point to the public repo (this is the eligibility field for the Skill Pack check)

---

## 13. Sources & Remaining Uncertainties

**Verified sources:**
- Devpost pages: main (incl. judging criteria panel), /rules, /details/dates, /updates (fetches via search cache today)
- challengepost/learn-ai-basics (repo, 6 SKILL.md files read)
- GitHub API + raw source for every listed repository; license files read directly
- Third-party market articles: qodo.ai comparison, hackernoon SDD tools, onout/vibers "12 Best AI Code Review Tools", Tessl blog, Dev.to AI-code-review guide, ainave/hackathon-radar mirrors

**Remaining uncertainties (honest):**
1. **What other participants are building** — Devpost's gallery for this hackathon is not public yet (checked today). Collision risk on the concept exists; mitigated by the sharp meta-fit of the skill-pack files as inputs and the 90-second planted-miss demo, and by Concept B/C being distinct fallbacks.
2. Judging is subjective; rubric scores in §4 are estimates, not predictions.
3. `vercel/ai` license detail (API says NOASSERTION; needs a 30-second LICENSE read before using it in the build).
4. LLM-judged code conformance is inherently probabilistic — mitigated by "evidence + conservative verdicts" design, but not a guarantee.
5. The Devpost judges' blind spots / taste — unknowable.
6. We did not clone-and-build any listed repo, exactly as instructed (read-only verification).

---

## Shortlist — Top 3

1. **Spec Sentinel** (Concept A) — build this. Novel *and* demo-robust, on-theme with the "plan-before-you-build" Skill Pack itself, and backed by a documented industry gap (only ~10% of devs trust AI output without verification; CodeRabbit/Qodo explicitly review diffs, not specs vs. implementation).
2. **"Explain-Your-Build" Feynman gate** (Concept B) — easiest to nail "coherent product experience"; warm, teaching-oriented; slightly less novel because close cousins already exist.
3. **Premortem Forge** (Concept C) — genuinely under-saturated (`devils advocate` has 3 repos), totally feasible; weakest "real audience/impact" story and a high risk of looking like "just another chatbot".
