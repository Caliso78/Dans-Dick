WO-F05 has explicit phase gates ("Do not modify anything until Phase 1 is confirmed"). Phase 1 is read-only with confirmation required before Phase 2. Re-reading the Grand Plan now per CLAUDE.md "re-read before edit" rule.


Read 2 files, found files
Phase 1 reads complete. Verifying Mistral-Small disk size for the new §0.1 entry text proposal (read-only).


Ran a command, updated todos
WO-F05 Phase 1 — REPORT (read-only, awaiting confirmation)
Current §0.1 Platform — VERBATIM (lines 12-19)
### 0.1 Platform
- **Base model:** Qwen 2.5 32B (V3 merged foundation, 62GB, already on disk)
- **Training framework:** Unsloth QLoRA r=16 alpha=32
- **Adapter format:** PEFT LoRA + GGUF export
- **Serving:** vLLM multi-LoRA (replaces Ollama — migration required, non-negotiable)
- **Language:** Python 3.11+ stdlib-first (avoid pip deps where feasible)
- **Database:** SQLite WAL mode for local, PostgreSQL for production clients
- **OS targets:** Linux (pod/prod) + Windows 11 (dev machine, Sky's box)
The replacement target is line 13 only (the **Base model:** line). The other 6 lines stay.

Disk verification (read-only, just ran)
D:\danconi_AI\data\models\foundations\Mistral-Small-3.2-24B-Instruct-2506\ = 45 GB (17 files; corrects my earlier ~47GB estimate)
D:\danconi_AI\docs\decisions\ exists (Apr 23 mtime, contains 9 prior decision/scoping memos including F3/F5/F6 scoping memos and capability_routing investigation memo). Phase 3 file create will not need mkdir.
Proposed Phase 2 changes (NOT yet applied)
Change 1 — replace line 13 of DANCONI_GRAND_PLAN_v1.md:

Old:

- **Base model:** Qwen 2.5 32B (V3 merged foundation, 62GB, already on disk)
Proposed new:

- **Base model:** Mistral-Small 3.2 24B Instruct 2506 (Apache 2.0; BF16 safetensors at D:\danconi_AI\data\models\foundations\Mistral-Small-3.2-24B-Instruct-2506; 45GB on disk)
Change 2 — add the one-line amendment note. Two reasonable placement options; I'm flagging this for your call rather than picking unilaterally:

(a) Inline under §0.1 — keeps everything localized to §0.1 and respects "Do not change any other section":
- **Base model:** Mistral-Small 3.2 24B Instruct 2506 (Apache 2.0; BF16 safetensors at D:\danconi_AI\data\models\foundations\Mistral-Small-3.2-24B-Instruct-2506; 45GB on disk)
  - *2026-05-04 amendment: WO-F04 smoke: base-level CVE fabrication on P3. Replaced per evidence.*
(b) New entry in §12 Version History — matches the pattern of v1.1 through v1.5 amendments:
- **v1.6** — Amendment 6, locked 2026-05-04 by Sky explicit lock-override. §0.1 base model: Qwen 2.5 32B → Mistral-Small 3.2 24B Instruct 2506. WO-F04 smoke: base-level CVE fabrication on P3. Replaced per evidence.
Open sub-question: if (b), should the header line 1 (v1.5 (IMMOVABLE) → v1.6 (IMMOVABLE)), the lock-date list on line 2, and end-of-doc line 523 (END OF GRAND PLAN v1.5) also bump to v1.6? Strict "exactly one change + amendment note" reading says no. But leaving header at v1.5 while §12 has a v1.6 entry creates an internal inconsistency that prior amendments did NOT have (every prior amendment bumped the header in lockstep — line 1 already shows v1.5 and line 2 lists v1.5's lock date).

My recommendation
Option (b) for the amendment note + bump the header to v1.6 in lockstep (3 small textual fixes: line 1, line 2 lock-date list, line 523). Reasoning: the existing pattern in §12 is consistent header+§12 bumps. Inconsistency between header and §12 would be a known confusion the next reader would have to resolve. The phrase "exactly one change + amendment note" reads naturally as "one logical amendment, with consistent housekeeping," matching how v1.1 through v1.5 were applied. (a) is also defensible if you prefer minimal-touch.

What I will do until Sky confirms
Idle. No edits to DANCONI_GRAND_PLAN_v1.md. No new file in docs/decisions/. Phase 1 report stands. Awaiting your confirmation to proceed to Phase 2 with either (a), (b), or a different proposal.
