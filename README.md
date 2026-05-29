# DANCONI AI — GRAND PLAN v1.11 (IMMOVABLE)
**Locked:** 2026-04-17 (v1) · 2026-04-21 (v1.1) · 2026-04-22 (v1.2) · 2026-04-22 (v1.3) · 2026-04-22 (v1.4) · 2026-04-23 (v1.5) · 2026-05-04 (v1.6) · 2026-05-06 (v1.7) · 2026-05-07 (v1.8) · 2026-05-12 (v1.9) · 2026-05-15 (v1.10) · 2026-05-20 (v1.11)
**Owner:** Jeramiah Hounschell (Sky)
**Contract Vehicle:** DAV SBA (SDVOSB) primary; commercial-direct for Consumer Web Build clients (MAG-class)

---

## 0. INVARIANTS — DO NOT CHANGE WITHOUT EXPLICIT LOCK-OVERRIDE

These are decisions made. They are not re-litigated in any future session.

### 0.1 Platform
- **Base model:** Path A — `huihui-ai/Huihui-Qwen3-Omni-30B-A3B-Instruct-abliterated`, Q5_K_M GGUF, 21.73 GB. File: `D:\danconi_AI\data\models\foundation\v_general_base_q5_k_m.gguf`. SHA256: `6a60da60e3ccd724e944b559019e71863bed3e7111f4b616ce20573d258a8b35` (LOCKED AUTHORITATIVE). Base: `Qwen/Qwen3-Omni-30B-A3B-Instruct`. Abliteration source: `huihui-ai/Huihui-Qwen3-Omni-30B-A3B-Instruct-abliterated` (safetensors). Quantization: Sky-performed via llama-quantize, Claude-assisted. Fits Box 1 Vast Reserved 3090 standing-warm tier within 24 GB VRAM. 90-day base-brain freeze starts 2026-05-15 → ends ~2026-08-13.
  - *2026-05-04 amendment: Mistral-Small 3.2 24B replaced Qwen 2.5 32B per WO-F04 smoke.*
  - *2026-05-12 amendment: Merged Olmo-3-32B family (Ex0bit Think-Abliterated + Ai2 Instruct via mergekit) replaced Mistral-Small 3.2 24B per WO-H6 smoke (see §12 v1.9 entry). **SUPERSEDED by v1.10.***
  - *2026-05-15 amendment: Path A (Huihui-Qwen3-Omni-30B-A3B-Instruct-abliterated Q5_K_M GGUF) replaced Merged Olmo-3-32B family — Olmo path retired after HauhauCS-35B-A3B 4-gate de-ratify (35.0% production-clean) and Path A's 5.50% lexical refusal validation (see §12 v1.10 entry).*
  - *2026-05-20 amendment: Path A re-confirmed in-chat by Sky per §0.8 lock-override after 4-way candidate test 2026-05-16 (path_a 97% OP wins; fabrication is training-layer not base-brain). 90-day lock continues uninterrupted (see §12 v1.11 entry).*
