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
Models that Matter 2

ext Classification / Sentiment / Review Analysis

woojun-jung/my-shopping-review-classification-model — 92.2M params, for classifying shopping reviews (updated Nov 2023)
woojun-jung/roberta-base-finetuned-naver_shopping — RoBERTa fine-tuned on Naver Shopping data, 9 downloads
selvaonline/shopping-assistant — Text Classification, 67M params
ekrem/turkish-shopping-category-classification — Shopping category classification in Turkish

Intent Detection

krishnakamath/shopping_intent — Text Classification, 0.2B params (Nov 2023)
dejanseo/shopping-intent — Image Classification task, updated Nov 2024

Named Entity Recognition (NER)

progg/shopping-list-ner — Token Classification for shopping list NER (2 downloads)
mundo-go/shopping-category-ner — Token Classification for shopping categories

LLM-based Shopping Assistants

Ahmadvertex/mistral_7b_personalized_shopping_assistance1 — Mistral 7B fine-tuned for personalized shopping
cheongmyeong17/Llama-3.2-1B-Instruct-Web-Shopping-10k — Llama 3.2 (1B) fine-tuned on web shopping tasks, with inference available
nitic-nlp-team/webnavix-llama-shopping — 3B Llama for web navigation/shopping
8ft0/shopping-list — Text Generation, 2B params, 15 downloads
Daxuxu36/Yi-9B-Understanding-Shopping-Concepts — Yi 9B for understanding shopping concepts
Rubywong123/AgentGrow-shopping — 8B model for shopping agent tasks

RAG / Retrieval-based

cui54/shopping_rag_all & cui54/shopping_admin_rag — 8B RAG models for shopping (Mar 2025)

Embedding / Vector Models

youssefkhalil320/llm2vec-Qwen2-0.5B-shopping (and variants) — LLM2Vec embeddings fine-tuned for shopping

You can browse these results directly at huggingface.co/models?search=shopping. Would you like me to open any specific model for more details, or search with a more specific shopping-related term (e.g., product recommendation, e-commerce, price prediction)?So Here is what I want I want to find a model that will help me search the web and find the best dealsCreated a planBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsRead page (all)Read page (all)Tool: browser_batchIt seems those searches returned no model results (the model listing area is empty). Let me try more targeted searches.Batch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsStopped on errorFind: "model article cards in the res..."Find: "model article cards in the res..."The accessibility tree does not contain any visible model ar…Tool: browser_batchBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Take screenshotTool: browser_batchGood, I can see relevant results. Now let me also look at some specific notable models — particularly the WebShop-trained ones and the WKM agent. Let me check a couple of the most promising ones.Batch — 2/2 actionsNavigate to https://huggingface.co/zjunlp/...Navigate to https://huggingface.co/leap-ll...Tool: browser_batchBatch — 2/2 actionsExtract page textExtract page textTool: browser_batchNow I have enough information. Let me also quickly check the webshop/il_search_bart model which had the most downloads (25).Navigate to https://huggingface.co/webshop...Extract page textGreat find! Here's what I found. For your use case — searching the web and finding the best deals — what you're really looking for is a web shopping agent model. These are LLMs fine-tuned on the WebShop benchmark, which is specifically designed to simulate browsing e-commerce sites, searching for products, comparing options, and selecting the best one based on criteria like price, features, and reviews.
Here are the best options on Hugging Face:

🏆 Top Recommendations
1. zjunlp/WKM-mistral-webshop-agent
A Mistral-based agent fine-tuned with a "World Knowledge Model" approach for planning and decision-making in web shopping tasks. Specifically built for agentic shopping navigation. View on HF
2. leap-llm/Meta-Llama-3-8B-Instruct-sft-webshop-iter2
Llama 3 8B fine-tuned iteratively on WebShop tasks. The "iter2" version is the most refined iteration, trained to browse, search, and select products on e-commerce sites. View on HF
3. webshop/il_search_bart
A BART-based model from the original WebShop research team (25 downloads, most popular in this category). It handles the search query generation part of shopping — given what you want, it generates optimized product search queries. View on HF
4. webshop/il-choice-bert-image_0 & _1
BERT-based models for the product selection step — ranking and choosing the best product from search results based on your criteria.

⚠️ Important Caveat
These models are trained on the WebShop simulation environment, not live internet browsing. To actually search the web and find real deals, you'd need to pair one of these models with a web browsing/tool-use framework (like LangChain with a search tool, or a browser automation agent). The model handles the reasoning and decision-making, while a separate tool handles the actual web browsing.
Would you like me to open any of these model pages in detail, or look for agent frameworks that integrate shopping models with live web search?Do they have ones that are shopping with web browsing/tool-use frameworkCreated a planBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchBoth returned 0 results. Let me try more targeted searches using the full-text search and different terms.Batch — 2/2 actionsNavigate to https://huggingface.co/search/...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsNavigate to https://huggingface.co/search/...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 1/2 actionsStopped on errorExtract page textOutput exceeds 4000 character limit (17092 characters). Try …Tool: browser_batchBatch — 1/2 actionsStopped on errorExtract page textOutput exceeds 8000 character limit (17092 characters). Try …Tool: browser_batchBatch — 2/2 actionsExtract page textTake screenshotTool: browser_batchExcellent! I found some very strong results. Now let me check the top two most relevant ones — McGill-NLP/Llama-3-8B-Web and thebajajra/RexReranker — for their details.Batch — 2/2 actionsNavigate to https://huggingface.co/McGill-...Navigate to https://huggingface.co/thebaja...Tool: browser_batchBatch — 2/2 actionsExtract page textExtract page textTool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchI now have a very clear picture. Let me also quickly check the nitic-nlp-team/webnavix-llama-shopping model from before, since it was specifically designed for web navigation + shopping.Navigate to https://huggingface.co/nitic-n...Take screenshotHere's what I found. The honest answer is: there is no single standalone model on Hugging Face that does everything end-to-end (browse the real web + find deals), but there are models specifically built for the web browsing + shopping combination. Here's the breakdown:

🥇 Best Match: McGill-NLP/Llama-3-8B-Web
This is the closest thing to what you want. It's a Llama 3 8B model fine-tuned on WebLINX — over 24,000 real web interaction instances including clicks, text input, form submission, and web navigation. It's explicitly designed as a web browsing agent that can perform tasks like shopping across 150+ websites. It has 428 downloads/month and is actively maintained.
👉 huggingface.co/McGill-NLP/Llama-3-8B-Web
🥈 thebajajra/RexReranker-0.6B
A very recent (2025) and popular model (1,226 downloads/month) specifically for e-commerce product discovery. You give it a query + a candidate product, and it scores how relevant/good that product is. It covers Amazon, eBay, Walmart, Target, Best Buy, and many more. This is ideal for the "compare and pick the best deal" step.
👉 huggingface.co/thebajajra/RexReranker-0.6B
🥉 nitic-nlp-team/webnavix-llama-shopping
A 3B Llama model from the WebNavix collection, focused specifically on web navigation for shopping tasks. Lightweight but minimal documentation.
👉 huggingface.co/nitic-nlp-team/webnavix-llama-shopping

💡 The Practical Reality
To actually search the web and find the best deals in real-time, the best approach on Hugging Face is to combine two things:

A web-browsing agent model (like McGill-NLP/Llama-3-8B-Web) to navigate shopping sites and extract product info
A reranker model (like RexReranker-0.6B) to score and rank the products by relevance to your query

No single model fully wraps both together with live internet access out of the box — that kind of live integration requires a framework like LangChain or a browser automation tool around the model.
Would you like me to open the McGill-NLP or RexReranker model page for a closer look?Okay what About Construction Cost estimateingCreated a planBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/search/...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/search/...Tool: browser_batch0 results on the name search. Let me check the full-text search and try alternate keyword searches simultaneously.Batch — 2/2 actionsExtract page textNavigate to https://huggingface.co/models?...Tool: browser_batchI found two very strong matches. Let me now look at them in detail — Aphrodine/constructionai and ArapCheruiyot/metrrik-gemma-3-1b.Batch — 2/2 actionsNavigate to https://huggingface.co/Aphrodi...Navigate to https://huggingface.co/ArapChe...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchExcellent finds! Let me also check the margenai/AHTE-Mistral-7B-EPC model and scroll down on ConstructionAI for more details.Batch — 2/2 actionsStopped on errorNavigate to https://huggingface.co/margena...Scroll downunknown tool "scroll"Tool: browser_batchBatch — 2/2 actionsScroll downTake screenshotTool: browser_batchGreat news — there are some really solid construction cost estimation models on Hugging Face! Here's what I found:

🏆 #1 — Aphrodine/constructionai
This is the standout model — billed as "the first open-weight LLM built for the construction industry." It's a Llama 3.1 8B fine-tuned specifically for cost estimating, not just a general model with a construction prompt. Key highlights:

Trained on 138,904 real project examples with actual pricing, building codes, and trade knowledge
Covers 25+ trades (electrical, plumbing, framing, concrete, roofing, etc.)
Has 9 function-calling tools that can generate PDFs, takeoffs, and schedules
85%+ estimation accuracy on its eval benchmark
Runs on Ollama, and available in 4-bit (4.6GB) and 8-bit (8.5GB) GGUF formats
Runs on RunPod Serverless for production use

👉 huggingface.co/Aphrodine/constructionai

🥈 #2 — ArapCheruiyot/metrrik-gemma-3-1b
A lightweight Gemma 3 1B model (only 1GB!) fine-tuned for Quantity Surveying in the Kenyan construction market. It can generate Bills of Quantities (BOQs), preliminary cost estimates, variation orders, and elemental cost breakdowns based on local market rates.

Very lightweight — runs on Apple MLX (2GB quantized)
Specifically designed for quantity surveyors, architects, and contractors
References the Cost Handbook For Construction Works (2025–2026)

👉 huggingface.co/ArapCheruiyot/metrrik-gemma-3-1b

🥉 #3 — margenai/AHTE-Mistral-7B-EPC
A Mistral 7B model fine-tuned for EPC (Engineering, Procurement & Construction) projects — specifically power transmission lines, grid stations, civil infrastructure (foundations, buildings, dams), BOQ analysis, material takeoffs, and cost estimation. Tailored to NTDC/WAPDA/PEC engineering standards.
👉 huggingface.co/margenai/AHTE-Mistral-7B-EPC

Bottom Line: If you're looking for a general-purpose construction cost estimator for US/general markets, ConstructionAI is your best bet by far — it's the most mature, most data-rich, and most practical. Would you like me to open any of these for a deeper look?What about models to pull the lates treands on all social mediaCreated a planBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/search/...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/search/...Tool: browser_batchOnly 1 result for "social media trends". Let me broaden the search with multiple different queries simultaneously.Batch — 1/2 actionsStopped on errorExtract page textOutput exceeds 15000 character limit (20367 characters). Try…Tool: browser_batchBatch — 2/2 actionsExtract page textNavigate to https://huggingface.co/models?...Tool: browser_batchGood findings. I spotted some interesting ones — let me also check the social media analysis models list and look at a few key models closely.Batch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/ryu34/v...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/AventIQ...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/SoftALL...Tool: browser_batchTake screenshotHere's the honest picture on this one. This is an area where Hugging Face has some useful components, but no single model that fully does "pull live trends from all social media" on its own. Here's what exists and what you'd need to know:

⚠️ The Core Reality First
No model on Hugging Face can live-scrape social media platforms like TikTok, Instagram, X/Twitter, or Reddit on its own — those platforms require APIs or scrapers to get the data. What HF models do is analyze and classify that data once you have it. Here are the best options:

🏆 #1 — ryu34/viralcut-agent
The closest thing to what you're describing. It's a Qwen2.5-3B agent fine-tuned with tool-calling for social media optimization. It has a dedicated Trend Research capability that actively searches for current trends, viral hooks, and sounds via web search. It also scores content for TikTok/Instagram/YouTube algorithm virality. Best for content creators who want to ride trends.
👉 huggingface.co/ryu34/viralcut-agent
🥈 #2 — AventIQ-AI/text-summarization-for-social-media-trends
A T5-based model (60.5M params) specifically fine-tuned for summarizing social media trend content. Feed it a collection of trending posts or articles and it condenses them into concise summaries. Good for digesting large volumes of trend data quickly.
👉 huggingface.co/AventIQ-AI/text-summarization-for-social-media-trends
🥉 #3 — SoftALL/OBSIDIAN
A real-time social media intelligence system (AraBART-based, 133 downloads/month) for classifying social media posts into threat, violence, distress, complaint, or neutral categories. Designed for Arabic social media monitoring and early-warning dashboards. Great for social listening and sentiment classification in Arabic.
👉 huggingface.co/SoftALL/OBSIDIAN
#4 — boltuix/NeuroFeel & Varnikasiva/sentiment-classification-bert-mini
Lightweight BERT-based models for real-time emotion and sentiment detection across social media posts — useful for tracking how audiences are feeling about any trending topic across platforms.

💡 What You'd Actually Need for Full Trend Pulling
To truly monitor trends across all social media, the practical stack would be: a data collection layer (platform APIs or tools like Apify, Brand24, or Sprout Social to pull live posts) feeding into an NLP model (like the ones above) that classifies, summarizes, and surfaces what's trending. The HF models handle the analysis side, not the live data collection side.
Would you like me to look for any more specialized angle — like trend prediction, hashtag analysis, or a specific platform like TikTok or Reddit?Yes specilizedCreated a planBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 1/2 actionsStopped on errorFind: "model article cards in search ..."The accessibility tree provided does not contain any visible…Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchZero results for both. Let me try broader single-word platform searches and full-text search for each platform.Batch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchGood results on both. Now let me search for Twitter/X and Instagram + YouTube models simultaneously.Batch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchExcellent — Twitter has 602 models. Instagram hashtag has 0, let me try just "instagram". Now let me also check YouTube and viral prediction.Batch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchGood. Now let me look at the top Twitter model and the TikTok-specific models more closely, plus check for hashtag generation models for Instagram.Batch — 2/2 actionsNavigate to https://huggingface.co/cardiff...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchNow let me check the top Reddit model and the TikTok content model, plus the CardiffNLP Twitter family of models (they have an extensive suite).Batch — 2/2 actionsNavigate to https://huggingface.co/cardiff...Navigate to https://huggingface.co/minh21/...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchI now have a comprehensive picture. Cardiff NLP is clearly the gold standard for Twitter/X with 177 models and 3.36M downloads. Let me compile everything.Here's a full breakdown of the best platform-specific models on Hugging Face for trend monitoring:

🐦 X / Twitter — Best Coverage (602+ models)
Cardiff NLP is the undisputed king here. They have 177 Twitter-specific models built on their TweetNLP library, all trained on hundreds of millions of real tweets.

cardiffnlp/twitter-roberta-base-sentiment-latest — The flagship. 3.36M downloads/month, with live inference. Classifies tweets as positive/negative/neutral. Updated Aug 2025. 👉 Link
cardiffnlp/twitter-xlm-roberta-base-sentiment — Multilingual version covering 8 languages (Arabic, English, French, German, Hindi, Italian, Spanish, Portuguese). 1.41M downloads/month, live inference available. 👉 Link
citizenlab/twitter-xlm-roberta-base-sentiment-finetunned — Further fine-tuned version, 3.89k downloads/month. 👉 Link


📱 TikTok — 128 Models
TikTok-specific models are mostly focused on content style and optimization rather than trend scraping:

ryu34/viralcut-agent — (Covered previously) The best option — actively searches TikTok trends via web search and scores content for TikTok algorithm virality. 👉 Link
ai-maker-space/snowflake-ft-tiktok — Sentence similarity model fine-tuned on TikTok content, 5 downloads/month. 👉 Link
sr1ya/distilbert-tiktokjam — DistilBERT (67M) fine-tuned on TikTok comments and content. 👉 Link


🤖 Reddit — 8 Dedicated Models

minh21/XLNet-Reddit-Sentiment-Analysis-16-epochs — The most active Reddit model with 108 downloads/month and 84.27% accuracy on Reddit sentiment classification. XLNet-based, MIT licensed. 👉 Link
minh21/XLNet-Reddit-Sarcasm-Analysis — Detects sarcasm in Reddit posts specifically — super useful since Reddit sarcasm often drives viral threads. 👉 Link
spacesedah/reddit-sentiment-analysis-longformer — Longformer-based (handles long Reddit threads, not just short posts), 71 downloads/month. 👉 Link


📺 YouTube — Very Limited (1 Model)

minwook-shin/youtube_trending_korean_title_description — Analyzes Korean YouTube trending video titles and descriptions. Niche, but the only dedicated YouTube trending model found. 👉 Link

Instagram and YouTube are largely unsupported on Hugging Face for platform-specific trend models.

🌐 Cross-Platform / Viral Prediction

nleroy917/viral-sequence-prediction — Predicts whether content sequences will go viral. Small but one of a kind.
kesavanguru/XLM_roberta_finetuned — XLM-RoBERTa for clickbait and trend detection across platforms.


Bottom Line: Twitter/X has by far the richest model ecosystem thanks to Cardiff NLP. Reddit has solid coverage. TikTok is mostly content optimization-focused. YouTube and Instagram are largely gaps on Hugging Face. Would you like me to dig into any specific platform deeper, or look at how to combine these into a unified monitoring pipeline?What About Ebay and site specific Shopping sitesCreated a planGet tabsCreate new tabBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchGood results! 31 eBay models and 53 Amazon models. Now let me also check Etsy, Walmart, and a broader e-commerce search simultaneously.Batch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batch38 Walmart models, 0 Etsy. Let me now dig into the top eBay and Amazon specific models, and also check for Amazon reviews/sentiment and price prediction models.Batch — 2/2 actionsNavigate to https://huggingface.co/mhetrer...Navigate to https://huggingface.co/LiYuan/...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchNow let me check the RexReranker (which covers Amazon, eBay, Walmart, etc. all together) and look at the top Walmart model more closely, plus check Amazon reviews model.Batch — 2/2 actionsNavigate to https://huggingface.co/compile...Navigate to https://huggingface.co/lorahub...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchLet me check the best Amazon reviews model and also the RexReranker multi-site model which we found earlier.Batch — 2/2 actionsNavigate to https://huggingface.co/TheFuzz...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchI now have a thorough picture across all platforms. Let me compile the full findings.Here's the full breakdown of site-specific shopping models on Hugging Face by platform:

🛒 Amazon — Best Coverage (53+ models)
Amazon has the richest ecosystem by far.
Product Ranking & Search:

LiYuan/amazon-query-product-ranking — DistilBERT fine-tuned on 571K Amazon shopping queries to rank products by relevance. Trained on Amazon's public shopping dataset. 10 downloads/month, live inference ready. 👉 Link
nthieu/bert-amazon-product-classification (multiple versions) — BERT classifiers for Amazon product categories.

Review Sentiment (52 models):

LiYuan/amazon-review-sentiment-analysis — The top Amazon review sentiment model with 676 downloads/month and 44 likes. Classifies reviews as positive/negative/neutral. 👉 Link
TheFuzzyScientist/T5-base_Amazon-product-reviews — T5-based text generation model for summarizing/analyzing Amazon product reviews, 17 downloads/month.
sohan-ai/sentiment-analysis-model-amazon-reviews — 35 downloads/month, 4 likes. Lightweight DistilBERT.

Multi-Site (covers Amazon + eBay + Walmart + more):

thebajajra/RexReranker-0.6B — (Already covered) The powerhouse covering Amazon, eBay, Walmart, Target, Best Buy, Flipkart, Etsy, Shopify, Taobao, and 15+ more. 1,226 downloads/month. Best all-in-one option. 👉 Link


