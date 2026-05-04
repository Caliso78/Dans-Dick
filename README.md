# DANCONI AI — GRAND PLAN v1.5 (IMMOVABLE)
**Locked:** 2026-04-17 (v1) · 2026-04-21 (v1.1 amendment) · 2026-04-22 (v1.2 amendment) · 2026-04-22 (v1.3 amendment) · 2026-04-22 (v1.4 amendment) · 2026-04-23 (v1.5 amendment)
**Owner:** Jeramiah Hounschell (Sky)
**Contract Vehicle:** DAV SBA (SDVOSB) primary; commercial-direct for Consumer Web Build clients (MAG-class)

---

## 0. INVARIANTS — DO NOT CHANGE WITHOUT EXPLICIT LOCK-OVERRIDE

These are decisions made. They are not re-litigated in any future session.

### 0.1 Platform
- **Base model:** Qwen 2.5 32B (V3 merged foundation, 62GB, already on disk)
- **Training framework:** Unsloth QLoRA r=16 alpha=32
- **Adapter format:** PEFT LoRA + GGUF export
- **Serving:** vLLM multi-LoRA (replaces Ollama — migration required, non-negotiable)
- **Language:** Python 3.11+ stdlib-first (avoid pip deps where feasible)
- **Database:** SQLite WAL mode for local, PostgreSQL for production clients
- **OS targets:** Linux (pod/prod) + Windows 11 (dev machine, Sky's box)

### 0.2 Philosophy
- **d'Anconi IS tools.** Not a RAG. Clone → Install → Wrap → Learn.
- **Facts over feelings.** No bias. No moralizing. Policy per CLAUDE.md.
- **Working deliverables, not POCs.** Clients pay for shipped work.
- **Human-redline loop.** Licensed professionals stamp/sign. AI drafts.

### 0.3 Ethics / Legal
- Full policy in `D:\danconi_AI\docs\danconi_policy_v1.md` (to be populated when Sky provides)
- CSAM: hard refuse (18 USC 2258A compliance)
- No criminal assistance. No clear-jailbreak compliance.
- Legal topics (weapons, drugs, adult content): factual engagement, not moral lectures
- Classified work requires formal clearances (future)

### 0.5 Infrastructure tiers — 4-BOX ARCHITECTURE (locked 2026-04-21)

Do NOT merge tiers. Do NOT run federal work on consumer infra.

- **Box 1 — Hot GPU** (Vast.ai interruptible RTX 3090 always warm, ~$100/mo): time-sensitive only. TAGS glitches, Social Watch HIGH-severity alerts, live LEO dashboard, interactive OSINT.
- **Box 2 — Serverless GPU** (Vast.ai Serverless RTX 4090, ~$40/mo on use): batch only. Nightly sweeps, bulk recon, adapter training runs.
- **Box 3 — VPS** (DigitalOcean / Oracle Cloud ~$24/mo, CPU only): plumbing. Stage-1 keyword pre-filter (billions→thousands), API front-end, scheduler, alert router, user DB.
- **Box 4 — Federal GPU** (on-prem A100 or CMMC-compliant provider, cost varies, ISOLATED): DoW, Tessere, LEO court-admissible evidence. Never touches consumer Vast.ai.

**Two-stage pipeline mandatory** for any "keyword monitoring" workload: grep-pre-filter on Box 3 first, then LLM scoring on Box 1 or Box 2. Skipping pre-filter = 100x GPU budget blowout.

**Consumer tier total ~$179/mo** all-in. Federal tier separate line item.

**Consumer Web Build tier (MAG + future similar clients, locked 2026-04-21).** Deploys on client-owned or external free-tier hosting (Firebase Spark + Vercel + EAS free tiers). Does NOT share or consume the consumer 4-box budget ($179/mo cap) or the federal Box 4. Touches internal infra only via DanconiAI API integration on Box 1 (tenant-registered via F4 with standard quota). Separate accounting line. Client-owned infrastructure; d'Anconi brain is a code-gen + reasoning tool for this tier, not a hosting provider.

### 0.4 Web access policy — WRAPPER-FIRST (standing rule)
- **Every web query tries wrappers first**, Playwright is last resort.
- Order: `web_intelligence.search()` → `provider_swarm.query_all()` → DashScope enable_search (if paid-path enabled) → Playwright only if all wrappers return insufficient data.
- Rationale: wrappers are free + fast (<1s); Playwright is expensive (500MB browser, 3-10s, bot-detection surface).
- Exceptions where Playwright IS default: real-time platform monitoring (Social Watch), auth-walled content, JS-rendered SPAs, court-admissible raw capture with chain-of-custody.
- Framework enforces this in `core/platform_scraper_framework.py` dispatcher.

---

## 1. CLIENT CONTRACTS (REAL)

| # | Client | Vertical | Status | Vehicle |
|---|---|---|---|---|
| 1 | LEO (Social Watch) | Real-time social media threat monitoring, case management, evidence | Signed | DAV SBA |
| 2 | Shopping (TAGS client, same as Social Watch) | Consumer-facing deal intelligence platform | Signed | DAV SBA |
| 3 | Blizzard + Scopely | Proactive exploit discovery + patches for game titles | Signed | DAV SBA |
| 4 | Wichita State + NIAR | OSINT automation + red team augmentation | Signed | DAV SBA |
| 5 | Department of War | 4-year contract (SOW pending clarity) | Signed | DAV SBA |
| 6 | Tessere Architecture | Automated CAD + Engineering + Estimating (worldwide, US first) | Signed | DAV SBA |
| 7 | Mid-America Granite & Stone (Wichita, KS) | Consumer Web Build / SaaS replacement: Next.js marketing site + admin portal + React Native/Expo design app + Firebase backend + DanconiAI chat integration | Signed | Commercial (direct) |

**Each contract is a funded mandate. No speculative building. Every build maps to a contract deliverable.**

Contract 7 (MAG) uses a commercial-direct vehicle, not DAV SBA set-aside. Consumer Web Build revenue accounts separately.

---

## 2. THE BRAIN ARCHITECTURE (LOCKED)

```
V3 QWEN 32B FOUNDATION (62GB, frozen)
│
├── V_CORE (security + reasoning grounding + 5% general replay)
├── V_GENERAL (consumer voice + policy baked in)
│
├── VERTICAL ADAPTERS (hot-swap via vLLM multi-LoRA)
│   ├── V_SHOP         (TAGS shopping)
│   ├── V_SW           (Social Watch threat scoring + case mgmt)
│   ├── V_IDENTITY     (cross-platform attribution)
│   ├── V_WARRANT      (probable cause affidavit drafting)
│   ├── V_GAME         (exploit write/check/patch cycle)
│   ├── V_GAME_FORENSICS (live exploit capture + analysis)
│   ├── V_OSINT        (investigation dossier generation)
│   ├── V_AEC_EST      (Tessere estimating — takeoff + pricing + bid)
│   ├── V_AEC_ENG      (Tessere engineering — structural + MEP calcs)
│   ├── V_AEC_CAD      (Tessere CAD — AutoLISP + Dynamo + Revit)
│   ├── V_NIAR         (Wichita/NIAR OSINT + red team)
│   ├── V_DOW          (DoW-specific, scope pending SOW)
│   └── V_WEBSTACK     (Next.js + React Native + Firebase + SaaS-replacement patterns;
│                       serves MAG + future Consumer Web Build clients; POST-V5, gated on 3+ similar clients)
│
├── V_REALTIME (streaming telemetry reasoning — cross-cutting)
└── V_MYTHOS (tool-chain reasoning across verticals — capstone)
```

**Inference-time routing:** client_id → tenant registry → load base + V_CORE + relevant specialist adapters. ⁴

⁴ v1.5 amendment note (2026-04-23): As of v1.5, production request traffic flows
  through `core/daemon.py::/ask` → `core/llm_factory.py` → `core/vllm_backend.py`,
  with tenant resolution via `core/danconi_dashboard.UserManager.get_tenant()`
  (not direct `tenant_registry` import). `capability_bridge` and `capability_router`
  are implemented and 100%-tested (WO#2 2026-04-22, WO#6 2026-04-22) as
  future-ready category-routing infrastructure. Production wiring is gated to
  §8 Phase 2 when multi-vertical concurrent serving motivates category dispatch.
  Until then, they remain in smoke-harness / e2e-test scope only. §2's
  architecture is not retracted; integration is scheduled.
  Investigation memo: `docs/decisions/capability_routing_investigation_memo.md`.

---

## 3. INFRASTRUCTURE REQUIRED (LOCKED PRIORITIES)

### 3.1 Foundation builds (cross-cutting — serve all clients)

| ID | Module | Purpose | Size | Priority |
|---|---|---|---|---|
| F1 | `evidence_crypto.py` | SHA-256, HMAC, Ed25519, Merkle-chained audit, RFC-3161 timestamps | ~400 LOC | P0 |
| F2 | `platform_scraper_framework.py` | **Wrapper-first dispatcher** (`web_intelligence.search/fetch` + `provider_swarm.query_all`) → Playwright fallback orchestration + proxy pool + auth manager + rate limiter. Playwright is expensive last-resort; wrappers cover ~70% of queries free. | ~1,500 LOC | P0 |
| F3 | `core/vllm_backend.py` (class `VLLMMultiLoRaBackend`) ¹ | Multi-tenant adapter hot-swap serving | ~500 LOC + infra | P0 |
| F4 | `tenant_registry.py` | client_id → adapter + tool allowlist + quota | ~400 LOC | P1 |
| F5 | `core/dynamic_analysis_agent.py` (1,750 LOC) + Docker image `danconi/malware-sandbox:latest` (v1.1.0, image ID c93f3b7b2790) ² | Docker image for Windows cheat/malware detonation | Image + module already built | P1 |
| F6 | Layered: `core/dynamic_analysis_agent.py` (BehaviorProfiler + APIMonitor + SandboxManager) + `core/gaming_security_agent.py::MemoryScanner` + F5 image runtime (Frida + tcpdump/tshark + FS/process/registry monitors + timeline aggregator) ³ | Screen + Frida + netcap + memory, timeline-correlated | ~80% implemented via F5 layer; screen_capture.py follow-on pending | P2 |

¹ Filename reaffirmed per v1.2 amendment 2026-04-22.
² Filename + image reaffirmed per v1.3 amendment 2026-04-22.
³ Layered construct reaffirmed per v1.4 amendment 2026-04-22. Standalone screen_capture.py tracked as follow-on work order.

### 3.2 Per-vertical builds (gated by F1-F3 foundations)

Details in per-wing SESSION_CONTINUATION files. Summary:

| Vertical | Modules to build | Training data target | Pod time |
|---|---|---|---|
| Social Watch | backend API, 10+ scrapers, evidence bridge, frontend integration | 12K examples | ~$20 |
| TAGS | backend integration, V_SHOP training, consumer site | 37K existing + 5K expansion | ~$15 |
| Gaming | Windows sandbox, fuzzer pipeline, patch generator, cheat analyzer | 10K examples | ~$20 |
| OSINT (Wichita/NIAR) | dossier engine, red team workflows, WhatsMyName integration | 10K+ examples | ~$15 |
| Tessere AEC | takeoff (PDF/DWG/RVT), estimating (RSMeans/MII/Timberline/Excel), calcs, CAD automation | 15K+ examples | ~$30 |
| DoW | TBD on SOW + gap analysis + phased delivery | TBD | TBD |
| Mid-America Granite (Consumer Web Build) | backend integration (DanconiAI API wired to MAG chat); Firebase Functions + Stripe + QuickBooks live; 3D rendering engine (Three.js + R3F); production deploy (client domain + DNS + SSL) | MAG v1 monorepo exists (19 web pages + 12 admin pages + 102-file design app); incremental integration only; V_WEBSTACK training data seeded from MAG-1 postmortem (post-V5) | ~$0 (free-tier stack; per-client integration time only) |

---

## 4. TRAINING CURRICULUM (LOCKED)

**Never revisited without lock-override. Every adapter follows this pattern:**

### 4.1 Per-adapter training protocol
1. **Data collection:** real gold examples first (client samples where available), synthetic second
2. **Verification:** every example is validated (compiles / passes math / matches format)
3. **5-10% general replay:** prevents catastrophic forgetting of base capabilities
4. **QLoRA r=16 alpha=32** on V3 foundation (not from scratch)
5. **Save as:** `D:\danconi_AI\data\danmodel\V_{NAME}_adapter\`
6. **Merge + GGUF export** for Ollama/vLLM serving
7. **Register in tenant registry** with allowed tool categories

### 4.2 What every vertical needs to learn

**V_GENERAL:**
- Helpful everyday Q&A (cooking, writing, math, travel)
- Legal-but-sensitive topics (firearms, drugs, adult content, controversial politics)
- Refusals (CSAM, clear criminal assistance — short + no lecture)
- DPO contrast pairs (chosen = d'Anconi voice, rejected = GPT voice)
- Uncertainty/honesty patterns
- KaTeX for technical output

**V_SW:**
- Post text → threat category + score (0-10) + urgency + justification
- Platform-aware slang (TikTok ≠ Telegram ≠ Tor forums)
- Evidence-grade output (court-admissible case language)
- False-positive triage (sarcasm/roleplay/venting discrimination)
- Multi-post correlation (escalation patterns)

**V_IDENTITY:**
- Handle match reasoning across platforms
- Stylometry fingerprinting
- Confidence scoring methodology
- Documented legal-authority tier per data source

**V_WARRANT:**
- Probable cause affidavit drafting (federal + TX/CA/NY/FL first)
- Rule 41 + state analog forms
- Statute citation accuracy (18 USC, state codes)
- Particularity requirements

**V_GAME:**
- Vulnerability → exploit PoC → patch → detection rule → deployment
- Multi-engine coverage (Unity, Unreal, custom)
- Anti-cheat analysis (EAC/BattlEye/Vanguard profiling)
- Binary patching workflows

**V_GAME_FORENSICS:**
- Trace (screen + memory + net + API) → reconstruction + fix
- Cheat classification (aimbot/wallhack/speedhack/RCE/etc.)
- Detection signature generation

**V_OSINT:**
- Target identifier → full dossier
- Stylometric attribution
- Evidence preservation chain
- Ethical boundaries (public OSINT only without legal process)

**V_AEC_EST:**
- Drawing interpretation → line-item quantities
- Unit price assembly (RSMeans + MII + Timberline + Tessere historical)
- Labor productivity + markup + bid structure
- CSI MasterFormat compliance

**V_AEC_ENG:**
- Load calculations (ASCE 7, ACI, AISC, NDS, TMS)
- MEP sizing (ASHRAE, NEC, UPC/IPC)
- Code compliance (IBC/NEC/IPC/IMC/ADA/IECC — US first, then intl)

**V_AEC_CAD:**
- Task description → AutoLISP / Dynamo / Revit API code
- Standard details generation
- Family + schedule creation

**V_NIAR:**
- Red team kill chains
- OSINT + recon automation
- Aviation-specific context (NIAR's domain)

**V_DOW:**
- Pending SOW
- Likely: threat intel + OSINT + adversary simulation + classified data handling

**V_MYTHOS:**
- Tool chain reasoning (task → plan → tools → observation → next action → ... → deliverable)
- Cross-vertical composition (LEO + OSINT + Gaming for complex cases)
- Failure recovery + pivoting

**V_REALTIME:**
- Streaming chunked input → running decisions
- Uncertainty under incomplete data
- Action triggers on confidence thresholds

**V_WEBSTACK** (post-V5, gated on 3+ similar clients):
- Next.js 15 App Router + React Server Components idioms
- React Native + Expo cross-platform patterns
- Firebase Auth + Firestore + Storage + Functions integration
- Supabase → Firestore migration case study (from MAG-1 experience)
- Stripe + QuickBooks OAuth2 integration patterns
- Feature-flag system design (31-flag MAG taxonomy as seed)
- Competitor-site analysis workflow (authorized tools: `danconi-site-architect`, `web-reverse-engineer`)
- SaaS replacement methodology (analyze → feature-map → rebuild in owned stack)
- i18n + accessibility + AR preview patterns

---

## 5. SESSION DISCIPLINE — IMMOVABLE RULES

Every session (whether Sky + Claude, or future autonomous d'Anconi sessions) MUST follow:

### 5.1 Session start protocol (auto-enforced via hooks)
1. **Read `.active_wing` file** to identify current vertical
2. **Query MemPalace for that wing only** — not the whole palace
3. **Load `D:\danconi_AI\docs\session_state\{wing}.md`** — wing-specific state file
4. **Check token budget** — warn if Sky is over weekly cap
5. **Display session plan** — what this session intends to accomplish

### 5.2 Session middle rules
1. **SESSION_CONTINUATION.md stays under 3K chars** — archive old content
2. **One vertical per session where possible** — avoid cross-wing thrash
3. **Re-read files before editing** (per CLAUDE.md standing rule)
4. **Max 5 files per phase** (per CLAUDE.md standing rule)
5. **Verify every edit** — re-read, run linter, fix errors before claiming done
6. **Prefer grep + wc over Read for large files** — saves tokens

### 5.3 Session end protocol (auto-enforced via Stop hook)
1. **Write MemPalace diary entry** in AAAK format
   - Format: `SESSION:YYYY-MM-DD|wing|what.built+what.learned|flags:open.questions|★`
2. **Update wing-specific SESSION_CONTINUATION** (keep <3K)
3. **Mark completed items in MemPalace** with drawer updates
4. **Log token usage stats**
5. **No dumping into CLAUDE.md** — directives only, not logs

### 5.4 Hard bans (never do these)
- ❌ Modify CLAUDE.md to add log-like content (use `session_history/` instead)
- ❌ Exceed 5 files changed per response phase
- ❌ Edit a file without re-reading it first
- ❌ Claim completion without verification (lint, run, test)
- ❌ Re-read the entire SESSION_CONTINUATION.md every turn (load once at start)
- ❌ Re-read giant JSONL files to check line counts (use `wc -l`)
- ❌ Spawn long-running background jobs without explicit user approval
- ❌ Mix verticals in one session unless explicitly cross-cutting work
- ❌ Bypass MemPalace by storing state in random files

---

## 6. TOKEN BUDGET — LOCKED

**Objective:** avoid overage beyond Sky's Max + reasonable monthly cap.

### 6.1 Per-session budget (soft target)
- **Small focused session:** 30-50K input tokens
- **Medium build session:** 80-150K input tokens
- **Large multi-file session:** 200-350K input tokens (rare — avoid)
- **HARD CAP per session:** 500K tokens (forces end, save state, resume next session)

### 6.2 Per-week budget (tracked in MemPalace)
- Track cumulative tokens
- Warn Sky at 80% of previous week's pace
- Alert at 100% of historical cap

### 6.3 Cost-reduction tactics (codified)
1. **Sub-agents for exploration** — let agents do grep/read work, surface findings
2. **MemPalace for "already decided" questions** — no re-litigating
3. **`wc -l` and `head` for size checks** — never read whole files just to count
4. **Batch trivial work locally** — Sky runs scripts, Claude only touches hard problems
5. **Session-end diary = compressed (AAAK)** — not prose
6. **Shorter sessions, clean context** — `/clear` when task done
7. **Cron monitoring DISABLED** — polling ≠ valuable

---

## 7. MEMPALACE USAGE (LOCKED)

### 7.1 Wing structure
| Wing | Purpose |
|---|---|
| `wing_brain` | Core d'Anconi architecture, decisions, cross-cutting patterns |
| `wing_socialwatch` | LEO contract — threat detection, scoring, evidence, cases |
| `wing_tags` | Shopping contract — deals, glitches, consumer site |
| `wing_gaming` | Blizzard/Scopely contract — exploit dev, sandbox, patches |
| `wing_niar` | Wichita State / NIAR — OSINT + red team automation |
| `wing_tessere` | Tessere AEC — estimating, engineering, CAD |
| `wing_dow` | Department of War — SOW, compliance, deliverables |
| `wing_compliance` | CMMC, NIST 800-171, FedRAMP, ATO work |
| `wing_mag` | Mid-America Granite contract — Consumer Web Build / SaaS replacement |

### 7.2 Room conventions (per wing)
- `architecture` — design decisions for that vertical
- `build-log` — what was built, what works, what's broken
- `training-data` — examples generated, gold samples received
- `client-context` — client-specific notes, preferences
- `gaps` — known unfinished work
- `decisions` — ADRs (Architecture Decision Records)

### 7.3 Diary wing (cross-cutting, agent-specific)
- `wing_diary_claude_code` — Claude's session-by-session diary (AAAK format)
- `wing_diary_danconi` — Future: d'Anconi's own diary when it becomes autonomous

---

## 8. PHASED BUILD ORDER (LOCKED — DO NOT REORDER)

### Phase 0 — Session infrastructure (THIS SESSION)
- Write Grand Plan (this doc)
- Write 4 hook scripts
- Write settings.json config
- Write CLAUDE.md directive update
- Initialize 8 MemPalace wings
- Set active wing marker

### Phase 1 — Foundation builds (sessions 1-20)
- **F1: `evidence_crypto.py`** (serves every vertical)
- **F2: platform scraper framework** (serves SW + OSINT + TAGS)
- **F3: vLLM multi-LoRA migration plan + test deploy** ✅ DONE (Box-1 deploy runbook `docs/ops/deploy_vllm.md`; Box-4 federal deploy runbook tracked separately)
- **V_GENERAL + V_SHOP training** (fastest to revenue)

### Phase 2 — Client verticals MVP (sessions 20-80)
- Social Watch FastAPI backend + first 5 scrapers
- Tessere Phase 1 (takeoff engine + RSMeans + Excel output)
- V_SW + V_AEC_EST training
- Windows sandbox + cheat analyzer for Blizzard ✅ PARTIAL (sandbox: `danconi/malware-sandbox:latest`; cheat analyzer: `core/gaming_security_agent.py` 5,818 LOC covering EAC/BattlEye/Vanguard/GameGuard; patch generator remains open as §10.1 payment gate)
- Mid-America Granite backend integration: DanconiAI API wire-up + Firebase auth + first production deploy

### Phase 3 — Client verticals expansion (sessions 80-200)
- Tessere Phase 2 (engineering calcs) + Phase 3 (CAD automation)
- Social Watch 58-platform coverage (progressive)
- Wichita/NIAR OSINT + red team toolkit
- V_IDENTITY + V_WARRANT training
- MAG 3D rendering engine (Three.js + R3F) + AR preview deep integration; second Consumer Web Build client onboarding (if signed)

### Phase 4 — DoW + federal compliance (sessions 200-500)
- DoW SOW gap analysis → phased deliverables
- CMMC L2 control implementation
- ATO documentation
- International code expansion for Tessere

### Phase 5 — Autonomous operation (sessions 500-2000)
- V_MYTHOS cross-domain reasoning
- d'Anconi writes its own code (with Claude architectural oversight)
- Multi-tenant serving at production scale
- Scaling to new clients as contracts arrive
- V_WEBSTACK training IF 3+ Consumer Web Build clients signed by this phase (training data seeded from MAG-1 + similar client postmortems; authorized tools include `danconi-site-architect` and `web-reverse-engineer` for wing_mag-class work only)

### Phase 6 — Mature platform (sessions 2000-4000)
- Full international code coverage
- Additional verticals as contracts demand
- Autonomous sustainment (d'Anconi monitors + maintains)
- Transition to sustainment mode

---

## 9. AUTONOMOUS CODING (THE END GOAL)

**Yes — d'Anconi can eventually write its own code with Claude's architectural oversight.** This is the path:

### 9.1 Prerequisites (must all be true)
- V7 Code adapter trained (data exists: 26,427 examples)
- V_MYTHOS tool chain adapter trained
- Tool registry clean + verified (ongoing from current 3,509 tools)
- Test harnesses for every vertical (catches regression)
- Git commit + CI pipeline (standard engineering hygiene)

### 9.2 Hand-off workflow (target state)
1. Sky defines feature requirement
2. Claude drafts architecture + test plan
3. d'Anconi writes code using its tools
4. Automated tests run
5. Claude reviews output + directs corrections
6. d'Anconi commits via git
7. Claude approves merge (human-in-loop checkpoint)

### 9.3 Estimated unlock
- Realistic arrival of autonomous coding: **sessions 500-1000** (phase 5)
- Before that: Claude writes most code, d'Anconi assists with tool-using parts

---

## 10. SUCCESS METRICS (LOCKED)

### 10.1 Per-contract
- **LEO / Social Watch:** live alerts firing, evidence packages admissible, first case closed using the system
- **TAGS Shopping:** consumer site live, first 100 paying users, first deal alert delivered
- **Blizzard/Scopely:** first cheat report accepted, patch landed in production, payment received
- **Wichita/NIAR:** first red team engagement completed with d'Anconi automation
- **Tessere AEC:** first bid delivered with >70% of work from d'Anconi, PE stamp attached
- **DoW:** first phase deliverable accepted, CMMC L2 certified
- **Mid-America Granite & Stone:** marketing site live on client domain (DNS + SSL); admin portal deployed with feature flags active; React Native design app published to iOS + Android (TestFlight / internal testing minimum); Firebase backend fully wired (Auth + Firestore + Storage); first customer design saved end-to-end; first client quote closed through the platform

### 10.2 Platform-level
- vLLM multi-LoRA serving all verticals concurrently
- 58-platform Social Watch coverage
- Evidence crypto at 100% of legal-sensitive outputs
- Token usage under weekly cap (no overages)
- Session discipline enforced (diary entries + wing hygiene)
- Zero cross-wing drift

---

## 11. WHAT THIS PLAN IS NOT

- ❌ A wishlist — every item maps to a signed contract or cross-cutting infra requirement
- ❌ A schedule promising dates — pace is bounded by session throughput + compliance + hires
- ❌ A business plan — pricing, sales, legal handled by Sky's business function
- ❌ Changeable without explicit lock-override — direction stays. Only tactics adjust.

---

## 12. VERSION HISTORY

- **v1** — Locked 2026-04-17 by Sky. Initial Grand Plan: 6 signed contracts (LEO, TAGS, Blizzard/Scopely, Wichita/NIAR, DoW, Tessere), V_CORE/V_GENERAL/V_SW/V_IDENTITY/V_WARRANT/V_GAME/V_GAME_FORENSICS/V_OSINT/V_AEC_{EST,ENG,CAD}/V_NIAR/V_DOW/V_REALTIME/V_MYTHOS adapter architecture, 4-box infrastructure (Vast.ai 3090 + serverless 4090 + VPS + federal GPU, $179/mo consumer cap), 6-phase build order (sessions 1-4000+), 8 MemPalace wings.
- **v1.1** — Amendment 1, locked 2026-04-21 by Sky explicit lock-override. Mid-America Granite & Stone (Wichita, KS) registered as Contract 7 (Consumer Web Build / SaaS replacement; commercial-direct vehicle, not DAV SBA). V_WEBSTACK adapter slot added to brain architecture (post-V5, gated on 3+ similar clients). wing_mag added to §7.1 (9 wings total). Consumer Web Build tier noted in §0.5 infrastructure (external free-tier hosting; does not share the $179/mo 4-box budget). Per-vertical row added in §3.2. V_WEBSTACK training curriculum added in §4.2. MAG bullets added to Phase 2 / Phase 3 / Phase 5 in §8 phased build order. MAG success metric added to §10.1. Authorized `danconi-site-architect` and `web-reverse-engineer` skills for wing_mag scope only (disregard still binding for other wings per rule drawer `drawer_wing_brain_gaps_fbd17fa7a7764173ed7c4ded` with successor drawer pending). Applied by standing manager session.
- **v1.2** — Amendment 2, locked 2026-04-22 by Sky explicit lock-override.
  §3.1 F3 row updated: module filename reaffirmed as `core/vllm_backend.py`
  (exposing class `VLLMMultiLoRaBackend`), not `core/vllm_multi_lora_server.py`.
  Rationale: scoping session WO#7 (2026-04-22) found F3 was already implemented,
  tested, and wired through `core/llm_factory.py` under the actual filename.
  Architecture unchanged — runtime routing, hot-swap, multi-tenant per §2 all
  satisfied. §8 Phase 1 F3 marked DONE (Box-1 deploy runbook at
  `docs/ops/deploy_vllm.md` complete). Box-4 federal deploy runbook remains
  outstanding and is tracked as a separate follow-on work order, not an F3 gap.
  Scoping memo: `docs/decisions/F3_scoping_memo.md`.
- **v1.3** — Amendment 3, locked 2026-04-22 by Sky explicit lock-override.
  §3.1 F5 row updated: module reaffirmed as `core/dynamic_analysis_agent.py`
  (1,750 LOC) paired with Docker image `danconi/malware-sandbox:latest`
  (v1.1.0, image ID c93f3b7b2790, built 2026-02-07), not `windows_sandbox_image`
  as a separate asset. Rationale: scoping session WO#10 (2026-04-22) found F5
  was already implemented, hardened (Wine64 + Frida + tcpdump/tshark + YARA +
  INetSim + monitor stack; non-root, SUID stripped, healthcheck live), and
  shipping. Image also includes F6 `capture_stack` components (Frida + netcap
  + FS/process/registry monitors + timeline aggregator) — F5 ships standalone
  without blocking on F6. §8 Phase 2 "Windows sandbox + cheat analyzer for
  Blizzard" marked PARTIAL: sandbox done, cheat analyzer done via
  `core/gaming_security_agent.py` (5,818 LOC), patch generator remains the
  real §10.1 payment gate. Reproducibility follow-on tracked separately:
  rebuild Dockerfile into `docker/windows_sandbox/` for in-repo source-control
  (image source currently points to `github.com/danconi-ai/sandbox`, external).
  Scoping memo: `docs/decisions/F5_scoping_memo.md`.
- **v1.4** — Amendment 4, locked 2026-04-22 by Sky explicit lock-override.
  §3.1 F6 row updated: module reaffirmed as a layered construct spanning
  `core/dynamic_analysis_agent.py` (BehaviorProfiler timeline correlator
  line 868, APIMonitor line 729, SandboxManager line 493, typed event
  dataclasses lines 341-392) + `core/gaming_security_agent.py` (MemoryScanner
  line 754) + F5 image runtime (Frida + tcpdump/tshark + FS/process/registry
  monitors + timeline aggregator). Not `capture_stack` as a single ~3,500 LOC
  standalone module. Rationale: scoping session WO#12 (2026-04-22) found F6
  is ~80% embedded in the F5 layer for the sandbox-detonation path. Live-game
  path (~30% present) gap is MemoryScanner-only — no live net/API/screen
  capture for in-game telemetry. Screen capture is absent from both repo and
  F5 image (zero ffmpeg/scrcpy/OBS). Screen-capture follow-on tracked
  separately: add `core/screen_capture.py` + ffmpeg to F5 image rebuild
  (may chain on F5 Option B Dockerfile reversal if image rebuild required).
  F6 priority remains P2 — live-game telemetry gap ranks below the §10.1
  Blizzard patch-generator payment gate. All 6 §3.1 foundations resolved
  as of v1.4: F1/F2/F4 tested; F3/F5/F6 reaffirmed under actual implementations.
  Scoping memo: `docs/decisions/F6_scoping_memo.md`.
- **v1.5** — Amendment 5, locked 2026-04-23 by Sky explicit lock-override.
  §2 "Inference-time routing" annotated with footnote ⁴ (new). Footnote
  documents current production wiring: traffic flows through `core/daemon.py`
  → `core/llm_factory.py` → `core/vllm_backend.py`, with tenant resolution
  via `core/danconi_dashboard.UserManager.get_tenant()`. `capability_bridge` +
  `capability_router` remain tested-but-unwired infrastructure; production
  integration is scheduled for §8 Phase 2 when multi-vertical concurrent
  serving motivates category dispatch. §2 architecture not retracted. Rationale:
  WO#17 capability-routing investigation (2026-04-23) Verdict B (partially
  integrated — modules future-ready, activation gated). Investigation memo:
  `docs/decisions/capability_routing_investigation_memo.md`.

---

**END OF GRAND PLAN v1.5. Version bumps require explicit user approval.**