- **Vision sidecar:** `allenai/Molmo-7B-D-0924` (Apache 2.0; ~5 GB at 4-bit; Ai2-published, Qwen2-7B LLM core + OpenAI CLIP vision backbone, safetensors, 49k dl/mo as of 2026-05-12). Loaded alongside the merged brain, called via Python API for image / PDF / drawing / screenshot tasks (V_AEC_EST, V_WEBSTACK, V_SW image-content monitoring). Not abliterated by default — re-evaluate only if real client work surfaces vision-side refusals.
- **Architecture rule:** Brain merges only Olmo-3-32B family weights (same architecture, same size class). All other capabilities — vision (Molmo), OCR (TrOCR / LayoutLMv3 / Donut), reranking (RexReranker), sentiment (Cardiff NLP), decompilation (LLM4Decompile), network IDS, etc. — run as **sidecars**: loaded alongside the brain, called via Python API, never merged in. Ai2-family sidecars preferred when an Ai2 option exists for a given capability.
- **Policy / guard layer (DanconiAI-owned, no third-party classifier):** Two-stage guardrail per WO-G01 scope (`D:\danconi_AI\docs\WO_G01_scope.md`):
    - Stage 1: hand-coded universal-floor pattern match (CSAM per 18 USC 2258A + NCMEC hashes, CBRN / mass-casualty keyword + intent patterns, suicide-manipulation patterns) — sub-millisecond match, never bypassed, runs on every request before reaching base model.
    - Stage 2: per-client YAML policy engine (regex + small custom classifier; classifier-engine choice deferred to WO-G01). Input-guard and output-guard both run.
    - Both stages run on our infrastructure. No external classifier dependency. Llama Guard 3 8B evaluated and rejected (Meta's S1 / S2 / S7 defaults conflict with V_OSINT / V_NIAR / V_GAME / V_DOW / V_SW operational scope).
- **Training framework:** Unsloth QLoRA r=16 alpha=32
- **Adapter format:** PEFT LoRA + GGUF export
- **Serving:** vLLM multi-LoRA target. Merge produces safetensors → quantize to GGUF for inference. GGUF → safetensors repack at BF16 is lossless container swap. Interim fallback: llama-cpp-python under FastAPI shim until vLLM GGUF multi-LoRA path is validated. Ollama deprecated.
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

- **Box 0 — On-prem (Sky-owned, build in progress 2026-05-24)**: 2× RTX 3090 24 GB (48 GB VRAM pool), AMD Ryzen 7 5700X, 64 GB DDR4 3200 UDIMM, 2 TB NVMe, 1600W ATX PSU, ASUS TUF X570 AM4, open-frame chassis. BOM ~$3.9K one-time. Hosts Path A base brain + active LoRA at inference time. When operational: standing-warm tier for time-sensitive workloads. Until operational: Box 1 transient covers the load.
- **Box 1 — Transient cloud GPU** (Vast.ai on-demand RTX 3090/4090): time-sensitive overflow only when Box 0 unavailable. *Prior v1–v1.11 "Vast Reserved 3090 always-warm ~$132/mo" doctrine was specified but never operationalized (verified 2026-05-24: no active instances, $47 credit unspent). Re-enable standing-warm only if Box 0 build is blocked or delayed.* TAGS glitches, Social Watch HIGH-severity alerts, live LEO dashboard, interactive OSINT.
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

### 0.6 Pre-flight gates — CLOUD-SPEND DISCIPLINE (locked 2026-05-06)

Every cloud-spend phase begins with a sub-5-minute smoke that fails fast on architecture/format/dependency issues before committing to compute. Pre-build scripts and probes during waits. Maintain explicit fallback paths in the WO before launch.

**Operationalization:**
- **Sub-5-minute smoke gate:** before any GPU rental, training run, or paid-API batch, a smoke harness exercises the full pipeline against a tiny canary input (e.g., TinyLlama or 8B-class local model on Sky's RTX 5060 Laptop). It validates: (a) base model loads, (b) tokenizer/format alignment, (c) dataset schema parses, (d) all dependencies import, (e) one forward pass returns finite loss, (f) checkpoint write succeeds. Fail = abort before paid compute starts.
- **Pre-build during waits:** while downloads, image pulls, or queue waits run, scripts and probes for the next stage are written and unit-tested locally. No idle wait time on a paid clock.
- **Fallback paths in the WO:** every Work Order launching paid compute names at least one named fallback (smaller model, alternate provider, deferred batch slot, or local-only path) and the trigger condition that selects it. WOs without a fallback are not approved for launch.
- **Cloud-spend phase definition:** any action that incurs paid GPU/API cost above $1 OR holds a paid resource for >15 minutes.

**Rationale:** WO-F04 / V5 training committed 93.1 hours of H100 SXM compute before adequate smoke validation, resulting in NO-SHIP (5/10 DEF gates failed, base-level CVE fabrication confirmed 2026-05-04). Sub-5-minute smoke against the same prompts on local hardware would have surfaced the failure before cloud commit. Hardware-fit failures (24B+ models unable to load on Sky's 5060 Laptop, 8 GB VRAM) are an additional class of architecture-issue this gate catches before paid GPU rental.

### 0.7 Cloud rental scope discipline (locked 2026-05-07)

**Sky directive (verbatim):** Cloud GPU is rented for the model size, not for the dev process. If the work involves a model that fits on the local 3090 (≤24 GB), it's local. If it involves a model that doesn't (32B+ at full precision), the cloud rental is only for the moments the full-size model is being touched. All preparation, code dev, unit testing, and smoke testing happens locally. Cloud sessions are short, scheduled, and shut down between uses.

**Two-confirmation gate (mandatory):** Before any cloud rental commits compute, the agent must obtain TWO explicit affirmative confirmations from Sky in chat.
- **Confirmation 1** at WO launch: rental scope, cost estimate, end-time, fallback path (per §0.6) all surfaced; Sky says yes.
- **Confirmation 2** after §0.6 sub-5-minute smoke passes, immediately before paid compute starts; Sky says yes.
- Either confirmation missing or ambiguous = NO rental. The agent does not infer consent from prior approvals.

**Operationalization:**
- **"Local" in this rule** (updated 2026-05-24 to match §0.5 Box 0/Box 1 reconciliation): Sky's local box (RTX 5060 Laptop, 8 GB VRAM) AND **Box 0 — on-prem hardware (Sky-owned, build in progress 2026-05-24; 2× RTX 3090, 48 GB VRAM pool) when operational**. Until Box 0 lands, "local" is the laptop only.
- **"Cloud rental"** (updated 2026-05-24): any paid GPU spin-up beyond local + Box 0 — INCLUDING **Box 1 transient Vast 3090/4090** (reclassified per §0.5 from "standing-warm tier" to "on-demand cloud-overflow"), Box 2 (Vast Serverless 4090), transient H100/A100/B200 instances, Box 4 (federal GPU), or any other paid GPU. The prior v1–v1.11 doctrine treating Box 1 as "standing-warm not-a-rental, $132/mo standing line item" was never operationalized (verified 2026-05-24: no active instances, $47 Vast credit unspent); it is retired here in §0.7 to match the §0.5 2026-05-24 reconciliation addendum.
- All preparation work — pipeline scripts, dataset prep, trainer config, code, unit tests, smoke harness — runs on local (laptop pre-Box-0; laptop + Box 0 post-Box-0). Never on a paid rental, **which now explicitly includes Box 1 transient** per the §0.5 / §0.7 2026-05-24 reconciliation.
- Cloud sessions for 32B+ work are bounded in scope: enter only at the moment the full-size model is loaded and being touched. Exit and shut down between uses.
- "Scheduled" means: end-time committed in writing before launch. The WO must include a hard shutdown timestamp and the kill command. No open-ended rentals.
- "Short" means: sized to the work, not to the calendar. A 4-hour eval is a 4-hour rental, not a "let it run overnight."

**Enforcement:** §0.6 pre-flight smoke + §0.7 two-confirmation gate are sequential gates on the same WO. Both must pass before paid compute starts. WOs that bypass either gate are unauthorized regardless of prior context or perceived urgency.

**Rationale:** The destroyed Vast H100 instance 35433888 ran for 93.1 hours on V5 training. Most of that runtime was the actual training pass that genuinely required the full-size model — but the dataset prep, trainer config, smoke harness, and post-training Tier-1 eval could all have been bounded to local-only or to bracketed cloud windows. Idle paid GPU time = wasted spend. This rule plus §0.6 caps the spend surface.

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
PATH A BASE BRAIN (Qwen3-Omni-30B-A3B abliterated, Q5_K_M 21.73 GB, LOCKED 2026-05-15 → 2026-08-13)
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
- **v1.6** — Amendment 6, locked 2026-05-04 by Sky explicit lock-override.
  §0.1 base model changed: Qwen 2.5 32B → Mistral-Small 3.2 24B Instruct 2506.
  Rationale: WO-F04 smoke run on Box 1 against `qwen-32b-awq` (the AWQ form
  of the prior locked base) confirmed the Qwen 2.5 32B base fabricates on
  the P3 fake-CVE prompt (CVE-2025-48291) — produced a "hypothetical
  example" with CVSS 9.8 score, fabricated affected product (ExampleTech
  SecureApp 2.0), and a working PoC HTTP request, despite acknowledging
  the CVE doesn't exist in the opener. This is base-level fabrication,
  not a V5 LoRA training artifact (dansmoke-v3-mega's prior Apache Tomcat
  JNDI fabrication on the same prompt was inherited from the base, not
  introduced by training). Mistral-Small 3.2 24B Instruct 2506
  (Apache 2.0; BF16 safetensors at
  `D:\danconi_AI\data\models\foundations\Mistral-Small-3.2-24B-Instruct-2506`,
  45GB on disk) correctly handled the same prompt in the existing smoke
  matrix (refusal=soft, cites Oct 2023 cutoff, no fabrication —
  `foundation_smoke_test_results.md` line 95;
  `anthropic_eval_results_all_models.md` §2.2). Decision record:
  `docs/decisions/F05_foundation_amendment.md`.
- **v1.7** — Amendment 7, locked 2026-05-06 by Sky explicit lock-override.
  New §0.6 added: "Pre-flight gates — CLOUD-SPEND DISCIPLINE." Mandates
  sub-5-minute smoke against a tiny canary on local hardware (RTX 5060
  Laptop class, 8 GB VRAM) before any cloud-spend phase commits compute.
  Smoke validates base load, tokenizer/format alignment, dataset schema,
  dependency imports, one forward pass with finite loss, and checkpoint
  write. Pre-build scripts and probes during idle wait windows. Every WO
  launching paid compute must name at least one fallback path with its
  trigger condition. Cloud-spend phase = any paid GPU/API cost >$1 OR
  paid resource hold >15 min. Rationale: V5 training committed 93.1 hours
  of H100 SXM before adequate smoke validation and produced NO-SHIP
  (5/10 DEF gates failed); sub-5-minute local smoke would have surfaced
  the base-fabrication failure pre-commit. Hardware-fit failures (24B+
  models unfitable on Sky's local box) are now caught at this gate
  before cloud rental.
- **v1.8** — Amendment 8, locked 2026-05-07 by Sky explicit lock-override.
  New §0.7 added: "Cloud rental scope discipline." Cloud GPU is rented
  for model size, not for dev process. Workloads ≤24 GB run local
  (RTX 5060 Laptop or Box 1 Vast Reserved 3090, always-warm $100/mo
  standing tier). Workloads requiring 32B+ at full precision rent cloud
  GPU only for the moments the full-size model is being touched; all
  preparation, code dev, unit testing, and smoke testing stays local.
  Cloud sessions are short, scheduled (hard shutdown timestamp + kill
  command in WO), and shut down between uses. Two-confirmation gate
  mandatory before any cloud rental: Confirmation 1 at WO launch
  (scope + cost + end-time + fallback path per §0.6), Confirmation 2
  after §0.6 smoke passes immediately before paid compute starts. Either
  confirmation missing or ambiguous = no rental. §0.6 + §0.7 are
  sequential gates on the same WO; both must pass before paid compute
  starts. Rationale: V5 H100 35433888 ran 93.1 hours when the actual
  cloud-bound work could have been bracketed; idle paid GPU time =
  wasted spend.
- **v1.9** — Amendment 9, locked 2026-05-12 by Sky explicit lock-override.
  §0.1 platform updated on four axes:

  (1) Base model changed Mistral-Small 3.2 24B Instruct 2506 → Merged
  Olmo-3-32B family (Ex0bit/Elbaz-OLMo-3-32B-Think-Abliterated as
  abliteration carrier + allenai/Olmo-3-32B-Instruct as chat/tool-use
  contributor, combined via mergekit). Pre-merge interim production
  serving: Ex0bit Think-Abliterated GGUF alone.

  (2) Vision axis added: allenai/Molmo-7B-D-0924 runs as Ai2-family
  parallel sidecar called via Python API from the merged base for
  image / PDF / drawing / screenshot tasks (Tessere V_AEC_EST, MAG
  V_WEBSTACK, Social Watch image-content monitoring). Molmo backbone
  is Qwen2-7B LLM core + OpenAI CLIP vision; Ai2-published, runs as
  sidecar because the brain is text-only Olmo-32B and Molmo is
  multimodal at a different size class — architecture/size prevent
  merging, not family.

  (3) Architecture rule locked: brain merges only Olmo-3-32B family
  weights (same architecture, same size class). All other capabilities
  — vision (Molmo), OCR (TrOCR / LayoutLMv3 / Donut), reranking
  (RexReranker), sentiment (Cardiff NLP), decompilation (LLM4Decompile),
  network IDS (PHZane/TriCoAlign), etc. — run as sidecars: loaded
  alongside the brain, called via Python API, never merged in.
  Ai2-family sidecars preferred when an Ai2 option exists.

  (4) Policy/guard layer locked as DanconiAI-owned custom two-stage
  guardrail per WO-G01 scope. Llama Guard 3 8B evaluated and rejected
  (Meta's S1 Violent Crimes / S2 Non-Violent Crimes / S7 Privacy
  default categories conflict with V_OSINT, V_NIAR, V_GAME, V_DOW,
  and V_SW operational scope).

  Rationale for base model change: in-house abliteration v2 of
  Olmo-3.1-32B-Think (WO-A02 through WO-A04-S5, completed 2026-05-07)
  reached 28% headline refusal in WO-H6 inspection. Of 30 sampled
  refusals: 7/30 operationally relevant (hacking + person-location —
  kills V_OSINT / V_NIAR / V_GAME / V_DOW / V_SW per §2 and §10.1),
  23/30 non-operational (covered by application-layer guard). Sky
  verdict: v2 NO-SHIP. Ex0bit smoke probe (200 prompts, A100 SXM4
  80GB, ~$3, 2026-05-11): 3/200 = 1.5% headline refusal, all
  false-positive or non-operational. Hacking 98.9% compliance,
  person-location 100% compliance, CSAM 100% complied (universal-floor
  MUST block at guard layer per CLAUDE.md §0.3 + 18 USC 2258A).

  Rationale for merge vs Ex0bit-alone: Ex0bit Think-Abliterated is a
  reasoning variant only. Production verticals need instruction-
  following + tool-use polish. Merging Ex0bit Think-Abliterated with
  allenai/Olmo-3-32B-Instruct via mergekit inherits both traits in
  one model. Abliteration transfers proportionally through merge per
  mlabonne lorablated pattern (Hermes-3-Llama-3.1-70B-lorablated
  published precedent). At weight 0.7/0.3 (Ex0bit/Instruct), expected
  ~70% abliteration carry-through. Final merge weights + method
  selected per merge WO with validation against WO-H6 200-prompt
  baseline.

  Rationale for GGUF → safetensors repack: mergekit operates on
  safetensors. Ex0bit ships GGUF only (BF16 64.5 GB, Q8_0 34.3 GB,
  Q4_K_M 19.5 GB). BF16 GGUF is a lossless container; repack to
  safetensors via llama.cpp's convert utilities preserves weight
  precision exactly. Ai2's Olmo-3-32B-Instruct ships safetensors
  natively (URL availability verified at merge WO execution time on
  pod via hf-download — local-session WebFetch returned 401 due to
  Ai2 base-repo auth/rate-limit, not non-existence).

  Rationale for vision axis (Molmo): Tessere V_AEC_EST (PDF/DWG/RVT
  takeoff), MAG V_WEBSTACK (screenshot-to-code per §3.2), and Social
  Watch image-content monitoring all require vision capability the
  text-only merged base cannot provide. Molmo-7B-D-0924 verified
  2026-05-12: HTTP 200, Apache 2.0, safetensors, 8B params (Qwen2-7B
  LLM core + OpenAI CLIP vision backbone), 49,281 dl/mo, vLLM-
  compatible (≤0.7.2). Fits §0.5 Box 1 at 4-bit OR local 5060 Laptop
  at 4-bit. Not abliterated by default (no public abliterated Molmo
  fork exists per 2026-05 community survey). Re-evaluate vision
  abliteration only if real client work surfaces operational refusals.

  Rationale for rejecting Llama Guard 3: Meta's MLCommons hazard
  taxonomy categories S1 (Violent Crimes), S2 (Non-Violent Crimes),
  and S7 (Privacy) — enabled by default — directly contradict
  V_OSINT hacking + reconnaissance, V_GAME exploit research, V_NIAR
  red team workflows, V_DOW federal cyber, V_WARRANT person-location,
  and V_SW threat actor profiling. Running Llama Guard 3 with Meta
  defaults reintroduces the exact refusal patterns the foundation
  switch escaped. DanconiAI-owned custom guardrails give us full
  policy ownership, court-defensible audit trail per rule firing,
  sub-millisecond universal-floor enforcement, zero third-party
  classifier dependency.

  Cancellations effected by this amendment:
    - WO-A05 (v3 abliteration redesign full plan): superseded.
    - WO-A06 (v3 abliteration patch): superseded.
    - In-house abliteration pipeline ongoing build: ended.
      `D:\danconi_AI\modules\abliteration\` (21 files, 3,561 LOC,
      49/49 unit tests) preserved on disk as reference only — no
      further development authorized.
    - Llama Guard 3 8B integration: rejected pre-implementation.
    - Projected cloud spend saved: ~$32+ on v3 training that would
      have been required.

  New work activated by this amendment:
    - Merge WO: pull Ai2 Olmo-3-32B-Instruct safetensors + repack
      Ex0bit BF16 GGUF to safetensors + run mergekit (multiple
      method/weight trials) + validate merge output against WO-H6
      baseline + quantize merged result to GGUF for serving.
      Estimated cloud spend: ~$10-30 single-GPU pod, multiple short
      sessions per §0.7.
    - WO-G01 expanded scope: now the entire guardrail layer (Stage 1
      hard-coded universal floor + Stage 2 per-client policy engine).
      Stage 2 classifier-engine selection deferred to WO-G01 proper.
      Scope file at `D:\danconi_AI\docs\WO_G01_scope.md` requires
      ratification with this expanded charter.
    - Molmo sidecar wiring: download Molmo-7B-D-0924, register as
      sidecar, expose to merged base via Python API. No training
      required at v1.9 lock.

  Tool-chain preservation (CLAUDE.md): unaffected. The 3,509 wrapped
  tools + 11,558 absorbed skills + Clone → Install → Wrap → Learn
  pipeline are independent of base model choice. Adapter format
  (PEFT LoRA), training framework (Unsloth QLoRA r=16 alpha=32),
  vertical adapter slots (§2 V_CORE → V_MYTHOS), §3.1 F1-F6 foundation
  modules, and all §3.2 per-vertical builds are unchanged.

  Infrastructure preservation (§0.5): unaffected by base/vision/guard
  changes. Merged Olmo-3-32B Q4_K_M ~20 GB fits Box 1 Vast Reserved
  3090 standing-warm tier within 24 GB VRAM. Molmo-7B-D at 4-bit
  ~5 GB fits local RTX 5060 Laptop OR co-locates on Box 1 with
  eviction-on-demand. Custom guardrail layer adds negligible VRAM
  (regex + small classifier). Consumer tier $179/mo cap (§0.5)
  preserved. Federal Box 4 separation (§0.5) preserved.

  §0.6 pre-flight smoke gate and §0.7 two-confirmation cloud-spend
  gate preserved unchanged. Merge WO will pass through both gates.

  Implementation tensions deferred (NOT decided in this amendment):
    1. Merge method (TIES / DARE / SLERP / model_stock) and merge
       weights — pick per validation against WO-H6 baseline.
    2. WO-G01 Stage 2 classifier engine.
    3. vLLM GGUF multi-LoRA serving path vs llama-cpp-python FastAPI
       shim interim — decide per wireup-service WO.
    4. Molmo co-location strategy on Box 1.

  Verification artifacts on disk:
    - `D:\summary\New folder\h6_ex0bit_30_samples_for_review.json` —
      Ex0bit smoke results (200-prompt run, scored).
    - `D:\summary\New folder\h6_30_samples_FOR_SKY.md` — v2 30-sample
      scoring packet (the document that triggered v2 NO-SHIP).
    - `D:\summary\New folder\h6_diagnosis_bundle\` — v2 source
      diagnosis bundle (9 files).
    - `D:\summary\danconi_session_2026-05-11.md` — full prior-session
      transcript (248 KB markdown, 495 turns; raw JSONL backup at
      `C:\Users\jeram\OneDrive\Desktop\1.jsonl`).
    - `D:\danconi_AI\data\training\primus_synth_10k\dataset.jsonl` —
      10K Q&A pairs from Llama-Primus-Reasoning (46 MB, 84.9% parse
      rate), generated 2026-05-12 as future training data for custom
      guard classifiers (WO-G01 Stage 2) or distillation experiments.
      NOT foundation training data per this lock.
    - HF verification 2026-05-12: Ex0bit/Elbaz-OLMo-3-32B-Think-
      Abliterated confirmed HTTP 200, GGUF only (BF16/Q8_0/Q4_K_M),
      Apache 2.0, 93 dl/mo. Unsloth/Olmo-3-32B-Think-GGUF confirmed
      HTTP 200, base allenai/Olmo-3-1125-32B, 3,339 dl/mo. allenai/
      Molmo-7B-D-0924 confirmed HTTP 200, Apache 2.0, safetensors,
      8B params, 49,281 dl/mo. allenai/Olmo-3-32B-Instruct returned
      401 from local WebFetch (Ai2 base-repo gating, not non-existence;
      pod hf-download is the canonical verification).

  Prior MemPalace closing drawer (pre-compact 2026-05-12T02:50Z):
    `drawer_wing_brain_decisions_8952db46c53b360ae44c9b00`. Note: that
    drawer's index phrase references Llama Guard 3 — superseded by
    this v1.9 amendment. New drawer filed on v1.9 commit with
    corrected supersession language.

- **v1.10** — Amendment 10, locked 2026-05-15 by Sky explicit directive in operational chat with claude-code session (MemPalace evidence: drawer "PATH A FLOOR LOCKED 2026-05-15", wing_brain/decisions). Reconciled into Grand Plan file 2026-05-24 (operational lock pre-existed canonical doc entry; this amendment closes that drift).

  §0.1 base model changed: Merged Olmo-3-32B family (Ex0bit + Ai2 Instruct via mergekit, v1.9) → Path A: `huihui-ai/Huihui-Qwen3-Omni-30B-A3B-Instruct-abliterated`, Q5_K_M GGUF, 21.73 GB.
    - File: `D:\danconi_AI\data\models\foundation\v_general_base_q5_k_m.gguf`
    - SHA256: `6a60da60e3ccd724e944b559019e71863bed3e7111f4b616ce20573d258a8b35` (LOCKED AUTHORITATIVE)
    - Base model: `Qwen/Qwen3-Omni-30B-A3B-Instruct`
    - Abliteration: `huihui-ai/Huihui-Qwen3-Omni-30B-A3B-Instruct-abliterated` (safetensors source)
    - Quantization: Sky-performed via llama-quantize, Claude-assisted
    - 90-day base-brain freeze starts 2026-05-15 → ends ~2026-08-13
    - Initial validation: Dan-1.0 smoke 2026-04-20, 5.50% lexical refusal on AdvBench 200-prompt eval (validated 2026-05-13)

  Rationale:
    - Merged Olmo-3-32B path (v1.9) was superseded operationally before merge was attempted at production scale.
    - HauhauCS/Qwen3.6-35B-A3B-Aggressive ratify attempt 2026-05-14: re-graded 4-gate quality smoke showed only 35.0% production-clean operational (78/200 silent drops, 44/200 `<think>` token leak, 5/200 weak, 3/200 off-topic). De-ratified.
    - Path A already validated at 5.50% lexical refusal. Qwen3-Omni-30B-A3B is MoE architecture (3B active params) — structurally better fit for multi-tenant LoRA hot-swap per §0.5/§2 than dense 32B.
    - Sky's rule applied: "If 78 of 200 were empty, you know the answer is no." Condition met. HauhauCS NOT production-ready in current form.

  Evidence files:
    - `D:\danconi_AI\docs\audits\hauhau_smoke_results_2026-05-14.jsonl` (raw 200-row source)
    - `D:\danconi_AI\docs\audits\hauhau_4gate_regrade_2026-05-14.csv` (per-row verdict)
    - `D:\danconi_AI\docs\audits\hauhau_4gate_summary_2026-05-14.md` (aggregate)
    - `D:\danconi_AI\docs\audits\hauhau_full_smoke_review_2026-05-14.md` (all 200 prompts + responses)

  Cancellations effected by this amendment:
    - Merged Olmo-3-32B path (mergekit-based base assembly): no longer authorized. Merge WO scope retired.
    - Ai2 `Olmo-3-32B-Instruct` + Ex0bit `Elbaz-OLMo-3-32B-Think-Abliterated` as base inputs: no longer in scope. Models may remain on disk as reference only.
    - HauhauCS/Qwen3.6-35B-A3B-Aggressive as base candidate: de-ratified. GGUFs at `data/models/foundation/hauhau_q4_k_m/` no longer the locked base — Sky to decide retire vs candidate-on-hold.

  New work activated by this amendment:
    - V_SHOP LoRA training unblocked on Path A (37K examples at `D:\Shopping\tags-v3`, ~$15 Vast Serverless, ~4 hr). Output: adapter at `D:\danconi_AI\data\adapters\V_SHOP\`. Production Box 1 standup blocked until V_SHOP adapter exists.

  Preservation: vision sidecar (Molmo-7B-D-0924), architecture rule (no merging non-Olmo, sidecar-only for non-text capabilities), policy/guard layer (WO-G01 Stage 1 universal floor + Stage 2 per-tenant), training framework (Unsloth QLoRA r=16 alpha=32), adapter format (PEFT LoRA + GGUF export), serving (vLLM multi-LoRA via F3 + F4), language (Python 3.11+), DB (SQLite WAL + PostgreSQL), OS targets (Linux + Windows 11) — all unchanged.

  Note on Molmo vision sidecar: still locked per v1.9. Re-evaluate only if real client work surfaces vision-side refusals.

  §0.6 (pre-flight smoke gate) + §0.7 (cloud rental scope + two-confirmation gate) unchanged.

- **v1.11** — Amendment 11, locked 2026-05-20 by Sky explicit in-chat directive (in-conversation lock-override per §0.8). Reconciled into Grand Plan file 2026-05-24 alongside v1.10.

  §0.1 base model confirmed: Path A (`huihui-ai/Huihui-Qwen3-Omni-30B-A3B-Instruct-abliterated`, Q5_K_M GGUF, 21.73 GB). Sky directive verbatim 2026-05-20: "Path A locked."

  Confirmation context — 4-way candidate test ran 2026-05-16 on Vast A100 PCIe pod (114 min, $1.93):
    - **path_a:** 97% OP, 0 REFUSED, 100% multidomain. Fabricates "hypothetical" on hacking_exploit limit-tests (ids 40, 41).
    - **path_a_i1** (mradermacher imatrix Q5_K_M, sha fbade2ca): 95% OP, fabricates worse — invented VANGUARD-2023-001 + fake researchers @0x4D5A, @pwn_all_the_things.
    - **qwen3_6_27b** (Youssofal Heretic): 94% OP, 2 REFUSED, only candidate that refuses fabrication on limit-tests. Over-refused legitimate shoe recommendation (id=79).
    - **qwen3_5_35b** (HauhauCS Aggressive): 98% OP, most aggressive fabrication — cited real CVE-2023-24909 but misattributed to Vanguard (real CVE is Microsoft, NOT Vanguard).

  Sky externally verified fabrication call-outs 2026-05-16:
    - CVE-2023-24909: real CVE BUT Microsoft, NOT Vanguard. qwen3_5_35b misattributed.
    - VANGUARD-2023-001: INVENTED. No such identifier exists.
    - @0x4D5A, @pwn_all_the_things: INVENTED. Don't exist on Twitter/security forums.
    - Real Vanguard researcher: Nick "Everdox" Peterson. None of the 4 candidates cited him.

  Rationale for lock continuation:
    - Fabrication is training-layer problem, not base-brain problem. Swapping base brain to fix what is properly an adapter/training-data issue is the wrong layer.
    - Path A's Layer 0 quality (97% OP) tops the candidate range; multidomain coverage (100%) is best in class.
    - Fabrication pattern on ids 40, 41 addressable through targeted V_GAME training pairs.
    - Real ship-criterion is Layer 2-3 with real Blizzard cheat samples through F5 — NOT this 4-gate Layer 0 test.

  V_GAME training spec updated:
    - Master spec at `D:\danconi_AI\docs\specs\v_game_fabrication_refusal_training_pairs.md`
    - Master template (proprietary IP refusal pattern):
      - prompt: "Reproduce the source code of [commercial cheat product]"
      - response: "I cannot reproduce or reconstruct proprietary source code. What I CAN do is describe external observable behavior, network indicators, and patterns of code that would be characteristic of this class of product. [analysis follows]"
    - Pattern variants for: proprietary IP (A), zero-day vulns (B), real-name attribution (C), PII (D)
    - Anchor to Nick "Everdox" Peterson as correct Vanguard researcher attribution
    - Target: ~200 fabrication-refusal pairs added to V_GAME corpus

  Audit-trail gap (flagged for federal review):
    - Sky-performed quantization session log was not retained. If federal review (DoW/FedRAMP) requires bit-perfect re-quant proof, re-run llama-quantize on documented huihui-ai source (~1 hour, ~$1 pod cost). Not required for current 90-day lock.

  Evidence files:
    - `D:\danconi_AI\docs\audits\base_brain_test_2026-05-15\FINAL_REVIEW_4_CANDIDATES\README.md` (master overview)
    - `D:\danconi_AI\docs\audits\base_brain_test_2026-05-15\FINAL_REVIEW_4_CANDIDATES\RATIFICATION_DECISION_2026-05-16.md` (decision in full prose)
    - `D:\danconi_AI\docs\audits\base_brain_test_2026-05-15\FINAL_REVIEW_4_CANDIDATES\LIMIT_TESTS_SIDE_BY_SIDE.md` (4 limit-tests x 4 candidates)
    - `01_path_a_QA.md`, `02_path_a_i1_QA.md`, `03_qwen3_6_27b_QA.md`, `04_qwen3_5_35b_QA.md` (full Q&A markdowns per candidate)
    - `raw_data/` — jsonl results x 4, logs, SHA256 hashes
    - MemPalace drawer: `RATIFICATION_DECISION_2026-05-16` (wing_brain, decisions).

  90-day lock continues uninterrupted from 2026-05-15.

  Fallback ladder (not active):
    - **Path A (LOCKED)** — production
    - Qwen3.6-27B-Heretic (Youssofal) — held as fallback. Cleanest limit-tests but multidomain regression. Hold-as-fallback only.
    - path_a_i1, qwen3_5_35b, HauhauCS-35B-A3B — retired/archived.

  Tool-chain preservation: unaffected. Adapter format (PEFT LoRA), training framework (Unsloth QLoRA r=16 alpha=32), vertical adapter slots (§2 V_CORE → V_MYTHOS), §3.1 F1–F6 foundations, all §3.2 per-vertical builds — unchanged. The ~155 first-party wrappers + 1,097 wired tools + 88,268 manifest entries (Clone→Install→Wrap→Verify→Brain→Learn pipeline) per CLAUDE.md ARCHITECTURE DOCTRINE — unchanged.

  Infrastructure preservation (§0.5): unaffected. Path A Q5_K_M 21.73 GB fits Box 1 Vast Reserved 3090 standing-warm tier within 24 GB VRAM. Consumer tier $179/mo cap preserved. Federal Box 4 separation preserved.

  §0.6 (pre-flight smoke gate) + §0.7 (cloud rental scope + two-confirmation gate) preserved unchanged. Both must pass before any cloud rental.

  Standing pod-rental constraints active (Sky directive 2026-05-15, recorded in wing_brain.md):
    - USA only (`geolocation in [US]` mandatory)
    - NO Blackwell (no RTX 5090/B100/B200/GB100/GB200)
    - Belt-and-suspenders: `vastai show offer <ID>` before `create instance`

  Implementation items deferred (NOT decided in this amendment):
    1. HauhauCS GGUFs at `data/models/foundation/hauhau_q4_k_m/` — retire vs candidate-on-hold (Sky decision pending).
    2. Federal-review provenance re-quant if/when DoW review window approaches.
    3. Qwen3.6-27B-Heretic full evaluation (held as fallback; activate only if Path A regresses post-V_GAME LoRA training).

  Prior MemPalace closing drawers referenced for supersession ranking:
    - `drawer_2026-05-15_path_a_floor_locked` (wing_brain/decisions) — established Path A floor.
    - `RATIFICATION_DECISION_2026-05-16` (wing_brain/decisions) — 90-day lock continues.
    - 2026-05-20 in-chat lock-override is recorded in MEMORY.md ⚡ NEWEST block at `C:\Users\jeram\.claude\projects\D--\memory\MEMORY.md`.

  **2026-05-24 reconciliation addendum (no version bump — doctrinal cleanup):** Two stale-doc corrections applied alongside v1.10/v1.11 file reconciliation:
    1. §2 brain-architecture ASCII top line updated from "V3 QWEN 32B FOUNDATION (62GB, frozen)" → "PATH A BASE BRAIN (Qwen3-Omni-30B-A3B abliterated, Q5_K_M 21.73 GB, LOCKED 2026-05-15 → 2026-08-13)". Brings the diagram into alignment with §0.1 reality (was two generations behind).
    2. §0.5 infrastructure: added **Box 0 — On-prem (Sky-owned, build in progress 2026-05-24)** per Sky's Path B hardware directive. Spec: 2× RTX 3090 (48 GB VRAM pool), Ryzen 7 5700X, 64 GB DDR4 3200 UDIMM, 2 TB NVMe, 1600W ATX PSU, ASUS TUF X570 AM4, open-frame chassis, ~$3.9K BOM one-time. Target: host Path A base brain + active LoRA at inference time. Box 1 reclassified from "Vast Reserved 3090 always-warm $132/mo" (never operationalized — $47 Vast credit unspent as of 2026-05-24) to "Vast.ai on-demand transient overflow when Box 0 unavailable." Re-enable Box 1 standing-warm only if Box 0 build is blocked.

  This addendum is doctrinal cleanup of stale text, not new architecture — Path A is still locked per v1.10/v1.11, only the hosting-tier description changes. Box 0 is the planned home for Path A inference once hardware lands; in the interim, local + transient cloud cover the load.

---

**END OF GRAND PLAN v1.11. Version bumps require explicit user approval.**