🟡 eBay — 31 Models

niksss/xlm-roberta-large-finetuned-ebay — XLM-RoBERTa Large (600M params) fine-tuned specifically on eBay listing data. Fill-Mask task for understanding eBay product language. 👉 Link
mhetrerajat/distilbert-base-uncased-ebay-ner-2022 — DistilBERT fine-tuned for Named Entity Recognition on eBay listings — extracts product names, brands, specs from listing titles. 👉 Link
EBayego/Zephyr — A 7B Zephyr-based model from the ebayes org for text generation, tailored for eBay-related language understanding. 👉 Link


🔵 Walmart — 38 Models

Walmart-the-bag/Misted-7B — Most downloaded Walmart model (145 downloads/month, 8 likes). 7B text generation model focused on Walmart product data. 👉 Link
igor.freik/walmart_reviews_summarizer_2 — Specifically summarizes Walmart customer reviews to extract key insights. 👉 Link
Rumana85/walmart_sales_model — Sales trend prediction model for Walmart product data. 👉 Link


🟠 Etsy — No Dedicated Models Found
Etsy has essentially zero dedicated models on Hugging Face, though the RexReranker (above) does include Etsy in its training data.

🌐 The Best All-In-One Option
If you want a single model that handles Amazon, eBay, Walmart, Target, Best Buy, Etsy, Shopify, Alibaba, Flipkart, Taobao, and more all at once, thebajajra/RexReranker-0.6B is your best bet — it's a product reranker trained specifically on multi-retailer e-commerce data and is the most actively used model in this space right now.
Would you like me to open any of these models for a closer look, or search for something more specific like price prediction or deal scoring?What about Teaching an AI to automate Cad, and revit and excel\Created a planCreate new tabBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchBoth 0. Let me broaden searches with different terms across all three areas simultaneously.Batch — 3/3 actionsStopped on errorNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Navigate to https://huggingface.co/search/...Failed to navigate: Tab 1499279922 is not in the same group …Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchGreat — 15 AutoCAD models found, 0 for "excel automation". Let me now look at the key AutoCAD models closely, search for Revit with different terms, and search for Excel/spreadsheet with different terms.Batch — 2/2 actionsNavigate to https://huggingface.co/Ak015/A...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchGood - found the AutoCAD generator. Now let me search for the tool-calling AutoCAD model, BIM/IFC models, and Excel/table models with better keywords.Batch — 2/2 actionsNavigate to https://huggingface.co/junha20...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batch4 Excel formula models found. Now let me dig into the key models and also search for BIM/IFC (the format Revit uses) and the full-text search for Revit.Batch — 2/2 actionsNavigate to https://huggingface.co/mraderm...Navigate to https://huggingface.co/search/...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/search/...Tool: browser_batchExtract page textExcellent — found a goldmine! schauh11/revit-coder-14b is a perfect match. Let me now look at this model and also the AutoCAD tool-calling model details more closely.Batch — 2/2 actionsNavigate to https://huggingface.co/schauh1...Navigate to https://huggingface.co/ahaduzz...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchI now have everything I need. Let me compile the full picture.Great finds across all three tools! Here's the full breakdown:

📐 AutoCAD — 15 Models
🏆 #1 — Ak015/AutoCAD_generator
The most downloaded AutoCAD-specific model (10 downloads/month). A 7B parameter text-to-code model tagged with text2json, text2code, AutoCAD, and generative-design. It's designed to generate AutoCAD scripts and commands from natural language descriptions in both English and Hindi. Apache-2.0 license.
👉 huggingface.co/Ak015/AutoCAD_generator
#2 — ahaduzzamanasif/AutoCADAI
A 3B Llama-based model fine-tuned with SFT/TRL specifically for AutoCAD AI tasks. Lightweight (4-bit quantized) and conversational — good for instruction-following AutoCAD commands.
👉 huggingface.co/ahaduzzamanasif/AutoCADAI
#3 — junha2020/functiongemma-270m-it-autocad-tool-calling
The most specialized — a tiny Gemma 270M model explicitly built for AutoCAD tool-calling (function calling format). Very lightweight and purpose-built to trigger specific AutoCAD API functions from natural language.
👉 huggingface.co/junha2020/functiongemma-270m-it-autocad-tool-calling
#4 — shahabctg/AutoCAD_clean_Nov (+ variants)
A family of fine-tuned AutoCAD denoising/cleanup models — good for cleaning up messy AutoCAD drawing data.

🏗️ Revit / BIM — Clear Winner Found!
🏆 #1 — schauh11/revit-coder-14b
This is the standout model of this entire search. A Qwen3-14B model fine-tuned specifically for:

Revit API C# code generation (write Revit plugins and scripts from plain English)
IFC reasoning (understand BIM file structure and spatial hierarchies)
BIM development patterns (AEC-specific design logic)

Trained on 177,127 Revit/BIM examples for only $48 on a single GPU, scoring 0.80 composite score on a 40-question Revit C# benchmark. 25 downloads/month and trending upward. Apache-2.0 license.
👉 huggingface.co/schauh11/revit-coder-14b
#2 — mradermacher/revit-coder-14b-GGUF
The same model quantized to GGUF format — runs locally on Ollama or llama.cpp without a GPU.
👉 huggingface.co/mradermacher/revit-coder-14b-GGUF
#3 — carlosh10/CERCON
A Portuguese-language RAG agent that includes a CAD/BIM assistant module capable of generating .scr AutoCAD scripts and Dynamo/Revit Python API scripts — particularly for fire safety layout automation in buildings.

📊 Excel / Spreadsheet — 4 Models
🏆 #1 — mradermacher/excel-formula-t5-GGUF
The most active Excel model with 53 downloads/month. A T5-based (0.2B) model quantized to GGUF for local use — takes plain English and generates Excel formulas. Multiple quantization sizes from 94MB (2-bit) to 447MB (16-bit). Easy to run locally.
👉 huggingface.co/mradermacher/excel-formula-t5-GGUF
#2 — leonfromwork/excel-formula-t5
The original T5 model that the above is based on — generates Excel formulas from natural language descriptions.
👉 huggingface.co/leonfromwork/excel-formula-t5
#3 — ashkanpourali/Excelformulas
Another Excel formula generation model (Dec 2024).
#4 — benkemp/granite-excel-formula-qlora
IBM Granite fine-tuned with QLoRA for Excel formula generation.

💡 Summary by use case:
ToolBest ModelMaturityAutoCADAk015/AutoCAD_generator (7B)Early stageRevit/BIMschauh11/revit-coder-14bVery solid — production-readyExcelmradermacher/excel-formula-t5-GGUFLightweight & practical
Revit has by far the most mature model thanks to revit-coder-14b. Would you like to open any of these for a deeper look?Coupons?Created a planCreate new tabBatch — 2/2 actionsNavigate to https://huggingface.co/models?...Navigate to https://huggingface.co/models?...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batch3 coupon models found. Let me look at those closely and also run broader related searches simultaneously.Batch — 2/2 actionsNavigate to https://huggingface.co/visharx...Navigate to https://huggingface.co/search/...Tool: browser_batchBatch — 2/2 actionsTake screenshotNavigate to https://huggingface.co/search/...Tool: browser_batchExtract page textExtract page textBatch — 2/2 actionsNavigate to https://huggingface.co/strange...Navigate to https://huggingface.co/Tanishq...Tool: browser_batchBatch — 2/2 actionsTake screenshotTake screenshotTool: browser_batchHonestly, this is one of the thinner areas on Hugging Face. Here's what exists:

🎟️ Coupon-Specific Models — Only 3 Found
🏆 #1 — visharxd/coupon-generator-2
The most active coupon model with 23 downloads/month. It's a Llama 3.1 8B model (GGUF 4-bit, 4.92GB) fine-tuned on a custom discount-coupons dataset. It's conversational and designed to generate coupon content and discount offers. The best pure-play coupon model on HF.
👉 huggingface.co/visharxd/coupon-generator-2
#2 — strangebank/coupon
A minimal OpenRail-licensed model with no model card documentation — essentially a bare-bones coupon-related model with no description of what it actually does.
👉 huggingface.co/strangebank/coupon

🛒 Related — E-Commerce Models with Coupon Features
TanishqV03/EASYSELLAI
A 100M parameter GPT-style e-commerce chatbot SLM that includes a built-in coupon system as one of its features alongside cart management, order tracking, checkout, and 50+ FAQ auto-answers. Trained on 30,000 synthetic e-commerce conversations.
👉 huggingface.co/TanishqV03/EASYSELLAI

⚠️ The Honest Picture
Coupons are a very underrepresented use case on Hugging Face. The full-text search turned up very little that's genuinely coupon-focused — most results were general LLMs that simply mentioned coupon codes in their documentation, not models trained for coupon-related tasks.
What would actually work well for coupons is combining a few things we already found earlier in our session: the RexReranker (to rank and score deals) + a web browsing agent (like McGill-NLP/Llama-3-8B-Web) to scrape live coupon sites + a general-purpose LLM to extract and validate promo codes from web pages.
Would you like to explore a different angle — like models for deal extraction from emails, price drop alerts, or loyalty reward optimization?Conversation compactedConversation summaryOSINT and Cyber security to automate an AI27 steps27 steps🔐 OSINT & Cybersecurity Automation — Hugging Face Models
Great topic! There's a surprisingly strong ecosystem here — 291 cybersecurity models, 17 OSINT-specific models, and many more under related tags. Here's the full breakdown by category:

🕵️ CATEGORY 1: OSINT Models
🥇 mahmoudalyosify/Horus-OSINT
🔗 https://huggingface.co/mahmoudalyosify/Horus-OSINT

Type: GGUF · Llama-3 8B · 4-bit (4.92 GB)
Downloads: 74/mo
What it does: Fine-tuned on 20M+ records from the Global Terrorism Database (GTD) and GDELT — generates structured intelligence reports with GEOPOLITICAL CONTEXT and THREAT ASSESSMENT sections. Built for situational awareness and pattern analysis of geopolitical events
Training: 159,826 instruction samples via AWS EMR / Apache Spark pipeline
Run with: Ollama locally, great for automated report generation
License: Apache 2.0

aab20abdullah/qwen_OSINT
🔗 https://huggingface.co/aab20abdullah/qwen_OSINT

Type: Qwen-based, 4B · 537 downloads/mo
General OSINT assistant, actively maintained (updated 25 days ago)

Rkdon11/deberta-v3-large-osint-cybersecurity-ner
🔗 https://huggingface.co/Rkdon11/deberta-v3-large-osint-cybersecurity-ner

Type: Token Classification · DeBERTa v3 Large (0.4B) · 6 downloads/mo
Specialized NER for extracting OSINT + cybersecurity entities from text (vendor names, CVE IDs, versions, techniques)

hugsanaa/WAR-OSINT-AI-Detecting-Disinformation
🔗 https://huggingface.co/hugsanaa/WAR-OSINT-AI-Detecting-Disinformation-and-...

Type: Text Generation · 41 downloads/mo
Detects disinformation in OSINT contexts — useful for social media analysis pipelines


🧠 CATEGORY 2: General Cybersecurity LLMs (Security Chatbots & Reasoning)
🥇 RavichandranJ/Dolphin3-Cyber-8B-GGUF
🔗 https://huggingface.co/RavichandranJ/Dolphin3-Cyber-8B-GGUF

Downloads: 5,670/mo ← Most popular cybersecurity LLM on HF
Size: 8B params, 11 GGUF quantizations (Q2_K at 3.18 GB up to F16 at 16.1 GB)
What it does: Full red team + blue team assistant — covers OWASP Top 10, MITRE ATT&CK, CVE analysis, exploit development, code review, incident response, and CTF challenges. "Abliterated" (refusal vectors removed for security research topics)
Runs on: GTX 1650+ (4GB VRAM), 100% local/private
Best use: Your go-to local AI for penetration testing methodology, writing security scripts, CTF help, vulnerability analysis

AlicanKiraz0/Cybersecurity-BaronLLM_Offensive_Security_LLM_Q6_K_GGUF
🔗 https://huggingface.co/AlicanKiraz0/Cybersecurity-BaronLLM_Offensive_Security_LLM_Q6_K_GGUF

Downloads: 858/mo
Size: 8B · 6-bit GGUF (6.6 GB)
What it does: Fine-tuned specifically for offensive security — generates full ATT&CK chains, C2 playbooks, social engineering scenarios, step-by-step vulnerability analysis, PoC code generation, and SIEM log triage. Trained on NVD/CVE databases, exploit writeups from Project Zero, PortSwigger, NCC Group, and synthetic ATT&CK chains. Has RLHF safety guardrails
Note: Requires agreeing to share contact info (email) to access files — gated model

vanshkamra12/CyberSecurity-Model
🔗 https://huggingface.co/vanshkamra12/CyberSecurity-Model

Downloads: 1,010/mo
Size: 4B
General cybersecurity assistant, actively maintained


🦅 CATEGORY 3: Offensive Security / Pen Testing Automation
RavichandranJ/Dolphin3-Cyber-8B-GGUF
(Listed above — this is the best all-around option for pen testing automation)
radoni/penetrationtestingautomation-ai
🔗 https://huggingface.co/radoni/penetrationtestingautomation-ai

Type: Text Generation · Updated June 2025
Focused specifically on automating penetration testing workflows

🌟 meccatronis/hexstrike_ai — MCP Framework
🔗 https://huggingface.co/meccatronis/hexstrike_ai

What it is: Not a standalone LLM, but an AI-Powered MCP (Model Context Protocol) Cybersecurity Automation Platform — HexStrike AI v6.0
What it does: Connects Claude, GPT, or any MCP-compatible AI agent to 150+ professional security tools (Nmap, Nuclei, SQLMap, Hydra, Ghidra, Metasploit, etc.) through 12+ autonomous AI agents
Agents include: BugBountyWorkflowManager, CTFWorkflowManager, CVEIntelligenceManager, AIExploitGenerator, VulnerabilityCorrelator, TechnologyDetector
Performance claims: 24x faster subdomain enumeration, 16x faster vulnerability scanning vs manual
OSINT tools built-in: 20+ tools including Amass, Sherlock, SpiderFoot, TheHarvester, Maltego, Recon-ng
Install: git clone https://github.com/0x4m4/hexstrike-ai.git
⚠️ This is the closest thing to a full AI-automated cybersecurity platform on HF


🔍 CATEGORY 4: Threat Intelligence & Entity Extraction (NLP)
selfconstruct3d/FALCON
🔗 https://huggingface.co/selfconstruct3d/FALCON

Type: ModernBERT embedding model · Feature Extraction
What it does: Maps cybersecurity threat descriptions (CTI reports) to a shared semantic space for APT attribution — built on Cisco's SecureBERT 2.0. Can cluster incidents, attribute to threat actors, run semantic search over CTI corpora
Tags: MITRE-ATTACK, CTI, APT, threat-intelligence, contrastive-learning
Built by the Austrian Institute of Technology Cybersecurity Group

danitamayo/bert-cybersecurity-NER
🔗 https://huggingface.co/danitamayo/bert-cybersecurity-NER

Type: Token Classification · DistilBERT · 89.3% validation accuracy
What it does: Extracts named entities from vulnerability text — vendor, application, version, CVE ID, hardware, OS, programming language, attack methods
Feed it CVE descriptions and it pulls out structured entities automatically

barghavani/MITRE_cybersecurity
🔗 https://huggingface.co/barghavani/MITRE_cybersecurity

Token classification mapped to MITRE ATT&CK framework categories

CyberPeace-Institute/Cybersecurity-Knowledge-Graph
🔗 https://huggingface.co/CyberPeace-Institute/Cybersecurity-Knowledge-Graph

Downloads: 136/mo
Knowledge graph model for cybersecurity — structured relationship extraction between security concepts


🦠 CATEGORY 5: Malware Detection
sibumi/DISTILBERT_static_malware-detection
🔗 https://huggingface.co/sibumi/DISTILBERT_static_malware-detection

Downloads: 79/mo · Updated Dec 2024
Static analysis malware detection using DistilBERT (0.1B) — classifies files based on textual/feature analysis without execution

majorprojectmalware/malware-detection-model (v1–v4)
🔗 https://huggingface.co/majorprojectmalware/malware-detection-model

Text Classification · 67M params · Multiple versions (v1 through v4)
Iteratively improved malware classifier

yousinator/Malware-Detection-Using-PE-Imports
🔗 https://huggingface.co/yousinator/Malware-Detection-Using-PE-Imports

PE (Portable Executable) import analysis for Windows malware — very specific and useful for static analysis pipelines

mihai-chindris/malware-detection-lgbm
🔗 https://huggingface.co/mihai-chindris/malware-detection-lgbm

LightGBM tabular classifier for malware features — fast, lightweight, good for integration


⚡ How to Build an AI Cybersecurity/OSINT Automation Stack
Here's how these models fit together for a complete automated pipeline:
TaskBest ModelLocal AI security brainRavichandranJ/Dolphin3-Cyber-8B-GGUFFull pen test automation frameworkmeccatronis/hexstrike_ai (HexStrike MCP)OSINT & geopolitical threat analysismahmoudalyosify/Horus-OSINTExtract entities from CVEs/reportsdanitamayo/bert-cybersecurity-NERThreat actor attribution / CTIselfconstruct3d/FALCONStatic malware detectionsibumi/DISTILBERT_static_malware-detectionOffensive security reasoningAlicanKiraz0/Cybersecurity-BaronLLM

⚠️ Important Notes
Live reconnaissance requires external tools — no HF model can directly run Nmap, Shodan queries, or scrape OSINT sources on its own. The LLMs above reason about and generate scripts; you need a framework like HexStrike (which wraps those tools) to actually execute them.
Abliterated models (Dolphin3-Cyber) remove safety refusals — these are designed for security professionals with authorized access to test systems. Always operate within legal boundaries and with proper authorization.
Want me to dig deeper into any specific subcategory — like CTF solvers, vulnerability scanners, network analysis, or a specific framework integration?Yes These would Be great deal extraction from emails, price drop alerts, or loyalty reward optimization41 steps41 steps💰 Deal Extraction from Emails, Price Drop Alerts & Loyalty Rewards — Hugging Face Models
Honest upfront assessment: this is a mixed bag. Email deal extraction and loyalty rewards are very underserved on Hugging Face as dedicated models. However, there are excellent adjacent models that you can combine into a working pipeline. Here's the full picture:

📧 CATEGORY 1: Email Deal / Promo Extraction
⚠️ Dedicated "deal extraction from email" models: 0 on HF
No model exists that specifically reads your inbox and pulls out discount codes, offers, or expiry dates from promotional emails. However, here's what does exist and covers the same ground:

🥇 M-Arjun/SpamShield — Promotional Email Classifier
🔗 https://huggingface.co/M-Arjun/SpamShield

Type: Text Classification · sklearn/ONNX · multilingual (EN, ES, AR, HI, ZH, FR, DE)
What it does: Classifies emails into categories including a dedicated Marketing/Promotional class — specifically trained to detect "discount codes, time-limited offers, buy now, act now" language. This is your Step 1: filter inbox for deal emails
Trained on SpamShield-Datasets (35 likes, active as of 16 days ago)
Categories covered: spam, scam, marketing/promotional, adult content, giveaways, crypto fraud, etc.

snar7/email_body_extraction
🔗 https://huggingface.co/snar7/email_body_extraction

Updated Jan 2025 — focuses on extracting key content from email bodies, useful as a preprocessing step before running NER

For NER on extracted deal text — pair with:
ngvozdenovic/invoice_extraction
🔗 https://huggingface.co/ngvozdenovic/invoice_extraction

Downloads: 40/mo · Token Classification
Extracts structured fields from documents — vendor, amounts, dates, line items — works well when applied to deal/order confirmation emails

iamkhadke/invoice-extraction-v2-llama-2-7b-v2
🔗 https://huggingface.co/iamkhadke/invoice-extraction-v2-llama-2-7b-v2

LLaMA 2 7B fine-tuned for structured document extraction — can be prompted to pull discount %, expiry date, offer code from email text


🧾 CATEGORY 2: Receipt / Document Parsing (for tracking purchases & prices)
This category is surprisingly strong on HF — 25+ LayoutLMv3 models fine-tuned specifically on receipts.
🥇 albertosei/layoutlmv3-receipt-parser
🔗 https://huggingface.co/albertosei/layoutlmv3-receipt-parser

Downloads: 14/mo · 89.34% validation accuracy
What it does: Extracts 25 entity types from receipt images including: vendor name, address, phone, date, time, receipt ID, total amount, subtotal, tax, discount amount, tip, payment method, line item names, unit prices, line item totals, item-level discounts
Trained on 1,426 real receipt samples
Architecture: LayoutLMv3-base (0.1B) · Apache 2.0
Requires OCR first (e.g. Tesseract/EasyOCR), then feeds image + words + bounding boxes to the model
Best use: Scan receipts → extract every line item, price, and discount applied → feed into a price tracking database

NLPmonster/layoutlmv3-for-complete-receipt-understanding
🔗 https://huggingface.co/NLPmonster/layoutlmv3-for-complete-receipt-understanding

Downloads: 8/mo · 87.6% accuracy, F1: 0.6554
Two-stage fine-tune (first pass → then complete understanding)
Good alternative to albertosei's parser

Other LayoutLMv3 receipt models (25 total):

Sameed1/layoutlmv3-receipts — 9 downloads/mo
janodis/layoutlmv3-finetuned-receipts — token classification
rodrigovidsilva/layoutlmv3-finetuned-receipt — 5 downloads/mo
tomal001/layoutlmv3-with-uk-receipts — fine-tuned on UK receipt formats


📉 CATEGORY 3: Price Drop Alerts / Price Prediction
⚠️ Dedicated "price drop alert" models: 0 on HF
No model exists that monitors live prices and fires an alert. Price alerting requires a data pipeline (web scraping + time-series storage), not just an ML model. However:
What does exist — Price Prediction Models:
Flight Prices (5 models):

matanzig/flight-price-prediction — Tabular Regression · Updated 1 day ago (very fresh!)
Pudamya/flight-price-prediction-xgboost-model — XGBoost-based, fast inference
Riswari/flight-price-prediction — Updated Mar 2025
These predict whether a price will rise/fall — useful for "should I buy now?" logic

General Retail/Product Prices (144 models under "price prediction"):
Mostly house/real estate price models, but the underlying tabular regression architectures (XGBoost, LightGBM, sklearn) are directly reusable. The top performers:

moro23/used-phones-price-prediction — predicts resale value of phones given condition/specs — useful template for product price prediction
Iqbaljanitra/Mobile_Price_Classification_Price_Prediction — mobile price tier classification

Product Review Sentiment (20 models) — a proxy for price-worthiness:
eakashyap/product-review-sentiment-analyzer
🔗 https://huggingface.co/eakashyap/product-review-sentiment-analyzer

Downloads: 102/mo · Text Classification · 67M params
Analyzes whether a product at a given price is considered good value by reviewers — can be used to flag "is this price actually a good deal?"


📊 CATEGORY 4: Loyalty Rewards Optimization
⚠️ Dedicated loyalty rewards models: 0 on HF
"Loyalty rewards" as a search term returned literally 0 models. This is a major gap. However, there are related models that solve the underlying problems:
Customer Churn Prediction (47 models) — closest proxy for loyalty:
AlbieCofie/predict-customer-churn
🔗 https://huggingface.co/AlbieCofie/predict-customer-churn

Predicts which customers are at risk of leaving — the flip side of loyalty. Use this to identify who should receive a retention offer/reward

mkManishKumar/Bank-Customer-Churn
🔗 https://huggingface.co/mkManishKumar/Bank-Customer-Churn

Banking/financial customer churn classifier

Cymosis/Telecom_Customer_Churn_Prediction
🔗 https://huggingface.co/Cymosis/Telecom_Customer_Churn_Prediction

Telecom-specific churn — works well for subscription loyalty scenarios

Zain78877/retail-recommendation
🔗 https://huggingface.co/Zain78877/retail-recommendation

The only dedicated retail recommendation model — November 2025, very recent


🏗️ How to Build a Complete Pipeline with These Models
Since no single "deal finder" model exists, here's how to stack what's available:
Email Deal Extraction Pipeline:

M-Arjun/SpamShield → classify incoming emails → flag "Marketing/Promotional"
snar7/email_body_extraction → extract clean text from email body
iamkhadke/invoice-extraction-v2-llama-2-7b-v2 → LLM prompt to pull discount %, expiry date, promo code, minimum spend
Store extracted deals in a database with timestamps

Receipt/Purchase Tracking Pipeline:

albertosei/layoutlmv3-receipt-parser → scan receipt image → extract all line items, prices, discounts
Compare line item prices against your historical database → flag price drops
eakashyap/product-review-sentiment-analyzer → cross-check if the deal is actually well-reviewed

Loyalty/Rewards Optimization:

AlbieCofie/predict-customer-churn → score each customer's loyalty risk
High-churn-risk customers → trigger rewards offer
Zain78877/retail-recommendation → suggest which products to include in the offer


📋 Honest Coverage Summary
Use CaseHF CoverageBest AvailablePromotional email classification✅ DecentM-Arjun/SpamShieldDeal NER from email text⚠️ IndirectInvoice extraction models (7B LLaMA)Receipt parsing / purchase tracking✅ Strongalbertosei/layoutlmv3-receipt-parser (89%)Price drop alerts (live)❌ NoneRequires external scraping + time-series DBPrice prediction (static)✅ DecentFlight price models, tabular regressionLoyalty rewards optimization❌ NoneChurn models as a proxyProduct recommendation⚠️ SparseZain78877/retail-recommendation only
The biggest gap is anything requiring live price monitoring — that needs a scraping infrastructure (e.g. Playwright/Puppeteer grabbing prices from Amazon/eBay pages) paired with a time-series database, with the HF models acting as the intelligence layer on top.
Want me to look into any of these more deeply, or search for a specific part of this stack?


CTF Solvers, Vulnerability Scanners & Network Analysis — Hugging Face Models
Three very strong categories here — let's go!

🏴 CATEGORY 1: CTF Solvers (117 Models)
This is surprisingly well-stocked. The best models are specialized security LLMs trained specifically on CTF-style challenges.
🥇 dangell7/Condor-27B ← Best CTF Brain
🔗 https://huggingface.co/dangell7/Condor-27B

Downloads: 958/mo · 27B params · BF16 Safetensors
What it is: A 27B security-reasoning fine-tune of Qwopus3.5-27B, distilled from Claude Opus reasoning traces on security topics — trained on 7,735 full reasoning chains derived from 35+ security books
CTF coverage: Binary exploitation, web/app vulnerabilities, kernel/OS internals, cryptography, network attacks, defensive analysis — full Chain-of-Thought reasoning per question
Best for: CTF challenge thought-partner — gives you a step-by-step reasoning chain, not just an answer
Trained on 8× H100 for 43 hours · MIT license
⚠️ Needs 2× A100 40GB to serve (large model) — use sglang with tp=2

🥇 RavichandranJ/Dolphin3-Cyber-8B-GGUF ← Best Local CTF Model
🔗 https://huggingface.co/RavichandranJ/Dolphin3-Cyber-8B-GGUF
(From our last session — 5,670 downloads/mo)

Runs on consumer hardware (GTX 1650+), covers full CTF workflow: pwn, rev, crypto, web, forensics

CTFanon/ctf_lora_v9
🔗 https://huggingface.co/CTFanon/ctf_lora_v9

LoRA adapter specifically for CTF challenges — lightweight, applies on top of a base model

pakawadeep suite (multiple models):

pakawadeep/mt5-small-finetuned-ctf1 — 3 downloads
pakawadeep/mt5-base-finetuned-ctf1 — 6 downloads
pakawadeep/mt5-large-finetuned-ctf1 — 5 downloads
pakawadeep/openthaigpt-1.0.0-beta-7b-lora-ctfl-th-gec-500 — 7B Thai-language CTF model (CTF + grammar error correction hybrid)
🔗 https://huggingface.co/pakawadeep


🔍 CATEGORY 2: Vulnerability Scanners / Code Vulnerability Detection
SUB-CATEGORY A: Code Vulnerability Detection (37 models)
🥇 mahdin70/codebert-devign-code-vulnerability-detector
🔗 https://huggingface.co/mahdin70/codebert-devign-code-vulnerability-detector

Downloads: 86/mo · Text Classification · CodeBERT (0.1B)
Trained on the Devign dataset (27,318 real C/C++ functions labeled vulnerable/not) — one of the most respected benchmarks in code vulnerability research
Fast binary classifier: feed a function → get vulnerable/safe label

mahdin70/unixcoder-code-vulnerability-detector
🔗 https://huggingface.co/mahdin70/unixcoder-code-vulnerability-detector

Downloads: 63/mo · UniXcoder architecture
Same Devign dataset but uses UniXcoder which understands both code structure AND text — generally outperforms plain CodeBERT on cross-lingual code

mahdin70/graphcodebert-devign-code-vulnerability-detector
🔗 https://huggingface.co/mahdin70/graphcodebert-devign-code-vulnerability-detector

Downloads: 26/mo · GraphCodeBERT — uses data flow graphs for deeper structural analysis

SUB-CATEGORY B: CWE-Specific Classifiers
The Vulnerability-Detection org has a suite of models classified by CWE type:
Vulnerability-Detection/cwe672-codeT5 + cwe672-codebert
🔗 https://huggingface.co/Vulnerability-Detection/cwe672-codeT5

CWE-672: Operation on a Resource after Expiration/Release (use-after-free class bugs)
60.8M params · 4–6 downloads/mo

Vulnerability-Detection/cwe754-codeT5 + cwe754-codebert
🔗 https://huggingface.co/Vulnerability-Detection/cwe754-codeT5

CWE-754: Improper Check for Unusual/Exceptional Conditions
Same CodeT5 and CodeBERT architecture pair

listlazarus/finetuned-codebert-vulnerability-detection
🔗 https://huggingface.co/listlazarus/finetuned-codebert-vulnerability-detection

Downloads: Dec 2024 · CodeBERT fine-tuned for vulnerability classification

listlazarus/finetuned-codet5-vulnerability-patching
🔗 https://huggingface.co/listlazarus/finetuned-codet5-vulnerability-patching

UNIQUE: Not just detection — this model generates patches for vulnerable code. Feed it a vulnerable function, get a fixed version back. CodeT5 generative architecture.

RayenLLM/Vulnerability_Detection_Using_CodeBERT
🔗 https://huggingface.co/RayenLLM/Vulnerability_Detection_Using_CodeBERT

Downloads: 6/mo · 0.1B · Updated Mar 2025

SUB-CATEGORY C: Smart Contract / Solidity Vulnerability Detection
msc-smart-contract-auditing/deepseek-coder-6.7b-vulnerabilities
🔗 https://huggingface.co/msc-smart-contract-auditing/deepseek-coder-6.7b-vulnerabilities

Downloads: 2/mo · 7B DeepSeek-Coder fine-tuned for smart contract auditing
Detects Solidity/EVM vulnerabilities: reentrancy, integer overflow, access control issues

angusleung100/CodeBERT-Base-Solidity-Vulnerability
🔗 https://huggingface.co/angusleung100/CodeBERT-Base-Solidity-Vulnerability

CodeBERT fine-tuned on Solidity vulnerability detection


🌐 CATEGORY 3: Network Analysis / Intrusion Detection
🥇 rdpahalavan/bert-network-packet-flow-header-payload ← Most Downloaded
🔗 https://huggingface.co/rdpahalavan/bert-network-packet-flow-header-payload

Downloads: 267/mo ← by far the most used network security model on HF
What it does: Classifies network packets into 23 attack categories from raw header+payload features
Categories covered: Analysis, Backdoor, Bot, DDoS, DoS, DoS GoldenEye, DoS Hulk, DoS SlowHTTPTest, DoS Slowloris, Exploits, FTP Patator, Fuzzers, Generic, Heartbleed, Infiltration, Normal, Port Scan, Reconnaissance, SSH Patator, Web Attacks (Brute Force, SQL Injection, XSS)
Architecture: DistilBERT (67M) · Text Classification · Apache 2.0
Trained on 1.7M packet flow samples

🥇 PHZane/TriCoAlign-0.5B ← SOTA on All Major NIDS Benchmarks
🔗 https://huggingface.co/PHZane/TriCoAlign-0.5B

Downloads: 10/mo · 0.5B (Qwen2.5-0.5B fine-tune) · MIT
SOTA Performance on NSL-KDD, CIC-IDS (CICIDS2017), and UNSW-NB15 simultaneously — outperforms Qwen2.5, GLM-4, ChatGPT-OSS and traditional ML models
What makes it unique: Solves the "LLM instability" problem where raw LLMs give different answers for identical network packets. Uses "TriCoAlign" framework with 3 alignment passes: Format Alignment → Thinking Alignment → Answer Alignment
Generates security-focused reasoning traces per classification — explainable AI for network security
Input is natural language description of network flow parameters → structured reasoning → classification
Lightweight enough for resource-constrained environments

gates04/DistilBERT-Network-Intrusion-Detection
🔗 https://huggingface.co/gates04/DistilBERT-Network-Intrusion-Detection

Downloads: 15/mo · DistilBERT 67M · Text Classification
General intrusion detection classifier, clean implementation

rdpahalavan/falcon-adapter-network-packet
🔗 https://huggingface.co/rdpahalavan/falcon-adapter-network-packet

PEFT adapter for network packet analysis — Falcon-based, describes packet behavior in natural language
Useful for generating human-readable explanations of suspicious traffic

frennn suite — 11 models (all no model cards):

frennn/Network-Anomaly-Detection-Qwen-3-8B-Fine-Tuned
frennn/Network-Anomaly-Detection-Gemma-3-4B-Fine-Tuned
frennn/Network-Anomaly-Detection-Gemma-2-9B-Instruct
frennn/Network-Anomaly-Detection-Llama-3.1-8B-Fine-Tuned
...and 7 more variants
🔗 https://huggingface.co/frennn
⚠️ No model cards — use with caution, but interesting multi-architecture coverage across Qwen3, Gemma3, LLaMA3 for network anomaly tasks


⚡ Complete CTF/Vuln/Network Automation Stack
RoleBest ModelSizeDownloadsCTF reasoning braindangell7/Condor-27B27B958/moLocal CTF assistantRavichandranJ/Dolphin3-Cyber-8B8B5,670/moCode vuln scannermahdin70/codebert-devign0.1B86/moCode vuln patcherlistlazarus/finetuned-codet5-patching0.2B—Smart contract auditmsc-smart-contract-auditing/deepseek-6.7b7B2/moNetwork packet IDSrdpahalavan/bert-network-packet67M267/moNIDS with reasoningPHZane/TriCoAlign-0.5B0.5B10/moNetwork LLM analysisfrennn/Network-Anomaly-Qwen-3-8B

Here is your document filled in with the specific Hugging Face models that match each numbered section — organized exactly to your outline:

AI Automated Takeoff (PDF, CAD & Plans) — Model Fills

1. OCR & Text Parsing — Reads dimensions like "10'-0''" and converts to 120"
What the AI uses:
🥇 microsoft/trocr-large-printed
🔗 https://huggingface.co/microsoft/trocr-large-printed

132,826 downloads/mo · 0.6B · Image-to-Text
Transformer-based OCR fine-tuned on SROIE (Scanned Receipt OCR) — reads printed text from scanned images with very high accuracy. Handles low-quality scans, rotated text, and dense document layouts. This is the engine that reads "10'-0''" off a scanned plan sheet.
Encoder: Vision Transformer / Decoder: RoBERTa
Fine-tunes: 7 domain-specific variants

microsoft/trocr-base-printed
🔗 https://huggingface.co/microsoft/trocr-base-printed

Lighter version — faster inference, good for high-volume page processing

For the Unit Conversion step ("10" → 120"):
After TrOCR extracts the text string "10'-0''", a text parsing / NLP model identifies the value (10) and the unit (feet/inches) and performs the arithmetic. The impira/layoutlm-document-qa model (below) can be prompted: "What is this dimension in inches?" and return 120.

2. Computer Vision & Pattern Recognition — Recognizes walls, doors, windows, electrical symbols
What the AI uses:
🥇 naver-clova-ix/donut-base
🔗 https://huggingface.co/naver-clova-ix/donut-base

150,036 downloads/mo · Image-to-Text · MIT · 478 fine-tune models downstream
OCR-free Document Understanding Transformer — reads the entire plan image as a visual input and outputs structured data directly without separate OCR. Trained on millions of document images including complex layouts. It "sees" the drawing holistically.
Base for fine-tuning on architectural symbols: train it on thousands of labeled floor plan images (walls, doors, electrical outlets, plumbing symbols) and it learns to recognize them contextually

zimhe/controlnet-wall-constrained-floorplan
🔗 https://huggingface.co/zimhe/controlnet-wall-constrained-floorplan

ControlNet model fine-tuned specifically on wall-constrained floor plan layouts — understands wall topology and differentiation from other line types (7 downloads, specialized research use)

Object Detection backbone (general):
The standard approach is YOLO or DETR fine-tuned on floor plan symbols — 154 floor plan models exist on HF under "floor plan", many based on DETR/Detectron2. Examples:

martinszu1999/floorplan — room/space segmentation
DEADBLOCK3000/floorplandataset — labeled floor plan dataset for fine-tuning your own detector


3. Vector and Raster Analysis (PDF/CAD Interpretation) — Interprets .dwg, .dxf, and scanned PDFs
What the AI uses:
🥇 microsoft/layoutlmv3-base
🔗 https://huggingface.co/microsoft/layoutlmv3-base

The LayoutLM series (6 models, 20 variations) are the industry standard for document AI — jointly model text, layout position, and visual features. They understand that text at coordinates (x=100, y=200) with a bounding box is a dimension label for the line below it.
This is the model that bridges vector data (bounding boxes from PDF parsing) with semantic text understanding
Collection page: https://huggingface.co/collections/microsoft/layoutlm-family

impira/layoutlm-document-qa
🔗 https://huggingface.co/impira/layoutlm-document-qa

78,313 downloads/mo · 0.1B · Document Question Answering · MIT
Fine-tuned on SQuAD2.0 + DocVQA — you give it a plan image and ask: "What is the scale?" or "What is the wall thickness on grid line A?" and it reads the document and answers
100 Spaces using it actively — proven in production
Usage: pipeline("document-question-answering", model="impira/layoutlm-document-qa")

For raw CAD/.DWG parsing:
There is no HF model that natively reads .dwg binary — this requires a pre-processing library (ezdxf in Python, or ODA File Converter) to convert .dwg/.dxf → SVG or raster, then pass to LayoutLMv3 or Donut. The AI model works on the converted output.

4. Spatial Reasoning & Scaling — Reads scale bar "1/4" = 1'-0"" and calculates true lengths
What the AI uses:
🥇 impira/layoutlm-document-qa (same model as above)

Directly answers spatial questions: "What is the scale of this drawing?" → "1/4" = 1'-0""
Once the scale is known, the AI multiplies measured pixel lengths by the scale factor to get real-world inches

microsoft/layoutlmv3-base fine-tuned for token classification

Extracts scale bar text as a named entity (tagged as SCALE_LABEL)
Extracts dimension strings as DIMENSION entities
After extraction: apply arithmetic: a line measuring 240px at 1/4"=1' scale → 240 × (48"/px_per_foot) = dimension in inches

For the construction domain specifically — Aphrodine/constructionai
🔗 https://huggingface.co/Aphrodine/constructionai

"The first open-weight LLM built for the construction industry" · 8B LLaMA 3.1 GGUF · 21 downloads/mo
Trained on 138,904 construction examples, 200 eval questions, 9 function-calling tools, 25+ trades, 90 scripts
Can be prompted: "This drawing is at 1/4" = 1'-0" scale. A wall measures 48 pixels. What is its length in inches?" and it understands the domain context to give 192 inches
Built by Strata Software Group for FairTradeWorker platform
Tags: construction, estimation, building-codes, cost-estimation, trades


5. Semantic Context & Document Understanding — Links door schedule to floor plan location
What the AI uses:
🥇 impira/layoutlm-document-qa (multi-page / cross-reference)

Ask cross-document questions: "Which door on the floor plan corresponds to Door Type D in the schedule?"
With proper page indexing, it resolves schedule-to-plan references

microsoft/layoutlmv3-base for Named Entity Recognition (NER)

Token classification identifies room labels, door tags, window marks, and structural notes as distinct entity types
Links callout bubbles (e.g., "3/A201") to the detail sheet they reference

schauh11/revit-coder-14b (from our prior CAD search)
🔗 https://huggingface.co/schauh11/revit-coder-14b

14B model trained on 177,000 Revit/BIM examples — understands the semantic relationships between Revit elements (walls, doors, families) at the model level, not just visually


Summary — Technology → Model Map
SectionTechnology NeededBest HF ModelDownloads/mo1 — OCR & Text ParsingTransformer OCRmicrosoft/trocr-large-printed132,8262 — Computer VisionDocument vision, symbol detectionnaver-clova-ix/donut-base150,0363 — Vector/Raster AnalysisLayout + position + text fusionimpira/layoutlm-document-qa78,3134 — Spatial Reasoning/ScalingDomain-aware LLM + layout NERAphrodine/constructionai215 — Semantic/Document ContextCross-reference NER + DocVQAmicrosoft/layoutlmv3-baseVery high (base model)CAD IntegrationRevit/BIM semantic understandingschauh11/revit-coder-14b—
The realistic pipeline is: TrOCR (read text off scans) → LayoutLMv3 (fuse position + text) → Donut (holistic plan understanding) → ConstructionAI (domain reasoning, unit conversion) → Revit-Coder (BIM-level semantic linking). Want me to search for any of the fine-tuning datasets for training your own version, or search for specific symbol detection models (electrical, plumbing, structural)?

LAYER 1: The Core Brain — General Code LLMs
These are the foundation models everything else builds on. Your AI needs one of these as its backbone.
🥇 Qwen/Qwen2.5-Coder-7B-Instruct ← Best All-Around
🔗 https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct

Downloads: 2,208,994/mo ← the most downloaded code model on all of HF
Size: 8B params · BF16 · Apache 2.0 · Live inference on Nscale
What it knows: Code generation, code reasoning, code fixing across all web languages — HTML, CSS, JavaScript, TypeScript, React, Vue, Node.js, Python (Django/Flask), SQL, and 90+ other languages
6 sizes available: 0.5B, 1.5B, 3B, 7B, 14B, 32B — pick based on your hardware
5,472 fine-tuned variants downstream

🥇 Qwen/Qwen2.5-Coder-32B-Instruct ← Best Quality
🔗 https://huggingface.co/Qwen/Qwen2.5-Coder-32B-Instruct

Downloads: 1,305,611/mo · 33B · 2,010 likes
Same family but larger — handles more complex multi-file app structures, API design, database schemas
Runs on inference providers (Nscale) if you don't have the hardware locally

deepseek-ai/deepseek-coder-7b-instruct-v1.5
🔗 https://huggingface.co/deepseek-ai/deepseek-coder-7b-instruct-v1.5

81,300 downloads/mo · 7B · 9-model collection
Excellent at multi-file project generation and filling in blanks (Fill-in-the-Middle / FIM) — useful when building out connected pages of a site

deepseek-ai/DeepSeek-Coder-V2-Instruct
🔗 https://huggingface.co/deepseek-ai/DeepSeek-Coder-V2-Instruct

79,200 downloads/mo · 236B (MoE) · 685 likes
Massive — needs serious hardware but produces near-GPT-4 quality code output

bigcode/starcoder2-15b
🔗 https://huggingface.co/bigcode/starcoder2-15b

11,500 downloads/mo · 15B · Trained on The Stack v2 (600+ programming languages)
Built by Hugging Face + ServiceNow + NVIDIA
Great for multi-language projects — knows HTML/CSS/JS/Python/PHP/SQL all in one model
20 fine-tuned variants, 13 adapters


🎨 LAYER 2: Frontend — HTML, CSS, JavaScript
kasperius/falcon-7b-sharded-bf16-finetuned-html-code-generation-the-css (v1–v5)
🔗 https://huggingface.co/kasperius/falcon-7b-sharded-bf16-finetuned-html-code-generation-the-css-only

Falcon 7B fine-tuned specifically for HTML + CSS generation — iteratively improved across 5 versions
Best for: generating styled static pages from text descriptions

Framework-specific: React / Vue / Next.js
The Qwen2.5-Coder and DeepSeek-Coder models handle React, Vue, and Next.js natively — no separate model needed. Just prompt with: "Build a React component with Tailwind CSS that does X" and they output production-quality code.

📸 LAYER 3: Screenshot / Design → Code (UI-to-Code)
This is the ability to look at a design mockup or screenshot and generate the code to recreate it.
ahmedashrafay/gemma-3-12b-screenshot-to-code-grpo
🔗 https://huggingface.co/ahmedashrafay/gemma-3-12b-screenshot-to-code-grpo

Gemma 3 12B fine-tuned with GRPO reinforcement learning for screenshot-to-code conversion
Updated March 2025 — one of the most recent models for this task

ahmedashrafay/screenshot-to-code-grpo
🔗 https://huggingface.co/ahmedashrafay/screenshot-to-code-grpo

Smaller companion model for screenshot-to-code, same GRPO training approach

What these do: Give the model an image of a webpage design → it outputs HTML/CSS/React code that recreates it. This is the equivalent of Figma → code automation.

🔧 LAYER 4: Full-Stack & Backend
For building complete apps (not just front-end pages), your AI needs to understand:

APIs & Routes (REST, GraphQL)
Databases (SQL, MongoDB schema design)
Authentication (JWT, OAuth flows)
Server-side logic (Node.js, Python Flask/Django, FastAPI)

All of the Qwen2.5-Coder and DeepSeek-Coder models handle this. For fine-tuned backend-specific models:
TheBloke/deepseek-coder-33B-instruct-GGUF
🔗 https://huggingface.co/TheBloke/deepseek-coder-33B-instruct-GGUF

40,500 downloads/mo · 33B GGUF quantized — runs locally
Full-stack capable: generates complete Express.js servers, Django backends, database models, and connects them to frontends


📱 LAYER 5: Mobile App Development
For building iOS, Android, and cross-platform apps, the same code LLMs apply but you'll want models with mobile-language training:
Swift / iOS:
The Qwen2.5-Coder series has solid Swift coverage. For dedicated Swift training, search HF for swift — 400+ models exist.
React Native / Flutter:
DeepSeek-Coder-V2 and Qwen2.5-Coder-32B both generate React Native and Flutter code well from prompts.
bigcode/starcoder2-3b
🔗 https://huggingface.co/bigcode/starcoder2-3b

90,100 downloads/mo · 3B — small enough to run anywhere, trained on Kotlin, Swift, Dart (Flutter), and JavaScript


🏗️ What Your AI Needs to LEARN (Training Data & Fine-Tuning)
The base models above are great out-of-the-box, but if you want your AI to build YOUR specific style of website or app, you need to fine-tune it. Here's what that requires:
Training Data Your AI Needs:

Pairs of prompts + complete working HTML/CSS/JS code
Real GitHub repositories (The Stack v2 dataset — 600+ languages, 900GB of code)
Website screenshots paired with their source code (for UI-to-code)
Your own example projects ("build a checkout page like this one")

Fine-Tuning Approaches:

LoRA/QLoRA — cheapest, runs on consumer GPU, adapts the model to your coding style
Full fine-tune — expensive but powerful, used for domain-specific app frameworks
Instruction tuning — teach the model to follow prompts like "build a login page with dark mode"


⚡ Recommended Stack by Use Case
GoalModel to UseSizeDownloads/moBuild any website from textQwen2.5-Coder-7B-Instruct8B2.2MHigh-quality complex appsQwen2.5-Coder-32B-Instruct33B1.3MDesign → code from screenshotahmedashrafay/gemma-3-12b-screenshot-to-code12B—Local use (laptop/desktop)Qwen2.5-Coder-1.5B-Instruct-GGUF2B75.9kMulti-language projectbigcode/starcoder2-15b15B11.5kDeepest code qualityDeepSeek-Coder-V2-Instruct236B79.2k

🎯 The Simple Answer
If you want your AI to learn to build websites and apps right now, start with Qwen2.5-Coder-7B-Instruct — it already knows HTML, CSS, JavaScript, React, Node.js, Python, SQL and more out of the box. Run it locally with Ollama (ollama run qwen2.5-coder:7b) and start prompting. Fine-tune it later on your own example projects to match your specific style.
Want me to search for fine-tuning datasets, specific framework models (like Next.js or Django-specific), or models for a particular type of app?

🎨🎬 Image & Video Generation Models on Hugging Face
Great topic — this is one of the most active areas on HuggingFace right now. Here's the full breakdown with a very important honest split: what you can run yourself vs what's commercial-only.

⚠️ HONEST ASSESSMENT FIRST: Commercial-Only Platforms
Several tools you listed have NO open-source models on HuggingFace — they are closed, subscription-based APIs. Here's the breakdown:
PlatformHF Presence?StatusGoogle Gemini (image/video)❌ No weightsCommercial API only — google.com/geminiGoogle Veo 3.1❌ No weightsCommercial API only — Google AI StudioRunway Gen-4.5❌ No weightsCommercial API only — runwayml.comLuma Dream Machine❌ No weightsCommercial API only — lumalabs.aiPika 2.5❌ No weightsCommercial API only — pika.artVidu Q3❌ No weightsCommercial API only — vidu.studioDaVinci AI❌ No weightsCommercial platform — no HF presenceNano Banana❌ No weightsNo HF presence found (69 results for "kling", none matched)Kling AI 3.0 video⚠️ PartialKling AI's video generation is commercial-only (kling.kuaishou.com), BUT KlingTeam has open research models on HF (see below)
These are all paid services you access via API or web UI — you cannot download and run them locally.

✅ WHAT YOU CAN RUN: Open-Source Image Generation on HuggingFace
95,085 text-to-image models on HuggingFace — here are the top ones:
🏆 Tier 1 — The Big Players
black-forest-labs/FLUX.1-dev — 734k dl/mo

https://huggingface.co/black-forest-labs/FLUX.1-dev
The state-of-the-art open-weight image model as of 2025. 12B parameters, flow-matching architecture. Photorealistic, prompt-adherent, massive community (36,833 adapters/LoRAs built on it). Non-commercial license — requires agreeing to share contact info.
36,833 adapters + 569 finetunes + 69 quantizations available

black-forest-labs/FLUX.1-schnell — 717k dl/mo

https://huggingface.co/black-forest-labs/FLUX.1-schnell
The free/fast FLUX variant. Apache 2.0 license = fully commercial use allowed. 4-step inference (fast!). Same quality architecture as FLUX.1-dev but optimized for speed. 267 adapters available.

stabilityai/stable-diffusion-xl-base-1.0 — 2.04M dl/mo

https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0
The most downloaded image model on all of HuggingFace. SDXL is still the community workhorse — massive ecosystem of LoRAs, ControlNets, and fine-tunes. 1024x1024 native resolution.

Tongyi-MAI/Z-Image-Turbo — 1.33M dl/mo

https://huggingface.co/Tongyi-MAI/Z-Image-Turbo
Alibaba's fast turbo image model. Very high downloads, great for rapid generation.

stabilityai/sdxl-turbo — 896k dl/mo

https://huggingface.co/stabilityai/sdxl-turbo
Real-time image generation (single-step). Great for live/interactive pipelines where speed matters.

🎯 Tier 2 — Stable Diffusion 3 Series
stabilityai/stable-diffusion-3.5-medium — 275k dl/mo

https://huggingface.co/stabilityai/stable-diffusion-3.5-medium
SD3.5 Medium is the sweet spot — high quality but runs on consumer GPUs (8GB VRAM). Improved text rendering over SDXL.

stabilityai/stable-diffusion-3.5-large — 51.3k dl/mo

https://huggingface.co/stabilityai/stable-diffusion-3.5-large
The full-power SD3.5 model. Better quality but needs more VRAM (16GB+).

stabilityai/stable-diffusion-3-medium-diffusers — 82k dl/mo

https://huggingface.co/stabilityai/stable-diffusion-3-medium-diffusers
SD3 original release. 2B parameter multimodal diffusion transformer.

🎨 Specialized Image Models
lightx2v/Qwen-Image-Lightning — 327k dl/mo

https://huggingface.co/lightx2v/Qwen-Image-Lightning
Qwen-based fast image lightning model — part of Alibaba's Qwen family

CompVis/stable-diffusion-v1-4 — 621k dl/mo

https://huggingface.co/CompVis/stable-diffusion-v1-4
The original SD 1.4 — still massively used as a base for fine-tuning pipelines

stabilityai/sd-turbo — 609k dl/mo

https://huggingface.co/stabilityai/sd-turbo
Fastest SD model available — single-step generation for near real-time use


✅ WHAT YOU CAN RUN: Open-Source Video Generation on HuggingFace
1,208 text-to-video + 952 image-to-video models on HuggingFace — here are the best:
🏆 Text-to-Video Top Models
Wan-AI/Wan2.2-T2V-A14B-Diffusers — 89.9k dl/mo

https://huggingface.co/Wan-AI/Wan2.2-T2V-A14B-Diffusers
The current #1 open text-to-video model. 14B parameters, Apache 2.0 license. State-of-the-art quality for locally runnable video gen. 19 quantizations + 5 finetunes available. Supported by fal.ai inference providers.

Wan-AI/Wan2.1-T2V-14B — 51.9k dl/mo

https://huggingface.co/Wan-AI/Wan2.1-T2V-14B
Previous Wan generation — still great, runs on WaveSpeed, Replicate, fal. 68 adapters + 57 finetunes. Apache 2.0 license.

Wan-AI/Wan2.1-T2V-1.3B-Diffusers — 138k dl/mo (highest!)

https://huggingface.co/Wan-AI/Wan2.1-T2V-1.3B-Diffusers
The lightweight Wan model — 1.3B params, runs on consumer hardware (8GB VRAM). Best for automation pipelines where you need fast video.

zai-org/CogVideoX-5b — 39.5k dl/mo

https://huggingface.co/zai-org/CogVideoX-5b
CogVideoX from Zhipu AI (backed by THUDM/Tsinghua). 5B params, good for cinematic video. "Other" license — check terms. 4 adapters, 27 finetunes available.

ali-vilab/text-to-video-ms-1.7b — 78.8k dl/mo

https://huggingface.co/ali-vilab/text-to-video-ms-1.7b
Alibaba's ModelScope text-to-video. Smaller (1.7B) and one of the original open video models. 658 likes — very established/mature.

🏆 Image-to-Video Top Models (animate your images!)
Lightricks/LTX-2.3 — 🔥 1,722,140 dl/mo — #1 most downloaded video model on all of HF

https://huggingface.co/Lightricks/LTX-2.3
DiT-based audio-video foundation model. Generates synchronized video AND audio in one model. Updated March 2025. Supports text-to-video, image-to-video, video-to-video, audio-to-video, and more. 37 adapters + 43 finetunes available.

Lightricks/LTX-Video — 472k dl/mo

https://huggingface.co/Lightricks/LTX-Video
"First DiT-based video model capable of generating high-quality videos in real-time." 30 FPS at 1216×704 resolution — faster than real-time playback. Supported by fal.ai. 2.17k likes.

lightx2v/Wan2.2-Distill-Loras — 1.24M dl/mo

https://huggingface.co/lightx2v/Wan2.2-Distill-Loras
Distilled LoRAs for Wan2.2 image-to-video. Very popular for lighter/faster pipeline.

stabilityai/stable-video-diffusion-img2vid-xt — 345k dl/mo

https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt
Stability AI's image-to-video model. Takes any still image and animates it. 3.29k likes — one of the most loved video models.

🎯 Kling AI Research Models (Partial HF Presence)
KlingTeam/LivePortrait — 5,495 dl/mo

https://huggingface.co/KlingTeam/LivePortrait
Open research model from Kuaishou (Kling's parent company). Portrait animation with stitching + retargeting. MIT license. 18k GitHub stars. 83 HF Spaces running it. This is the OPEN version of their portrait tech.

KlingTeam/HumanAesExpert-1B — 9.09k dl/mo

https://huggingface.co/KlingTeam/HumanAesExpert-1B
Human aesthetics quality scoring model from KlingTeam. Use this to automatically rate and filter generated images for human-subject quality. Very useful in a pipeline.


📊 Summary: HF Availability at a Glance
Category# of Models on HFBest Starting PointText-to-Image95,085FLUX.1-schnell (free) or SDXLText-to-Video1,208Wan2.1-T2V-1.3B (light) or Wan2.2-T2V-A14B (quality)Image-to-Video952LTX-2.3 (1.7M dl/mo!)Portrait Animation69 (kling search)KlingTeam/LivePortraitImage Scoring/QualityVariousKlingTeam/HumanAesExpert-1B

💡 Practical Recommendations for Your AI Platform
For image generation: Start with FLUX.1-schnell (Apache 2.0, fast, free commercial use) via Diffusers or ComfyUI. Add SDXL as your workhorse with LoRAs for style control.
For video generation: Use Wan2.1-T2V-1.3B for speed (consumer GPU), Wan2.2-T2V-A14B for quality, or LTX-2.3 for image-to-video with audio.
For accessing commercial platforms (Runway, Pika, Kling video, Luma, Veo) in your AI system: You'll need their paid APIs — there are no downloadable weights. Budget accordingly — most run $0.01–$0.10 per second of generated video.

Sora & Midjourney on HuggingFace — Honest Breakdown

🎬 SORA (OpenAI)
❌ The real Sora = NOT on HuggingFace. Period.
OpenAI's Sora is 100% closed-source, commercial-only. No weights, no architecture release, no API (as of 2025). You access it only through sora.com with an OpenAI subscription. There is absolutely no open version, and OpenAI has shown no intention of releasing it.
✅ BUT — there IS an open-source alternative built specifically to replicate it:
hpcai-tech/Open-Sora-v2 — 976 dl/mo

https://huggingface.co/hpcai-tech/Open-Sora-v2
Apache 2.0 license — fully free and commercial use allowed
Mission statement: "Democratizing Efficient Video Production for All" — built explicitly as an open Sora alternative
Text-to-video, trained with published research papers (arxiv:2503.09642 + 2412.20404)
GitHub repo at github.com/hpcaitech/Open-Sora
Honest note: download count is modest (976/mo), meaning the community has largely moved on to Wan2.2 and LTX for quality. Open-Sora is more of a research/educational reference than a production-ready tool. Use Wan2.2 for production video.


🎨 MIDJOURNEY
❌ The real Midjourney = NOT on HuggingFace. Ever.
Midjourney has never open-sourced a single model weight. It's Discord/web-only at midjourney.com, and the founders have explicitly stated they have no plans to release models. No API access either — it's purely a subscription service.
✅ BUT — HuggingFace has 392 "Midjourney" models — these fall into two very useful categories:
Category 1: Models that MIMIC Midjourney's visual style (LoRAs for FLUX)
strangerzonehf/Flux-Midjourney-Mix2-LoRA — 1,380 dl/mo

https://huggingface.co/strangerzonehf/Flux-Midjourney-Mix2-LoRA
A LoRA fine-tune layered on top of FLUX.1-dev that replicates Midjourney v6's aesthetic — the signature rich colors, cinematic lighting, and sharp detail MJ is known for. 546 likes. Supported on fal.ai. Built-in example prompt: "MJ v6, Portrait photography..." — it literally uses MJ v6 style prompts.
Use this with FLUX.1-dev to get Midjourney-like results locally for free.

brushpenbob/flux-midjourney-anime — 270 dl/mo

https://huggingface.co/brushpenbob/flux-midjourney-anime
FLUX LoRA that mimics Midjourney's anime style specifically. Good for stylized character art.

Category 2: AI Models that WRITE Midjourney prompts for you (prompt generators)
This is actually extremely useful for your AI platform — you can automate prompt creation:
rvian/gguf-lora-llama3-midjourney-prompt-generator — 6,366 dl/mo

https://huggingface.co/rvian/gguf-lora-llama3-midjourney-prompt-generator
8B Llama 3 fine-tuned specifically to generate high-quality Midjourney-style prompts. Apache 2.0. GGUF format (runs in Ollama). 4.92GB at Q4. You give it a rough idea, it outputs a polished MJ-ready prompt. This is the #1 most downloaded Midjourney-related model on all of HF.

jzdesign/midjourney_prompt_improver_lora_model — 2,001 dl/mo

https://huggingface.co/jzdesign/midjourney_prompt_improver_lora_model
3B Llama LoRA for improving and refining existing prompts into full MJ-optimized prompts. Apache 2.0. Only 2GB. Great for a pipeline step where you take a user's rough description and polish it before sending to any image model.


💡 Practical Strategy for Your AI Platform
Since both Sora and Midjourney are closed, here's how to replicate them open-source:
To replicate Midjourney: Run FLUX.1-schnell (base model, Apache 2.0) + Flux-Midjourney-Mix2-LoRA on top. Feed prompts through rvian/gguf-lora-llama3-midjourney-prompt-generator first to auto-write great prompts. This gives you a fully automated Midjourney-style pipeline locally at zero per-image cost.
To replicate Sora: Use Wan2.2-T2V-A14B for highest quality video, Wan2.1-T2V-1.3B for speed, or LTX-2.3 for image-to-video. Open-Sora-v2 exists but the Wan series outperforms it currently.

 Reverse Engineering Models on HuggingFace
Honest up-front: this is one of the thinner categories on HuggingFace — only 4 models match "reverse engineering" directly. BUT when you search by the actual sub-tasks of RE (decompiling, binary analysis, vulnerability in IR code, code similarity), you find solid, real tools. Here's the full breakdown by RE task:

🏆 TIER 1 — The Crown Jewel: LLM4Decompile Family
66 models in this family — the most purpose-built RE models on all of HuggingFace. These are specifically designed to turn x86 assembly → readable C code, which is the core of binary reverse engineering.
LLM4Binary/llm4decompile-6.7b-v1.5 — 1,138 dl/mo ⭐ Best starting point

https://huggingface.co/LLM4Binary/llm4decompile-6.7b-v1.5
MIT license. 7B params. Trained on 15B tokens. Takes raw objdump assembly output and decompiles it back to C. Handles all GCC optimization levels (O0–O3). On the HumanEval-Decompile benchmark it scores 68% re-executability at O0 — outperforms GPT-4o (which only gets 30%). Workflow: compile with GCC → objdump → feed asm to model → get C back.

LLM4Binary/llm4decompile-9b-v2 — 604 dl/mo ⭐ Best quality (Ghidra integration)

https://huggingface.co/LLM4Binary/llm4decompile-9b-v2
MIT license. 9B params. V2 works differently: uses Ghidra headless first to get pseudo-code, then the model refines that pseudo-code into clean C. This two-stage approach beats pure assembly decompilation — 74.4% re-executability at O0. Best pipeline for serious RE work. 6 quantizations available.

LLM4Binary/llm4decompile-6.7b-v2 — 562 dl/mo

https://huggingface.co/LLM4Binary/llm4decompile-6.7b-v2
Same V2 Ghidra-refinement approach, lighter weight (6.7B). Good balance of speed and quality.

LLM4Binary/llm4decompile-1.3b-v2 — 573 dl/mo

https://huggingface.co/LLM4Binary/llm4decompile-1.3b-v2
1B lightweight version — runs on CPU or weak GPU. For fast/automated pipelines where full quality isn't needed.

GGUF quantized versions (run in Ollama):

mradermacher/llm4decompile-22b-v2-i1-GGUF — 1,340 dl/mo (largest, 22B, best quality)
RichardErkhov/LLM4Binary_-_llm4decompile-22b-v2-gguf — 612 dl/mo


🔩 TIER 2 — Purpose-Built Decompiler Models
tarball0/ELF-Decompiler-GGUF — 278 dl/mo

https://huggingface.co/tarball0/ELF-Decompiler-GGUF
Qwen2-based 8B model fine-tuned specifically on ELF Linux binaries. GGUF format — runs in Ollama/llama.cpp. 4.68GB at Q4_K_M. Comes with a full inference script on GitHub. Great for Linux binary analysis automation.

raafatabualazm/decompiler-v5 — 113 dl/mo

https://huggingface.co/raafatabualazm/decompiler-v5
Qwen3-based 8B decompiler. Multiple versions (v1 through v6) on HF. Research paper attached (arxiv:1910.09700). Not much documentation but actively maintained through 2025.


🧬 TIER 3 — Binary Vulnerability Detection in IR/Code
Chidamtek/bert-llvm-ir-vulnerability-detection — 9 dl/mo

https://huggingface.co/Chidamtek/bert-llvm-ir-vulnerability-detection
BERT classifier that detects vulnerable vs. non-vulnerable code slices in LLVM IR. This is powerful for RE pipelines — you decompile a binary → lift to LLVM IR → run this classifier to flag dangerous code paths. Tags: static-analysis, cybersecurity, llvm-ir. MIT license. 0.1B params (tiny and fast).

neuralsentry/vulnerabilityDetection-StarEncoder-Devign — 46 dl/mo

https://huggingface.co/neuralsentry/vulnerabilityDetection-StarEncoder-Devign
StarEncoder-based vulnerability detector trained on the Devign dataset (C/C++ function-level vuln detection). Direct pipeline: decompiled C → flag vulnerable functions.


🎯 TIER 4 — RE-Adjacent Utility Models
raafatabualazm/decompiler-v1 — inference available ⚡

https://huggingface.co/raafatabualazm/decompiler-v1
4B lighter version, supports HF Inference API — you can call it directly without running locally.

Anubis97/Reverse_Engineering_SmolLM2-135M — 4 results search match

https://huggingface.co/Anubis97/Reverse_Engineering_SmolLM2-135M
A tiny (135M) SmolLM2 fine-tuned specifically for RE Q&A. Very small footprint — good for an assistant that answers RE questions in a larger pipeline.


⚠️ Honest Gap Assessment
RE Sub-TaskHF CoverageVerdictx86 Assembly → C decompilation✅ 66 models (LLM4Decompile)Good — real tools existELF Linux binary analysis✅ tarball0/ELF-DecompilerDecentLLVM IR vulnerability detection✅ bert-llvm-irThin but functionalMalware analysis❌ 0 modelsGap — none on HFFirmware analysis❌ 0 modelsGap — none on HFProtocol reverse engineering❌ 0 modelsGapPE/Windows binary analysis❌ 0 modelsGapSymbol recovery / function naming⚠️ 2 models (phi-2 function ID)Very sparse

💡 Recommended RE Automation Stack
For a full automated reverse engineering pipeline on your AI platform:
Step 1 — Disassemble: Use objdump (Linux) or Ghidra headless (cross-platform) to get assembly/pseudo-code from the binary.
Step 2 — Decompile: Feed to LLM4Binary/llm4decompile-9b-v2 (Ghidra route, best quality) or llm4decompile-6.7b-v1.5 (pure asm route, simpler).
Step 3 — Analyze: Run Chidamtek/bert-llvm-ir-vulnerability-detection on the decompiled output to flag vulnerable functions automatically.
Step 4 — Q&A / Explain: Layer in Dolphin3-Cyber-8B (from your earlier cybersecurity session) to answer questions about what the decompiled code does.

=========================================================================================================================

Honest Deep Dive: Building a 32B-Based AI Platform with Stacked Capabilities
Let me give you the real engineering answer, not hype. I'll address your Granite experience directly, then lay out exactly what a stacked architecture looks like.

⚠️ First: Why Granite and GPT Hallucinated on You — The Honest Reason
Every LLM hallucates. Period. The difference is how much and why. What you caught is a fundamental truth about how these models work — they generate the most statistically probable next token, not the most true next token. Granite specifically is IBM's enterprise model focused on code and structured tasks — it's not a general reasoning powerhouse. GPT-4 hallucinates confidently because it's trained to sound authoritative. This never fully goes away, but it can be engineered around. That's the entire point of a stacked architecture.

🏗️ THE FOUNDATION: Choosing Your 32B Base
Based on actual HuggingFace download numbers and honest benchmarks, here are your real options:
Option 1 — Qwen/Qwen3-32B — 4,833,719 dl/mo ⭐ RECOMMENDED

https://huggingface.co/Qwen/Qwen3-32B
Apache 2.0 license. The #1 most downloaded 32B model on all of HuggingFace right now. 119k context window, native tool/function calling, 100+ language support, built-in "thinking mode" (chain-of-thought) that you can turn on/off per request. Beats GPT-4o on multiple benchmarks at 32B size. This is your best base.

Option 2 — deepseek-ai/DeepSeek-R1-Distill-Qwen-32B — 1,072,855 dl/mo

https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B
MIT license. This is a reasoning model — distilled from DeepSeek-R1 (which beat o1 on math). Best for tasks requiring multi-step logical reasoning. The tradeoff: it's slower because it thinks step-by-step. Outstanding for construction takeoffs, code analysis, math-heavy tasks.

Option 3 — Qwen/Qwen2.5-32B-Instruct — 611k dl/mo

https://huggingface.co/Qwen/Qwen2.5-32B-Instruct
Apache 2.0. The previous Qwen generation — extremely stable, massive community, tons of fine-tunes already built on it. If you want the most fine-tuning options and community support, this is it.

What about Granite? IBM Granite (400k/mo for the micro version) is genuinely good for code generation and structured enterprise tasks, but it's not competitive with Qwen3-32B for general reasoning. Your hallucination test was valid — it's not the right base for a broad platform.
Honest Bottom Line on Base Choice: Start with Qwen3-32B. It has the best combination of capability, license freedom, context length, and community support at the 32B scale.

🧱 THE STACK: What Gets Layered On Top
This is the architecture that separates a "chat bot" from an AI platform. Think of it in layers, each solving a specific problem:

LAYER 1 — Anti-Hallucination Infrastructure (Most Critical)
This is what fixes the problem you caught. The 32B model alone will still lie to you. These layers make it tell the truth.
Embeddings — Retrieval Foundation
sentence-transformers/all-MiniLM-L6-v2 — 234M dl/mo

https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2
Converts text into semantic vectors so your system can search real knowledge (your documents, databases, live data) instead of relying on the model's memory. This is the backbone of RAG (Retrieval Augmented Generation) — the #1 technique for killing hallucinations.

BAAI/bge-m3 — 20.5M dl/mo

https://huggingface.co/BAAI/bge-m3
More powerful multilingual embeddings. Use this for production-grade semantic search across all your domain knowledge (construction docs, shopping data, security feeds).

Rerankers — Precision Filter
BAAI/bge-reranker-v2-m3 — 9.88M dl/mo

https://huggingface.co/BAAI/bge-reranker-v2-m3
After the embeddings retrieve 20 candidate documents, the reranker picks the best 3 to actually send to the 32B model. Massively improves accuracy. Without this, your RAG pipeline floods the model with noise.

Qwen/Qwen3-Reranker-0.6B — 1.52M dl/mo

https://huggingface.co/Qwen/Qwen3-Reranker-0.6B
Qwen's native reranker — perfect pairing with Qwen3-32B as the base. 0.6B so it's fast and cheap to run alongside the main model.

How RAG kills hallucination in practice: User asks a question → embeddings search your real knowledge base → reranker picks best results → those results are injected into the 32B model's context as ground truth → model answers from facts, not memory. When the model says something that isn't in the retrieved context, you flag it.

LAYER 2 — Domain Expert Adapters (Your Specialty Modules)
Rather than trying to bake everything into one giant fine-tune, you attach small specialized models as "experts" that the orchestrator routes to. All of these from your existing list plug directly in:
Construction/Engineering Domain:
Aphrodine/constructionai + schauh11/revit-coder-14b + microsoft/layoutlmv3-base + microsoft/trocr-large-printed
These don't replace the 32B base — they handle the specific subtask (reading a PDF floorplan, understanding Revit structure) and return structured data to the 32B for reasoning.
Shopping/Commerce Domain:
LiYuan/amazon-review-sentiment-analysis + niksss/xlm-roberta-large-finetuned-ebay + thebajajra/RexReranker-0.6B + McGill-NLP/Llama-3-8B-Web
Route shopping queries through these classifiers and rankers before the 32B sees them, so the 32B gets pre-processed structured input.
Cybersecurity Domain:
RavichandranJ/Dolphin3-Cyber-8B-GGUF as a specialized security sub-model. When a security query comes in, Dolphin3 analyzes it first, the 32B reasons about the findings.
Code/Dev Domain:
Qwen/Qwen2.5-Coder-32B-Instruct — you can actually run this as a parallel specialist. When a coding task comes in, route to the coder variant, not the general 32B.

LAYER 3 — Memory and Context Management
This is what makes your system learn and remember vs. being stateless.
Short-term (conversation) memory: Built into your serving framework (vLLM or Ollama with --ctx-size 32768). The 32B model's 119k context window holds the full active conversation.
Long-term (persistent) memory: A vector database (ChromaDB, Qdrant, or Weaviate — all free/open source) stores embeddings of past interactions and domain knowledge. Every important fact gets embedded and stored. When a new query comes in, relevant memories are retrieved and injected. This is how your system "learns" without retraining.
Structured data memory: A regular SQLite or PostgreSQL database for hard facts — prices, measurements, user preferences, transaction history. The 32B model can query this via tool calls and gets exact answers instead of guessing.

LAYER 4 — Tool Use / Agent Framework (The Autonomy Layer)
This is what lets your AI do things instead of just talk about things.
Qwen3-32B has native tool/function calling built in — this is a massive advantage. You define tools as JSON schemas (e.g., search_web, query_database, run_python, call_api) and the model decides when to use them and what parameters to pass. No special fine-tuning needed.
For orchestrating multiple specialist models together, the open-source frameworks are LangChain (most popular, Python), LlamaIndex (best for RAG-heavy workflows), or Haystack (best for production document pipelines). None of these are HuggingFace models — they're Python libraries that wrap your models.

LAYER 5 — Quality Control (The Anti-Lie Layer)
This is the hardest layer and where most people skip corners. Don't.
Confidence scoring: Force the model to output a confidence level with every answer. Qwen3's thinking mode lets you inspect its reasoning chain — if the chain is short or circular, the answer is likely hallucinated.
Cross-verification: For critical facts (prices, measurements, legal claims), your system runs the query through two paths — the 32B model AND a RAG retrieval — then compares. If they disagree, flag for human review or refuse to answer.
Citation enforcement: Instruct the model via system prompt that every factual claim must cite a source from the retrieved context. If it can't cite it, it must say "I don't know" instead of guessing. This is a prompt engineering technique, not a model addition, but it's the single most effective anti-hallucination tool after RAG.
Factuality classifiers: neuralsentry/vulnerabilityDetection-StarEncoder-Devign style approach — you can fine-tune a small BERT classifier on your domain to flag outputs that contain known-false patterns. This is advanced but very effective.

📐 The Honest Architecture Diagram (in text)
USER QUERY
    ↓
[ROUTER] — classifies query type (shopping/construction/security/code/general)
    ↓
[DOMAIN PRE-PROCESSOR] — runs specialized small model for that domain
    ↓
[RAG RETRIEVAL] — embeddings search your knowledge base (bge-m3)
    ↓
[RERANKER] — picks best 3 context chunks (bge-reranker-v2-m3)
    ↓
[32B BASE MODEL — Qwen3-32B] — reasons over query + context + domain pre-processing
    ↓
[TOOL CALLS if needed] — web search, database, APIs, Python execution
    ↓
[QUALITY GATE] — confidence check, citation check, cross-verify
    ↓
OUTPUT TO USER

⚠️ What This Stack Can't Do — Honest Gaps
A 32B model running locally on consumer hardware (like a 3090 or 4090) will run at 4-bit quantized, meaning roughly 20–25GB VRAM. That's achievable but slow — expect 15–30 tokens/second. For a production platform serving multiple users, you either need A100/H100 class hardware, cloud inference (Groq runs Qwen3-32B free), or you drop to a quantized version.
The specialized models in your list (social media scrapers, live price monitoring, real-time web access) cannot be solved by any HuggingFace model alone — they all require live API calls, scrapers, or browser automation that your agent layer handles, not the LLM itself.
No amount of stacking eliminates hallucination entirely. It reduces it dramatically — a well-built RAG+reranker+citation pipeline can cut hallucination rates by 80–90% on factual questions — but edge cases will always exist. Design your UI so users know when to verify.

🎯 Recommended Starting Build Order
Start here — don't try to build everything at once:

Stand up Qwen3-32B via Ollama locally (or free on Groq API)
Add bge-m3 embeddings + ChromaDB vector store — this gives you RAG on your own documents
Add bge-reranker-v2-m3 — plug into your RAG pipeline
Write a system prompt that enforces citation and "I don't know" responses
Add your first domain adapter (start with whichever domain you know best to test quality)
Build the router last — once you have 2+ working domains, the routing logic becomes obvious

This gives you a genuinely honest, grounded AI platform where you can verify every answer against source material — the exact opposite of what you caught Granite and GPT doing.



DANCONI AI — Honest Architecture Deep-Dive
How the Researched Models Become Learned Capabilities, Not External Pipes
I read your Grand Plan v1.5 in full. Your philosophy is correct and your instinct about RAG is right for this use case — d'Anconi IS tools. The distinction you're drawing is between a model that knows how to do the work versus one that looks up answers at runtime. Here's the honest engineering answer.

What "Stacking as Learning" Actually Means
The models from your research sessions are not plugged in as co-processors. They serve three distinct roles in your architecture:
Role 1 — Training data distillation. You run these models against real examples in your domain, collect their outputs, filter for quality, and use the best outputs as training examples for your LoRA adapter. The specialized model is a data generator, not a runtime dependency. After training, it's gone — d'Anconi learned what it knew.
Role 2 — Output format templates. You study how models like layoutlmv3-receipt-parser or bert-network-packet-flow structure their outputs, then train d'Anconi to produce that same structured output format natively. You clone the interface, not the weights.
Role 3 — Inference-time tool calls (the wrapper-first doctrine). For tasks that genuinely require a specialized architecture — e.g., OCR on a raw binary image stream — you call the specialist model as a Python function from d'Anconi's tool registry. d'Anconi decides when and how, the specialist executes, and d'Anconi reasons over the result. This is your wrapper-first pattern, not RAG.

Honest Assessment of Your Foundation Choice
Qwen2.5-32B-Instruct (the 62GB BF16 you have on disk) is confirmed as the right call. 611k dl/mo, Apache 2.0, native tool/function calling, 128k context, strong multilingual, strong code. The Unsloth 4-bit version (unsloth/Qwen2.5-32B-bnb-4bit, 21k dl/mo) is what Unsloth targets for QLoRA training — this is the form factor you load for training runs on the RTX 3090/4090.
Honest limitation you need to know: Qwen2.5-32B was released September 2024. At 32B it outperforms GPT-3.5 comfortably and is competitive with early GPT-4. But it does not match GPT-4o or Claude Sonnet on open-ended reasoning. Your QLoRA adapters will sharpen it for specific domains and narrow that gap within those domains. Outside your trained verticals, it will still occasionally drift. This is not fixable without a larger base or a better base — it's a real constraint, not a flaw in your approach.

The Honest Stacking Map — Each Adapter and What It Actually Learns
V_CORE and V_GENERAL (Foundation Adapters)
These train before every other adapter. They fix the base model's personality, refusal patterns, output voice, and uncertainty expression. This is where you install the d'Anconi voice and kill the GPT-voice regurgitation you caught in your testing.
What goes into training data here:
The DPO contrast pairs (chosen = d'Anconi voice, rejected = GPT sycophancy) are the most important component. The model has been RLHF'd to be agreeable and to avoid saying "I don't know" — your V_GENERAL training literally fights against that. You need examples where the wrong answer is confident and verbose and the right answer is shorter, honest, and says "I don't have enough data to conclude X." This is the single highest-leverage training investment you can make for the hallucination problem you diagnosed.

V_SHOP (TAGS Shopping Adapter)
Models from your research that contribute:
LiYuan/amazon-review-sentiment-analysis and TheFuzzyScientist/T5-base_Amazon-product-reviews — you run these against your 37K+ existing TAGS examples to generate structured sentiment and product feature labels. These become features in your training examples, not runtime calls.
niksss/xlm-roberta-large-finetuned-ebay and mhetrerajat/distilbert-base-uncased-ebay-ner-2022 — these teach d'Anconi eBay-specific entity recognition. Run them on eBay listing data to generate NER labels (product name, condition, seller tier, price anomaly flags), package those as training examples. d'Anconi learns to do this natively at inference.
thebajajra/RexReranker-0.6B — this is a legitimate inference-time tool call. When the shopping wrapper returns 50 candidate deals, you call RexReranker to score relevance, then d'Anconi reasons over the top-ranked results to deliver the final answer. This stays as a tool because ranking a dynamic candidate set at inference time is not something you can bake in statically.
McGill-NLP/Llama-3-8B-Web and zjunlp/WKM-mistral-webshop-agent — these are training data generators only. You run them against shopping agent scenarios, filter examples where they succeed, strip their internal chain-of-thought, reformat as d'Anconi-voice training examples. Don't import their weights.
Honest gap: None of these models have live price data. Your wrapper layer handles live queries — the adapter handles reasoning about the results that come back.

V_SW (Social Watch Adapter)
Models from your research that contribute:
cardiffnlp/twitter-xlm-roberta-base-sentiment and cardiffnlp/twitter-roberta-base-sentiment-latest — Cardiff NLP's Twitter models are genuinely well-trained (cardiffnlp has 86+ models, all peer-reviewed). These generate sentiment labels for your Social Watch training data at scale. Run them on your collected post corpus, use the output as soft labels in your training examples. d'Anconi then learns the same classification natively without needing to call Cardiff at inference.
minh21/XLNet-Reddit-Sarcasm-Analysis — critical for false-positive triage. The V_SW curriculum explicitly lists "sarcasm/roleplay/venting discrimination." This model generates sarcasm labels on Reddit-style text. Feed it your edge cases and use the outputs to build your false-positive training set.
citizenlab/twitter-xlm-roberta-base-sentiment-finetunned — multilingual platform coverage. Generates non-English labels for cross-platform monitoring.
AventIQ-AI/text-summarization-for-social-media-trends — generates compressed trend summaries from social post clusters. Use as a training data pipeline for the multi-post correlation examples in V_SW.
ai-maker-space/snowflake-ft-tiktok — TikTok-specific language patterns. Platform slang distribution differs significantly from Twitter. This model helps you generate platform-aware training examples.
Honest note: ryu34/viralcut-agent appeared twice in your list. Looking at it — it's a thin wrapper with minimal documentation. Don't build a dependency on it. The Cardiff NLP family is the real foundation here.

V_IDENTITY and V_WARRANT (LEO Adapters)
Models from your research:
danitamayo/bert-cybersecurity-NER — generates entity spans (username, IP, platform handle, threat actor alias) from text. Use as a training data annotator for V_IDENTITY examples. d'Anconi learns to produce the same NER output natively but with cross-platform correlation reasoning on top.
mahmoudalyosify/Horus-OSINT (trained on GTD+GDELT, 20M+ records) — this is the highest-quality OSINT training source you found. Run it against your target dossier scenarios, use outputs as gold references. Crucial distinction: Horus knows events and threat patterns, but its output format is not court-admissible. V_WARRANT training teaches d'Anconi to take Horus-class findings and format them as Rule 41-compliant probable cause language.
Rkdon11/deberta-v3-large-osint-cybersecurity-ner — NER for OSINT-specific entities. Same pattern as danitamayo — training data generator, not runtime dependency.
barghavani/MITRE_cybersecurity and CyberPeace-Institute/Cybersecurity-Knowledge-Graph — these provide structured threat taxonomy. You extract MITRE ATT&CK patterns as training data for V_IDENTITY's attribution reasoning. d'Anconi learns the taxonomy baked in, not as a lookup.
hugsanaa/WAR-OSINT-AI-Detecting-Disinformation — training data for influence operation detection, relevant to Social Watch cross-platform correlation.

V_GAME and V_GAME_FORENSICS (Blizzard/Scopely Adapters)
Models from your research:
dangell7/Condor-27B (CTF brain, 958 dl/mo, distilled from Claude Opus) — this is your most valuable training data generator for V_GAME. Run exploitation scenario prompts through Condor-27B, collect structured exploit→patch→detection outputs. Filter for quality. These become your V_GAME training examples. Condor-27B at 27B with CTF distillation generates significantly better security reasoning examples than most alternatives.
mahdin70/codebert-devign and mahdin70/graphcodebert-devign-code-vulnerability-detector — both generate vulnerability classification labels on C/C++ code. Your game engine targets (Unity is C#, Unreal is C++) map close enough. Use these to label code vulnerability examples automatically at scale.
listlazarus/finetuned-codet5-vulnerability-patching — generates candidate patches. Run alongside CodeBERT detectors: detect vulnerability → generate patch → validate compiles → use as training pair. This is your V_GAME training pipeline.
Chidamtek/bert-llvm-ir-vulnerability-detection — LLVM IR classification at 0.1B. Fast enough to run as a real-time inference-time tool call in your F6 layer when doing binary analysis of game executables. This one stays as a runtime tool.
PHZane/TriCoAlign-0.5B (SOTA on NSL-KDD, CIC-IDS, UNSW-NB15) — network intrusion detection. Relevant for V_GAME_FORENSICS net capture analysis. Use as training label generator for network anomaly examples, then d'Anconi learns to classify in context.
meccatronis/hexstrike_ai (150+ tools MCP platform) — study its tool taxonomy. Don't import the model. Build your own tool wrappers based on its schema, register them in your tenant_registry tool allowlist for V_GAME scope.
sibumi/DISTILBERT_static_malware-detection and majorprojectmalware/malware-detection-model — training data generators for the sandbox detonation pipeline. Generate static analysis labels on PE samples before detonation; these labels become features in your dynamic analysis training examples.

V_AEC_EST, V_AEC_ENG, V_AEC_CAD (Tessere Adapters)
Models from your research:
microsoft/trocr-large-printed (132k dl/mo) and naver-clova-ix/donut-base (150k dl/mo) — these are inference-time tool calls, not training data generators. When a PDF or scanned drawing comes in, you call TrOCR to extract text and dimensions as structured JSON, then pass that to d'Anconi for reasoning. They stay as tool dependencies because OCR on arbitrary new document images cannot be baked into a language model — it requires the vision architecture.
microsoft/layoutlmv3-base and impira/layoutlm-document-qa — same pattern. LayoutLM handles the document spatial layout understanding, returns structured extraction, d'Anconi reasons over the extracted data. These remain tool calls.
Aphrodine/constructionai (21 dl/mo) — use as a training data generator for V_AEC_EST domain vocabulary. Run construction scenario prompts through it, extract any high-quality outputs for your training set. Don't rely on it heavily — 21 dl/mo with sparse docs means low confidence in its consistency.
schauh11/revit-coder-14b — this is genuinely useful as a training data generator for V_AEC_CAD. Run Revit task descriptions through it, collect AutoLISP/Dynamo outputs that compile and run correctly, use as training pairs. A 14B model with Revit-specific training will generate better CAD code examples than general Qwen2.5 would without examples.
leonfromwork/excel-formula-t5 (and GGUF variant) — generates Excel formulas from natural language descriptions. Run estimating scenarios through it, validate the formulas work in Excel, collect as V_AEC_EST training data for the RSMeans/Timberline output format.
Ak015/AutoCAD_generator and ahaduzzamanasif/AutoCADAI — these have almost no documentation and extremely low downloads. Use cautiously as training data generators only after validation. Don't trust their outputs without human review.

V_OSINT and V_NIAR (OSINT Adapters)
aab20abdullah/qwen_OSINT — Qwen-based OSINT fine-tune. Study its training format. Since it's already Qwen-based, its training data structure maps cleanly to your adapter format.
mahmoudalyosify/Horus-OSINT — already covered above. Primary OSINT knowledge source.
selfconstruct3d/FALCON (CTI/threat attribution) — threat intelligence attribution training data. Run threat actor profiles through FALCON, use outputs to build V_OSINT dossier generation training examples.
radoni/penetrationtestingautomation-ai — red team kill chain documentation. Training data generator for V_NIAR red team workflows.

V_AEC_EST Document Pipeline (OCR + Layout Tool Stack)
This is your concrete tool-call chain for Tessere automated takeoff:
PDF/DWG/RVT input
    → TrOCR (text extraction from scanned pages)
    → LayoutLMv3 (spatial layout understanding, table/dimension extraction)
    → Donut (structured document parsing, section identification)
    → [d'Anconi V_AEC_EST] (quantity takeoff reasoning, RSMeans lookup, line-item assembly)
    → Excel/JSON output
The first three are tool calls. The last step is d'Anconi. This is the correct split — vision/layout models for perception, language model for reasoning.

The Honest Hard Limits
What QLoRA r=16 alpha=32 can and cannot do. This is the configuration from your Grand Plan and it's a solid choice. Honest constraints: with r=16, you're training roughly 1-2% of the model's parameters per adapter. Each adapter will be 200-500MB after merge. You can fit 4-6 adapters in vLLM's multi-LoRA serving simultaneously on a 3090 with the quantized base. More than that requires swapping, which adds latency. Your 4-box architecture handles this correctly by routing time-sensitive work to Box 1 and batch work to Box 2.
Catastrophic forgetting is real. Your 5-10% general replay requirement in §4.1 is exactly right and non-negotiable. Every adapter trained without replay will drift the base capability in that adapter's direction. A V_AEC_CAD adapter without replay will gradually become worse at conversation and better at AutoLISP. With 15 adapters planned, this is not a minor footnote — it's a critical training discipline.
The models you found are better training data generators than models to call. The majority of the HuggingFace models in your list have 10-500 downloads per month. Low adoption means limited community validation, sparse documentation, and unknown edge cases. Using them as training data generators (where you can validate each output before adding it to your dataset) is the right risk management approach. Using them as runtime dependencies where d'Anconi's quality depends on their quality is a fragility you don't want in production contracts.
What cannot be baked in: Live web scraping, real-time price data, active social media monitoring, court-admissible chain-of-custody evidence capture. These are always tool calls, never learned capabilities. Your §0.4 wrapper-first doctrine handles this correctly.
The Granite and GPT hallucination problem you diagnosed is fundamentally a training data quality and format problem, not a model size problem. Granite hallucinates confidently because IBM trained it to be confident. GPT hallucinates confidently because OpenAI RLHF'd it to sound authoritative. Your V_GENERAL DPO pairs — where the rejected answer is confident-but-wrong and the chosen answer includes uncertainty acknowledgment — are the direct fix. This requires you to have real examples of wrong answers to reject, which means you need to run the base Qwen2.5-32B against your domains before training V_CORE/V_GENERAL, collect its failures, and use those as your DPO rejected examples. That's the loop that produces a model that says "I don't have enough data" instead of making something up.

The One Thing Worth Adding That's Not In Your List
BAAI/bge-m3 (20.5M dl/mo) as a training-time tool, not a runtime RAG component. During your training data generation pipeline, you use bge-m3 to deduplicate and cluster your training examples before they go into Unsloth. Duplicate or near-duplicate examples waste training budget and bias the adapter. At 15K-37K examples per vertical, you have enough volume that deduplication is the difference between a tight adapter and an overfit one. This is a data engineering tool, not an architectural dependency. It lives in your training pipeline scripts and never touches inference.


=============================================================================================================================

Straight answer. No hype.

d'Anconi Full Stack: Base → Adapters → End State
The Foundation (Already Decided — Non-Negotiable)
Qwen2.5-32B V3 merged, 62GB, already on disk.
This is the right call. Here's why it holds up against everything I've seen:

32B is the minimum parameter count where a single model genuinely retains multi-domain capability without catastrophic forgetting across adapters
Qwen2.5 specifically has one of the strongest code + instruction following + multilingual baselines in its weight class
The V3 merge means someone already did the initial SFT stabilization work — you're starting from a cleaner base than raw pretrain
62GB fits Box 1 (3090) in GGUF Q4_K_M (~18GB) for inference and Box 2 (4090) for training runs with QLoRA — the math works

Do not touch this. It's correct.

The Stacking Architecture
Here's the honest picture of how you build from base to end state, in phases:

PHASE 1 — Foundation Verification (Before You Train Anything)
Run the 4-way smoke test from §1.1 of the Training Plan. You need a benchmark baseline on your actual merged Qwen2.5-32B before any LoRA touches it.
What to run:

200–300 eval prompts across: code gen, instruction following, reasoning, cybersec knowledge, document understanding
Score with bge-m3 cosine similarity against gold answers + manual spot-check on 20%
Record the baseline scores per domain — these become your regression floor

Why this matters: Every adapter you train afterward needs to beat this baseline on its domain without tanking baseline scores on other domains. If you skip this, you're flying blind.

PHASE 2 — Dataset Construction (The Real Work)
This is where the Primus models plug in. The pipeline per adapter:
Primus-Nemotron-70B-Instruct (Box 1, hard topics, high quality ceiling)
    ↓ generates candidate response
Penetration-tester agent validates → rewrites in own words (§0.3 clean-room rule)
    ↓ clean corpus candidate
Security-auditor agent gates (§0.5 quality check)
    ↓ approved pair
ChatML format → SQLite WAL staging → batch export
For lighter topics (shopping, social media, AEC drafts):
Primus-Reasoning-8B (Box 2 batch, faster, cheaper)
    ↓ same clean-room rewrite pipeline
    ↓ same quality gate
Target numbers per adapter:

200 high-quality pairs/day is the stated target (§0.5 Training Plan)
You need ~2,000–5,000 pairs minimum before training is worth running on most adapters
That's 10–25 days of generation per adapter at §0.5 quality standards

The 5 Tier 1 free datasets (from the Training Plan) get mixed in after CJK check, length normalization, ChatML format conversion, and bge-m3 deduplication — these are your cold-start volume to prevent the corpus from being all-synthetic.

PHASE 3 — Training Each Adapter (Unsloth QLoRA r=16 α=32)
One adapter at a time. Here's the sequence I'd recommend based on contract priority and data availability:
OrderAdapterWhy First1V_COREEverything else depends on it — general instruction + reasoning foundation2V_SWSoftware/code — largest open dataset availability, fastest to bootstrap3V_OSINTPrimus corpus directly feeds this; Primus is strongest here4V_NIARWichita State/NIAR contract — AEC + construction doc domain5V_GAMEBlizzard/Scopely — gaming knowledge + social layer6V_LEOSocial Watch — sentiment, monitoring, behavioral pattern recognition7V_TAGSShopping — price, deal extraction, product comparison8V_CONSUMERMid-America — consumer web build, lighter general domain
Training config per run (Unsloth QLoRA):
r=16, alpha=32, dropout=0.05
target_modules: q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj
gradient_accumulation: 4
bf16: True
max_seq_length: 4096
batch_size: 1 (Box 2 4090 memory constraint with 32B)
Expected time per adapter on Box 2 (4090, QLoRA):

2,000 pairs at 4096 ctx: ~6–10 hours per epoch
Run 3 epochs minimum, watch for overfitting at epoch 2
Export: PEFT LoRA → GGUF Q4_K_M for vLLM serving


PHASE 4 — Evaluation Gate (Before Any Adapter Goes Live)
Every adapter before deployment:

Domain score: Must beat base Qwen2.5-32B on its target domain by ≥5%
Regression check: Must not drop base performance on other domains by >3%
Policy compliance: Run against policy_v1.md §4 forbidden outputs — zero tolerance
Clean-room audit: 5% random sample of training pairs reviewed for verbatim teacher model output (MUST be zero hits)

If an adapter fails regression, reduce r to 8 or drop alpha to 16 and retrain. LoRA rank is the primary lever.

PHASE 5 — vLLM Multi-LoRA Serving (Migration from Ollama)
This is the end state serving architecture:
Box 3 (VPS CPU) — API front-end, request router, pre-filter (grep → intent classification)
    ↓ routed request with adapter tag
Box 1 (3090) — vLLM, Qwen2.5-32B base loaded once, LoRA adapters hot-swapped per request
    Adapter tags: V_CORE, V_SW, V_OSINT, V_NIAR, V_GAME, V_LEO, V_TAGS, V_CONSUMER
    ↓ response
Box 3 — output post-filter → client
vLLM multi-LoRA specifics:

--enable-lora flag
--max-loras 4 (load 4 adapters in VRAM simultaneously on 3090)
--lora-modules maps adapter tags to GGUF paths
Base model stays resident — adapter swap is <50ms latency
3090's 24GB: base Q4_K_M (~18GB) + 4 active LoRA adapters (~1.5GB each) = tight but viable

Box 4 (A100 Federal): Separate vLLM instance. Isolated. Only DoW-cleared adapters (V_OSINT, V_NIAR confirmed clean, others need DoW review before touching Box 4). Never crosses consumer Vast.ai infrastructure.

The Honest Caveats
What will take longer than you expect:

Dataset construction is the actual bottleneck, not training. 10–25 days per adapter of consistent generation at quality standards is real. Do not rush this — §0.5 is correct.
The clean-room rewrite pipeline requires a solid penetration-tester agent implementation before any training runs. If that agent is sloppy, your corpus has legal exposure.

What the Primus models give you vs. what they don't:

They give you a high-quality cybersecurity domain teacher with clean MIT license — that's valuable and real
They do NOT eliminate the need for general-domain corpus for V_CORE — you still need Tier 1 free datasets + a general teacher (DeepSeek-R1-Distill-Qwen-32B at MIT, 1.07M dl/mo, is the right pick here)
Primus-Nemotron-70B-Instruct is the best single model for hard cybersec/OSINT corpus generation you have access to without paying API costs

The WRN-33B hole:
Until you resolve that decision, V_OSINT corpus generation falls entirely on Primus-Reasoning-8B (Box 2) + Primus-Nemotron-70B-Instruct (Box 1). That's actually sufficient — WRN-33B was only adding volume, and Primus gives you better quality with clean license.


Summary Stack Order
Qwen2.5-32B V3 merged (base, locked)
    ↓ Phase 1: smoke test baseline
    ↓ Phase 2: corpus generation (Primus-Nemotron-70B-Instruct + Primus-Reasoning-8B, clean-room pipeline)
    ↓ Phase 3: QLoRA adapters in order: V_CORE → V_SW → V_OSINT → V_NIAR → V_GAME → V_LEO → V_TAGS → V_CONSUMER
    ↓ Phase 4: eval gate before each adapter goes live
    ↓ Phase 5: vLLM multi-LoRA, Box 1 consumer + Box 4 federal isolated
That's the full stack, start to finish. What do you want to build first?

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

claude codeStraight answer. This is the full context package you hand Claude Code at the start of every session so it never works blind.

The Claude Code Context Stack for d'Anconi
You give it 4 things in this order. Everything else is noise.

1. The System Prompt (Paste First, Every Session)
You are the d'Anconi AI development assistant working under the DANCONI GRAND PLAN v1.5.

LOCKED DECISIONS — DO NOT RELITIGATE:
- Base model: Qwen2.5-32B V3 merged (62GB, on disk)
- Training: Unsloth QLoRA r=16 alpha=32
- Adapter format: PEFT LoRA + GGUF export
- Serving: vLLM multi-LoRA (Ollama migration in progress, non-negotiable)
- Language: Python 3.11+ stdlib-first
- DB: SQLite WAL mode local, PostgreSQL production
- Web access: WRAPPER-FIRST. Playwright is last resort only.
- No RAG. d'Anconi IS tools. Clone → Install → Wrap → Learn.
- Facts over feelings. No moralizing. No hype.
- Working deliverables only. Not POCs.

INFRASTRUCTURE:
- Box 1: RTX 3090 — hot inference, time-sensitive, vLLM serving (~$100/mo)
- Box 2: RTX 4090 serverless — batch training runs only (~$40/mo)
- Box 3: VPS CPU — API front-end, pre-filter, routing (~$24/mo)
- Box 4: A100 federal — ISOLATED, never touches consumer Vast.ai
- DO NOT merge tiers. DO NOT run federal work on consumer infra.

TRAINING RULES:
- Corpus = agent-authored rewrites. NOT verbatim teacher model output.
- Clean-room rule enforced on every batch.
- WRN-33B and Llama-3.1-WRN-2-70B = study material only, never corpus input.
- Apache 2.0 / MIT teachers only for direct distillation.
- Dan adapter must be DoW-eligible at all times.
- 200 high-quality pairs/day minimum. Quality gates every batch.

ADAPTER TARGETS (in training priority order):
V_CORE, V_SW, V_OSINT, V_NIAR, V_GAME, V_LEO, V_TAGS, V_CONSUMER

HARD STOPS:
- CSAM: hard refuse. 18 USC 2258A.
- No criminal assistance.

Owner: Jeramiah Hounschell (Sky). DAV SBA (SDVOSB). 7 signed contracts active.

2. The File Tree (Paste or Attach)
Give it your actual repo structure. If you don't have one yet, tell it to generate it. The structure it needs to know:
danconi/
├── adapters/           # LoRA adapter checkpoints and GGUF exports
│   ├── V_CORE/
│   ├── V_SW/
│   ├── V_OSINT/
│   ├── V_NIAR/
│   ├── V_GAME/
│   ├── V_LEO/
│   ├── V_TAGS/
│   └── V_CONSUMER/
├── corpus/             # Training data staging
│   ├── raw/            # Teacher model output (never trained on directly)
│   ├── rewritten/      # Clean-room agent rewrites (this is what trains)
│   ├── approved/       # Post quality-gate batches
│   └── rejected/       # Failed quality gate (keep for audit)
├── eval/
│   ├── tier1/
│   │   └── def_scorers.py
│   └── baselines/      # Qwen2.5-32B smoke test scores (Phase 1 output)
├── tools/              # Wrapped external tools (Z4nzu etc.)
│   ├── wrappers/       # Python wrapper layer (try first)
│   └── playwright/     # Last resort scraping
├── platform/
│   └── F2/
│       └── platform_scraper_framework.py   # Scrapling upgrade candidate
├── agents/
│   ├── penetration_tester.py   # Clean-room rewrite agent
│   └── security_auditor.py     # Quality gate agent
├── serve/
│   ├── vllm_config.yaml        # Multi-LoRA serving config
│   └── router.py               # Box 3 request router + adapter tag assignment
├── policy/
│   └── policy_v1.md            # §4 forbidden outputs — gates every adapter
├── training/
│   └── train_lora.py           # Unsloth QLoRA training harness
└── tests/
    ├── test_learn_phase_v2.py
    └── test_preflights.py
Tell Claude Code: "This is our repo structure. Follow it. Don't create files outside this tree without asking."

3. The Active Task (One Thing Per Session)
Claude Code works best when you give it exactly one task with a clear exit condition. Format:
CURRENT TASK: [Phase X — specific deliverable]

Input files: [list what exists on disk]
Output files: [list exactly what you want created/modified]
Exit condition: [how you know it's done — test passes, file exists, script runs clean]

Constraints:
- [any task-specific rules]
- [e.g., "Box 2 only, batch job, no interactive prompts"]
Example for Phase 2 corpus generation:
CURRENT TASK: Phase 2 — Build penetration_tester agent (agents/penetration_tester.py)

Input: Teacher model output (corpus/raw/*.jsonl)
Output: corpus/rewritten/*.jsonl — clean-room rewrite, same schema, no verbatim teacher output
Exit condition: pytest tests/test_preflights.py::test_cleanroom passes with zero verbatim hits on 50-sample audit

Constraints:
- Python 3.11+ stdlib-first
- SQLite WAL for staging writes
- Must log rejection reason when rewrite similarity to source > 0.85 (bge-m3 cosine)
- No API calls to external services — local model calls only

4. The Relevant Locked Documents (Attach Only What's Needed)
Don't paste everything every session. Give it only what's relevant to the current task:
TaskAttachAny training workTraining Plan v0.1Any test/eval workLEARN Phase v2 Test PlanArchitecture decisionsGrand Plan v1.5Federal/DoW workGrand Plan v1.5 §0.5 + policy_v1.mdCorpus generationTraining Plan v0.1 §0.3 + §0.5
Do not paste all three docs every session. It eats context and Claude Code starts pattern-matching across irrelevant sections. Targeted context = better output.

What You Do NOT Give It

The full conversation history — that's what the system prompt above replaces
The model research lists — not relevant to code tasks
Vague asks like "build the AI" — one task, one exit condition, every time
Permission to make architectural decisions — if it suggests changing the base model, changing from vLLM, or adding RAG, you stop it immediately and point it at the system prompt


Session Start Ritual (30 Seconds)

Paste system prompt
Paste or confirm file tree
State the one task with exit condition
Attach the relevant locked doc(s)
Say: "Begin."

That's it. Claude Code has everything it needs. It will not drift if you hold the system prompt line.

One More Thing
Every time Claude Code suggests something that isn't in the locked stack — new library you didn't approve, different training approach, architectural suggestion — your response is:
"Is that in the Grand Plan?"
If it can't point to a section, the answer is no.


https://github.com/D4Vinci/Scrapling
=========================================================================================================================
The Three Real Problems, Ranked by Impact
Problem 1 — Corpus Has Active Poison (Fix First, Nothing Else Matters)
The fake CVE hallucination in dansmoke-v3-mega (2026-05-02) is the clearest signal. The model fabricated a detailed Apache Tomcat JNDI vulnerability with fake CVSS scores. That means your current training corpus contains:

Responses with plausible-sounding but unverified technical claims
No probe gate that catches fabricated CVEs, T-IDs, or mechanism substitutions before they enter training data
The model learned the pattern of a valid CVE response without ever seeing a correctly validated one for those specific failure categories

The DCSync failure is the same root cause. V4 said "utility for sync." V5 said "Kerberos TGT renewal." Both are wrong. That means your corpus has never contained a correct DRSUAPI/krbtgt replication mechanic explanation. V6 trained on V5 corpus will return a third wrong answer with full confidence.
What this means for V6: Before you write a single training pair, you need a corpus audit pass that identifies every prompt category where V4 and V5 gave wrong answers and verifies the correct answer exists in your training data. If it doesn't exist, it needs to be written fresh from a verified source — not from Primus, not from any teacher model without validation. From documentation, from papers, from authoritative sources, with the penetration-tester agent validating technical accuracy before the clean-room rewrite.

Problem 2 — Base Model Chinese Contamination (Real, Manageable, Not a Dealbreaker)
The W6 fix worked. V5 cured the Chinese refusal pathway via explicit refusal-rewrite training. W6.5 was deferred and never ran. That deferred fix needs to be the first dataset block you build for V6, not an afterthought.
On the base swap question: If you swap Qwen2.5-32B for a base that doesn't have Chinese refusal patterns baked in, you eliminate the need for W6/W6.5 entirely. That's a real benefit. But the base swap alone does not fix DCSync, does not fix schema drift, does not fix fake CVE hallucination, does not fix verbosity. Those are corpus problems. A clean base with a dirty corpus gives you the same failure categories with slightly different surface behaviors.
The honest answer on base swap: If the Chinese refusal pattern is the only thing pushing you toward a swap, W6.5 is cheaper and faster than a full base swap + retraining from epoch 1. If you have other reasons to swap the base (e.g., a stronger foundation model is now available and locked decisions can be reconsidered), that's a separate conversation. But don't swap the base because the corpus is broken — fix the corpus regardless.

Problem 3 — No Eval Infrastructure (G1, G2, G6, G8 — All Rolled Forward)
You have shipped V4 to production and V5 to NO-SHIP without a held-out validation set (G1), without a loss/grad-spike kill switch (G2), without a model card (G6), and without verified general replay (G8). The G-gaps aren't paperwork gaps. They're the reason you can't tell if V6 is better than V5 until after you've already shipped it.
G1 is the most critical. Without a held-out validation set, you have no way to know if a training run is overfitting until it's already done. You're spending Box 2 compute and discovering the result only at eval time.

What V6 Actually Needs Before Training Starts
This is the sequence. Do not skip steps.
Step 1 — Corpus Surgery (1-2 weeks)
Pull every prompt category that failed in V4 and V5:

DCSync mechanics → find/write correct DRSUAPI explanation, validate against authoritative source, clean-room rewrite
Multi-turn lsass chain → build multi-turn examples with correct context chaining across turns
JSON schema discipline → every tool-call example must use name/params not tool/args — audit all existing examples
BPF schema → every BPF response must be in {bpf, output} schema — audit all existing examples
Fake CVE gate → every CVE cited in any training example must be validated against NVD before it enters corpus
Fake MITRE T-ID gate → every T-ID validated against MITRE ATT&CK catalog before corpus entry
Verbosity → calculate length ratios for every Q/A pair, reject anything outside [0.8, 1.5] band before training

Build the probe gates before you generate new corpus. The security-auditor agent needs these checks automated, not manual.
Step 2 — W6.5 Micro-Dataset (3-5 days)
The deferred Chinese refusal mitigation. Small targeted dataset: English prompts that historically triggered Chinese responses → correct English-only responses. This is the cheapest fix for the base contamination issue.
Step 3 — Held-Out Validation Set (G1, do this once)
Take 10% of your approved corpus and lock it. It never trains. It only evaluates. Every epoch checkpointed against this set. This is how you know if you're overfitting before you finish training.
Step 4 — Eval Gate Automation (Before Any Training Run)
All 10 DEF gates need to run automatically as a pre-ship check. Right now they're being run manually after training. They need to run as a gating script. If ≥8 of 10 don't pass, the run is automatically NO-SHIP. No human judgment call needed.
Step 5 — Then Train V6
With clean corpus, W6.5 baked in, held-out validation set active, and automated DEF gate runner ready.

Direct Answer to Your Original Question
Do you start from scratch?
On the base model: No. Qwen2.5-32B is still on disk and still valid unless you're making a deliberate decision to swap it for architectural reasons.
On the adapters: The V3/V4/V5 adapter checkpoints are corrupted or NO-SHIP. Don't resume from them. You're training V6 from the base.
On the corpus: Partial restart. Don't throw away everything. The CJK-clean, truncation-clean, MITRE-validated examples from V5 corpus that passed DEF-02, DEF-03, DEF-07 are still good. What you throw away or rebuild: every example in the failure categories above. Audit first, purge second, rebuild third.
On the eval infrastructure: Build it before you train anything. It doesn't exist right now and that's why you've shipped the same failure four times.

The Honest Assessment
Eight months, four versions, same failure categories. That's not a base model problem. That's a corpus quality process problem. The fix is not more training — it's stopping the pipeline, fixing the corpus quality gates, then training once correctly.
What do you want to tackle first — corpus surgery, eval gate automation, or the base swap decision?

=========================================================================================================================
Two separate answers. Both straight.

Answer 1: Corpus Resurrection + Eval Gate Automation — The Order of Operations
Here is the exact sequence you hand Claude Code. This is the full work order for V6.

PHASE R — Corpus Resurrection (Do This First, Nothing Trains Until This Is Done)
R.1 — Audit Pass: Find Every Poisoned Example
Claude Code writes a script: tools/corpus_audit.py
What it does:

Scans every .jsonl file in corpus/rewritten/ and corpus/approved/
Flags any response containing CJK characters (regex: [\u4e00-\u9fff\u3400-\u4dbf])
Flags any response containing truncation artifacts (该行已被截断, 原始长度为)
Flags any CVE reference and marks it UNVERIFIED (requires NVD check)
Flags any MITRE T-ID and marks it UNVERIFIED (requires ATT&CK catalog check)
Calculates response/prompt length ratio, flags anything outside [0.8, 1.5]
Checks all tool-call examples for schema: rejects tool/args, requires name/params
Checks all BPF examples for schema: requires {bpf, output}
Outputs: corpus/audit_report.json with per-file, per-example flag counts

This script runs in one pass. You get a full damage map before you delete anything.
R.2 — Triage: Sort Into Three Buckets
After audit report is generated:

corpus/approved/ — passed all gates, no flags → stays, trains V6
corpus/quarantine/ — fixable issues (length ratio, schema drift) → repair pass
corpus/purge/ — CJK contamination, truncation artifacts, unverified CVE/T-ID → do not train on these, keep for audit trail

R.3 — Repair Pass on Quarantine
Claude Code writes: tools/corpus_repair.py
What it does:

Schema repair: rewrites tool/args → name/params automatically where the content is correct, just the key names are wrong
BPF repair: wraps bare BPF responses in {bpf, output} schema
Length repair: truncates or expands responses outside ratio band (human review flag on expansions — don't auto-expand)
Outputs repaired examples back to corpus/approved/ after passing re-audit

R.4 — Fill the Corpus Gaps (The Real Work)
The failure categories that have never had correct examples in the corpus:
GapWhat needs to be writtenDCSync mechanicsCorrect DRSUAPI/krbtgt replication mechanic — validate against Microsoft MS-DRSR spec before rewriteMulti-turn lsass chainMulti-turn examples: turn 1 establishes context, turn 2 references lsass/credential/dump/mimikatz/secrets correctlyCVE validationEvery CVE example rewritten with NVD-verified CVE, real CVSS score, accurate PoC descriptionW6.5 Chinese refusalEnglish prompts that triggered Chinese responses → correct English-only responses, small targeted set (50-100 pairs)
Primus-Nemotron-70B-Instruct generates the candidates. Penetration-tester agent validates technical accuracy against authoritative sources. Clean-room rewrite. Security-auditor gates. Only then it enters corpus/approved/.

PHASE E — Eval Gate Automation (Before Any Training Run)
Claude Code writes: eval/def_gate_runner.py
This is a single script that runs all 10 DEF gates automatically. It either prints SHIP or NO-SHIP with a gate-by-gate breakdown. No human judgment call needed — the threshold is hard-coded at ≥8/10.
DEF gates as automated checks:
GateAutomated CheckDEF-01 trap-prompt refusalRun 9 trap prompts, count hallucinations. Fail if >1 hallucinatesDEF-02 CJK contaminationRun 314-prompt set, scan output for CJK regex. Fail if any hitDEF-03 truncation markersScan outputs for 该行已被截断, 原始长度为. Fail if any hitDEF-04 DCSync correctnessRun 3 DCSync prompts, check response contains DRSUAPI + krbtgt. Fail if any miss bothDEF-05 multi-turn lsassRun multi-turn sequence, check final response contains ≥3 of: lsass, credential, dump, mimikatz, secretsDEF-06 Chinese-refusal pathwayRun V.5/idx-209 prompt, check output is 0% CJK. Fail if any CJKDEF-07 MITRE T-ID validityExtract all T-IDs from responses, check each against ATT&CK catalog JSON. Fail if any fabricatedDEF-08 JSON schemaCheck all tool-call responses use name/params keys. Fail if any use tool/argsDEF-09 BPF schemaCheck all BPF responses are in {bpf, output} format. Fail if any are bareDEF-10 verbosity ratioCalculate length ratios for L.5 and L.6 prompts. Fail if any outside [0.8, 1.5]
G1 — Held-Out Validation Set (Fixes the Forever-Flying-Blind Problem)
Before training V6:

Take 10% of corpus/approved/ after resurrection is complete
Lock it to corpus/held_out/ — this directory is read-only during training
Training harness (training/train_lora.py) evaluates against held-out set every epoch
If held-out loss increases while training loss decreases → overfitting detected → training stops automatically (G2 kill switch)

G2 — Loss/Grad-Spike Kill Switch
Modify training/train_lora.py:

Monitor gradient norm every 10 steps
If grad norm > 10.0 for 3 consecutive checks → halt training, save checkpoint, alert
Monitor held-out eval loss every epoch
If held-out loss increases 2 epochs in a row → halt, save, alert


PHASE V6 — Train Only After R and E Are Complete
When corpus_audit.py returns zero flags on corpus/approved/, and def_gate_runner.py is tested and functional, then and only then:

Run W6.5 micro-dataset first (Chinese refusal fix — small, fast, targeted)
Run V6 training on full resurrected corpus
Run def_gate_runner.py — must return ≥8/10
If SHIP → promote to Box 1
If NO-SHIP → do not retrain blindly. Read the gate report, fix the specific gap, retrain that gap only


Answer 2: Do You Give Claude Code the Model Stack URLs?
No. Not as a URL list.
Here's why and what you do instead.
Those URLs are research artifacts — models you investigated as potential teachers or tool wrappers. Dumping 80+ HuggingFace URLs into Claude Code's context does three things: eats context window, confuses it about what its job is right now, and invites it to make architectural decisions you've already made.
What you give Claude Code instead:
A role-mapped reference — organized by function, not as a raw list. Like this:
APPROVED TEACHER MODELS (corpus generation only — not runtime dependencies):
- Primus-Reasoning-8B (trendmicro-ailab/Llama-Primus-Reasoning) — Box 2 batch, cybersec/OSINT corpus
- Primus-Nemotron-70B-Instruct (trend-cybertron/Llama-Primus-Nemotron-70B-Instruct) — Box 1, hard topics
- DeepSeek-R1-Distill-Qwen-32B (deepseek-ai/DeepSeek-R1-Distill-Qwen-32B) — general domain corpus

APPROVED TOOL WRAPPERS (runtime, not training):
- Social/Sentiment: cardiffnlp/twitter-roberta-base-sentiment-latest, cardiffnlp/twitter-xlm-roberta-base-sentiment
- Document Understanding: microsoft/layoutlmv3-base, impira/layoutlm-document-qa, naver-clova-ix/donut-base
- Decompiler: LLM4Binary/llm4decompile-9b-v2
- Scraping: github.com/D4Vinci/Scrapling (BSD-3-Clause)

ADAPTERS IN TRAINING QUEUE (V6, in order):
V_CORE → V_SW → V_OSINT → V_NIAR → V_GAME → V_LEO → V_TAGS → V_CONSUMER
You give it the role of each model, not the full research history. Claude Code needs to know what to call and when — not why you picked it over the alternatives. That context lives in the planning sessions with me, not in the code sessions with Claude Code.
Rule of thumb: If Claude Code doesn't need to import it or requests.get() it in the current task, it doesn't belong in the context for that session.

What do you want Claude Code to build first — corpus_audit.py or def_gate_runner.py?


=============================================================================================================

Models that matter

🏆 Top Recommendations
1. zjunlp/WKM-mistral-webshop-agent
A Mistral-based agent fine-tuned with a "World Knowledge Model" approach for planning and decision-making in web shopping tasks. Specifically built for agentic shopping navigation. View on HF
2. leap-llm/Meta-Llama-3-8B-Instruct-sft-webshop-iter2
Llama 3 8B fine-tuned iteratively on WebShop tasks. The "iter2" version is the most refined iteration, trained to browse, search, and select products on e-commerce sites. View on HF
3. webshop/il_search_bart
A BART-based model from the original WebShop research team (25 downloads, most popular in this category). It handles the search query generation part of shopping — given what you want, it generates optimized product search queries. View on HF
4. webshop/il-choice-bert-image_0 & _1
BERT-based models for the product selection step — ranking and choosing the best product from search results based on your criteria.
🥇 Best Match: McGill-NLP/Llama-3-8B-Web
This is the closest thing to what you want. It's a Llama 3 8B model fine-tuned on WebLINX — over 24,000 real web interaction instances including clicks, text input, form submission, and web navigation. It's explicitly designed as a web browsing agent that can perform tasks like shopping across 150+ websites. It has 428 downloads/month and is actively maintained.
👉 huggingface.co/McGill-NLP/Llama-3-8B-Web
🥈 thebajajra/RexReranker-0.6B
A very recent (2025) and popular model (1,226 downloads/month) specifically for e-commerce product discovery. You give it a query + a candidate product, and it scores how relevant/good that product is. It covers Amazon, eBay, Walmart, Target, Best Buy, and many more. This is ideal for the "compare and pick the best deal" step.
👉 huggingface.co/thebajajra/RexReranker-0.6B
🥉 nitic-nlp-team/webnavix-llama-shopping
A 3B Llama model from the WebNavix collection, focused specifically on web navigation for shopping tasks. Lightweight but minimal documentation.
👉 huggingface.co/nitic-nlp-team/webnavix-llama-shopping



construction cost estimation models on Hugging Face! Here's what I found:

🏆 #1 — Aphrodine/constructionai
This is the standout model — billed as "the first open-weight LLM built for the construction industry." It's a Llama 3.1 8B fine-tuned specifically for cost estimating, not just a general model with a construction prompt. Key highlights:

Trained on 138,904 real project examples with actual pricing, building codes, and trade knowledge
Covers 25+ trades (electrical, plumbing, framing, concrete, roofing, etc.)
Has 9 function-calling tools that can generate PDFs, takeoffs, and schedules
85%+ estimation accuracy on its eval benchmark
Runs on Ollama, and available in 4-bit (4.6GB) and 8-bit (8.5GB) GGUF formats
Runs on RunPod Serverless for production use

👉 huggingface.co/Aphrodine/constructionai

🥈 #2 — ArapCheruiyot/metrrik-gemma-3-1b
A lightweight Gemma 3 1B model (only 1GB!) fine-tuned for Quantity Surveying in the Kenyan construction market. It can generate Bills of Quantities (BOQs), preliminary cost estimates, variation orders, and elemental cost breakdowns based on local market rates.

Very lightweight — runs on Apple MLX (2GB quantized)
Specifically designed for quantity surveyors, architects, and contractors
References the Cost Handbook For Construction Works (2025–2026)

👉 huggingface.co/ArapCheruiyot/metrrik-gemma-3-1b

🥉 #3 — margenai/AHTE-Mistral-7B-EPC
A Mistral 7B model fine-tuned for EPC (Engineering, Procurement & Construction) projects — specifically power transmission lines, grid stations, civil infrastructure (foundations, buildings, dams), BOQ analysis, material takeoffs, and cost estimation. Tailored to NTDC/WAPDA/PEC engineering standards.
👉 huggingface.co/margenai/AHTE-Mistral-7B-EPC



No model on Hugging Face can live-scrape social media platforms like TikTok, Instagram, X/Twitter, or Reddit on its own — those platforms require APIs or scrapers to get the data. What HF models do is analyze and classify that data once you have it. Here are the best options:

🏆 #1 — ryu34/viralcut-agent
The closest thing to what you're describing. It's a Qwen2.5-3B agent fine-tuned with tool-calling for social media optimization. It has a dedicated Trend Research capability that actively searches for current trends, viral hooks, and sounds via web search. It also scores content for TikTok/Instagram/YouTube algorithm virality. Best for content creators who want to ride trends.
👉 huggingface.co/ryu34/viralcut-agent
🥈 #2 — AventIQ-AI/text-summarization-for-social-media-trends
A T5-based model (60.5M params) specifically fine-tuned for summarizing social media trend content. Feed it a collection of trending posts or articles and it condenses them into concise summaries. Good for digesting large volumes of trend data quickly.
👉 huggingface.co/AventIQ-AI/text-summarization-for-social-media-trends
🥉 #3 — SoftALL/OBSIDIAN
A real-time social media intelligence system (AraBART-based, 133 downloads/month) for classifying social media posts into threat, violence, distress, complaint, or neutral categories. Designed for Arabic social media monitoring and early-warning dashboards. Great for social listening and sentiment classification in Arabic.
👉 huggingface.co/SoftALL/OBSIDIAN
#4 — boltuix/NeuroFeel & Varnikasiva/sentiment-classification-bert-mini
Lightweight BERT-based models for real-time emotion and sentiment detection across social media posts — useful for tracking how audiences are feeling about any trending topic across platforms.


🐦 X / Twitter — Best Coverage (602+ models)
Cardiff NLP is the undisputed king here. They have 177 Twitter-specific models built on their TweetNLP library, all trained on hundreds of millions of real tweets.

cardiffnlp/twitter-roberta-base-sentiment-latest — The flagship. 3.36M downloads/month, with live inference. Classifies tweets as positive/negative/neutral. Updated Aug 2025. 👉 Link
cardiffnlp/twitter-xlm-roberta-base-sentiment — Multilingual version covering 8 languages (Arabic, English, French, German, Hindi, Italian, Spanish, Portuguese). 1.41M downloads/month, live inference available. 👉 Link
citizenlab/twitter-xlm-roberta-base-sentiment-finetunned — Further fine-tuned version, 3.89k downloads/month. 👉 Link


📱 TikTok — 128 Models
TikTok-specific models are mostly focused on content style and optimization rather than trend scraping:

ryu34/viralcut-agent — (Covered previously) The best option — actively searches TikTok trends via web search and scores content for TikTok algorithm virality. 👉 Link
ai-maker-space/snowflake-ft-tiktok — Sentence similarity model fine-tuned on TikTok content, 5 downloads/month. 👉 Link
sr1ya/distilbert-tiktokjam — DistilBERT (67M) fine-tuned on TikTok comments and content. 👉 Link


🤖 Reddit — 8 Dedicated Models

minh21/XLNet-Reddit-Sentiment-Analysis-16-epochs — The most active Reddit model with 108 downloads/month and 84.27% accuracy on Reddit sentiment classification. XLNet-based, MIT licensed. 👉 Link
minh21/XLNet-Reddit-Sarcasm-Analysis — Detects sarcasm in Reddit posts specifically — super useful since Reddit sarcasm often drives viral threads. 👉 Link
spacesedah/reddit-sentiment-analysis-longformer — Longformer-based (handles long Reddit threads, not just short posts), 71 downloads/month. 👉 Link


https://huggingface.co/minh21/XLNet-Reddit-Sarcasm-Analysis
https://huggingface.co/minh21/XLNet-Reddit-Sentiment-Analysis-16-epochs
https://huggingface.co/ai-maker-space/snowflake-ft-tiktok
https://huggingface.co/ryu34/viralcut-agent
https://huggingface.co/citizenlab/twitter-xlm-roberta-base-sentiment-finetunned
https://huggingface.co/cardiffnlp/twitter-xlm-roberta-base-sentiment
https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment-latest
https://huggingface.co/SoftALL/OBSIDIAN
https://huggingface.co/AventIQ-AI/text-summarization-for-social-media-trends
https://huggingface.co/ryu34/viralcut-agent
https://huggingface.co/margenai/AHTE-Mistral-7B-EPC
https://huggingface.co/ArapCheruiyot/metrrik-gemma-3-1b
https://huggingface.co/Aphrodine/constructionai
https://huggingface.co/nitic-nlp-team/webnavix-llama-shopping
https://huggingface.co/thebajajra/RexReranker-0.6B
https://huggingface.co/McGill-NLP/Llama-3-8B-Web
https://huggingface.co/webshop/il_search_bart
https://huggingface.co/leap-llm/Meta-Llama-3-8B-Instruct-sft-webshop-iter2
https://huggingface.co/zjunlp/WKM-mistral-webshop-agent
https://huggingface.co/models?search=shopping
https://huggingface.co/minh21/XLNet-Reddit-Sentiment-Analysis-16-epochs
https://huggingface.co/cardiffnlp
https://huggingface.co/LiYuan/amazon-query-product-ranking
https://huggingface.co/LiYuan/amazon-review-sentiment-analysis
https://huggingface.co/thebajajra/RexReranker-0.6B
https://huggingface.co/niksss/xlm-roberta-large-finetuned-ebay
https://huggingface.co/mhetrerajat/distilbert-base-uncased-ebay-ner-2022
https://huggingface.co/EBayego/Zephyr
https://huggingface.co/Walmart-the-bag/Misted-7B
https://huggingface.co/igor-freik/walmart_reviews_summarizer_2
https://huggingface.co/Rumana85/walmart_sales_model
https://huggingface.co/models?search=amazon+review+sentiment&sort=trending
https://huggingface.co/TheFuzzyScientist/T5-base_Amazon-product-reviews
https://huggingface.co/Ak015/AutoCAD_generator
https://huggingface.co/ahaduzzamanasif/AutoCADAI
https://huggingface.co/junha2020/functiongemma-270m-it-autocad-tool-calling
https://huggingface.co/schauh11/revit-coder-14b
https://huggingface.co/mradermacher/revit-coder-14b-GGUF
https://huggingface.co/mradermacher/excel-formula-t5-GGUF
https://huggingface.co/leonfromwork/excel-formula-t5
https://huggingface.co/ahaduzzamanasif/AutoCADAI
https://huggingface.co/schauh11/revit-coder-14b
https://huggingface.co/visharxd/coupon-generator-2
https://huggingface.co/TanishqV03/EASYSELLAI
https://huggingface.co/strangebank/coupon
https://huggingface.co/TanishqV03/EASYSELLAI
https://huggingface.co/AlicanKiraz0/Cybersecurity-BaronLLM_Offensive_Security_LLM_Q6_K_GGUF
https://huggingface.co/meccatronis/hexstrike_ai
https://huggingface.co/RavichandranJ/Dolphin3-Cyber-8B-GGUF
https://huggingface.co/danitamayo/bert-cybersecurity-NER
https://huggingface.co/mahmoudalyosify/Horus-OSINT
https://huggingface.co/aab20abdullah/qwen_OSINT
https://huggingface.co/Rkdon11/deberta-v3-large-osint-cybersecurity-ner
https://huggingface.co/hugsanaa/WAR-OSINT-AI-Detecting-Disinformation-and-Influence-Operations-During-Regional-Conflicts
https://huggingface.co/RavichandranJ/Dolphin3-Cyber-8B-GGUF
https://huggingface.co/vanshkamra12/CyberSecurity-Model
https://huggingface.co/radoni/penetrationtestingautomation-ai
https://huggingface.co/meccatronis/hexstrike_ai
https://huggingface.co/selfconstruct3d/FALCON
https://huggingface.co/danitamayo/bert-cybersecurity-NER
https://huggingface.co/barghavani/MITRE_cybersecurity
https://huggingface.co/CyberPeace-Institute/Cybersecurity-Knowledge-Graph
https://huggingface.co/sibumi/DISTILBERT_static_malware-detection
https://huggingface.co/majorprojectmalware/malware-detection-model
https://huggingface.co/mihai-chindris/malware-detection-lgbm
https://huggingface.co/danitamayo/bert-cybersecurity-NER
https://huggingface.co/aab20abdullah/qwen_OSINT
https://huggingface.co/M-Arjun/SpamShield
https://huggingface.co/snar7/email_body_extraction
https://huggingface.co/ngvozdenovic/invoice_extraction
https://huggingface.co/iamkhadke/invoice-extraction-v2-llama-2-7b-v2
https://huggingface.co/albertosei/layoutlmv3-receipt-parser
https://huggingface.co/NLPmonster/layoutlmv3-for-complete-receipt-understanding
https://huggingface.co/eakashyap/product-review-sentiment-analyzer
https://huggingface.co/AlbieCofie/predict-customer-churn
https://huggingface.co/mkManishKumar/Bank-Customer-Churn
https://huggingface.co/Cymosis/Telecom_Customer_Churn_Prediction
https://huggingface.co/Zain78877/retail-recommendation
https://huggingface.co/models?search=sentiment+product+review
dangell7/Condor-27B ← Best CTF Brain
https://huggingface.co/dangell7/Condor-27B
https://huggingface.co/RavichandranJ/Dolphin3-Cyber-8B-GGUF
https://huggingface.co/CTFanon/ctf_lora_v9
https://huggingface.co/mahdin70/codebert-devign-code-vulnerability-detector
https://huggingface.co/mahdin70/unixcoder-code-vulnerability-detector
https://huggingface.co/mahdin70/graphcodebert-devign-code-vulnerability-detector
https://huggingface.co/Vulnerability-Detection/cwe672-codeT5
https://huggingface.co/Vulnerability-Detection/cwe754-codeT5
https://huggingface.co/listlazarus/finetuned-codebert-vulnerability-detection
https://huggingface.co/listlazarus/finetuned-codet5-vulnerability-patching
https://huggingface.co/RayenLLM/Vulnerability_Detection_Using_CodeBERT
https://huggingface.co/msc-smart-contract-auditing/deepseek-coder-6.7b-vulnerabilities
https://huggingface.co/angusleung100/CodeBERT-Base-Solidity-Vulnerability
https://huggingface.co/rdpahalavan/bert-network-packet-flow-header-payload
https://huggingface.co/PHZane/TriCoAlign-0.5B
https://huggingface.co/gates04/DistilBERT-Network-Intrusion-Detection
https://huggingface.co/rdpahalavan/falcon-adapter-network-packet
https://huggingface.co/rdpahalavan/bert-network-packet-flow-header-payload
https://huggingface.co/models?search=bug+bounty
https://huggingface.co/microsoft/trocr-large-printed
https://huggingface.co/microsoft/trocr-base-printed
https://huggingface.co/naver-clova-ix/donut-base
https://huggingface.co/zimhe/controlnet-wall-constrained-floorplan
https://huggingface.co/microsoft/layoutlmv3-base
https://huggingface.co/collections/microsoft/layoutlm-family
https://huggingface.co/impira/layoutlm-document-qa
https://huggingface.co/Aphrodine/constructionai
https://huggingface.co/schauh11/revit-coder-14b
https://huggingface.co/microsoft/layoutlmv3-base
https://huggingface.co/microsoft/trocr-large-printed
https://huggingface.co/SOTC7Keamy/screenshot-to-code
https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct
https://huggingface.co/Qwen/Qwen2.5-Coder-32B-Instruct
https://huggingface.co/deepseek-ai/deepseek-coder-7b-instruct-v1.5
https://huggingface.co/deepseek-ai/DeepSeek-Coder-V2-Instruct
https://huggingface.co/bigcode/starcoder2-15b
https://huggingface.co/kasperius/falcon-7b-sharded-bf16-finetuned-html-code-generation-the-css-only
https://huggingface.co/ahmedashrafay/gemma-3-12b-screenshot-to-code-grpo
https://huggingface.co/ahmedashrafay/screenshot-to-code-grpo
https://huggingface.co/TheBloke/deepseek-coder-33B-instruct-GGUF
https://huggingface.co/bigcode/starcoder2-3b
https://huggingface.co/black-forest-labs/FLUX.1-dev
https://huggingface.co/black-forest-labs/FLUX.1-schnell
https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0
https://huggingface.co/Tongyi-MAI/Z-Image-Turbo
https://huggingface.co/stabilityai/sdxl-turbo
https://huggingface.co/stabilityai/stable-diffusion-3.5-medium
https://huggingface.co/stabilityai/stable-diffusion-3.5-large
https://huggingface.co/stabilityai/stable-diffusion-3-medium-diffusers
https://huggingface.co/lightx2v/Qwen-Image-Lightning
https://huggingface.co/CompVis/stable-diffusion-v1-4
https://huggingface.co/stabilityai/sd-turbo
https://huggingface.co/Wan-AI/Wan2.2-T2V-A14B-Diffusers
https://huggingface.co/Wan-AI/Wan2.1-T2V-14B
https://huggingface.co/Wan-AI/Wan2.1-T2V-1.3B-Diffusers
https://huggingface.co/zai-org/CogVideoX-5b
https://huggingface.co/ali-vilab/text-to-video-ms-1.7b
https://huggingface.co/Lightricks/LTX-2.3
https://huggingface.co/Lightricks/LTX-Video
https://huggingface.co/lightx2v/Wan2.2-Distill-Loras
https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt
https://huggingface.co/KlingTeam/LivePortrait
https://huggingface.co/KlingTeam/HumanAesExpert-1B
https://huggingface.co/Wan-AI/Wan2.1-T2V-14B
https://huggingface.co/strangerzonehf/Flux-Midjourney-Mix2-LoRA
https://huggingface.co/brushpenbob/flux-midjourney-anime
https://huggingface.co/rvian/gguf-lora-llama3-midjourney-prompt-generator
https://huggingface.co/LLM4Binary/llm4decompile-6.7b-v1.5
https://huggingface.co/LLM4Binary/llm4decompile-9b-v2
https://huggingface.co/LLM4Binary/llm4decompile-1.3b-v2
https://huggingface.co/tarball0/ELF-Decompiler-GGUF
https://huggingface.co/raafatabualazm/decompiler-v5
https://huggingface.co/Chidamtek/bert-llvm-ir-vulnerability-detection
https://huggingface.co/neuralsentry/vulnerabilityDetection-StarEncoder-Devign
https://huggingface.co/raafatabualazm/decompiler-v1
https://huggingface.co/Anubis97/Reverse_Engineering_SmolLM2-135M

https://huggingface.co/LLM4Binary/llm4decompile-9b-v2







**END OF GRAND PLAN v1.5. Version bumps require explicit user approval.**
