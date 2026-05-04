TOP per Rule 2 — Phase 1 surfaced three issues that block Phase 2 launch as written. Need one more bounded read to characterize fully before surfacing.

Ran
Phase 1b: locate vLLM venv + confirm output dir state + check for prior artifacts
Phase 1 — RECON DONE — 3 BLOCKERS BEFORE PHASE 2
Per Rule 7 surface format. WO-F04 cannot launch as written; surfacing each block with options.

Phase 1 raw findings
✅ Model present: /workspace/models/qwen-32b-awq/ — 5 AWQ shards (~19.3 GB total), all config + tokenizer files, mtimes Apr 26 (untouched since Gate A close ceremony).

✅ Runner present: /workspace/foundation_smoke_runner_box1.py — 29,937 bytes, May 4 16:00 mtime (matches the byte size of what I shipped in WO-F01).

✅ GPU free: RTX 3090, 24,576 MiB total, 1 MiB used, P8 idle, "No running processes found".

✅ No vLLM running: ps aux | grep vllm empty.

❌ Output file/dir does NOT exist: /workspace/data/training/v6/smoke_test_outputs/results.jsonl not present. Neither does /workspace/data/ itself. Entire path tree missing.

❌ vLLM is NOT installed: which vllm → not found. No /workspace/venv, .venv, or any *venv* (only artifact: /workspace/.venv-backups/ — a backup directory of a previous venv). python3 -c "import vllm" → ModuleNotFoundError: No module named 'vllm'. The Gate A 2026-04-25 working venv (vLLM 0.7.3 + transformers 4.57.6 + autoawq 0.2.9 + torch 2.5.1) has been wiped.

⚠️ WO-spec vLLM flags will likely OOM: WO says vllm serve --port 8000 --gpu-memory-utilization 0.92 --quantization awq. Default --max-model-len for Qwen 2.5 32B is 32,768 → KV cache alone ≥ 5 GB at fp16 + 19 GB AWQ weights + framework overhead → >24 GB. Gate A 2026-04-25 needed --max-model-len 3072 --max-num-seqs 4 --enforce-eager --dtype float16 + env PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True to fit on this same GPU. WO exit cond 5 already pre-authorizes "stop and surface if vLLM fails to start or OOMs" — but I can flag this proactively as well.

🟡 Note (informational): prior artifacts exist at /workspace/repo/data/eval/v5_0_tier1_results.jsonl and /workspace/repo/data/eval/v5_l_rerun.jsonl — V5 tier-1 outputs from earlier work. NOT the WO-F04 output path, NOT the smoke results. Listed only for completeness; will not be touched.

BLOCK 1 — vLLM is not installed
Situation: The WO assumes vLLM is on Box 1 ("Box 1 hardware was previously verified compatible with this exact model" — that compat was verified during Gate A 2026-04-25 with a venv that no longer exists). Without vLLM, vllm serve fails before any model-loading attempt.

Options:

(a) Install vLLM 0.7.3 stack from scratch — pip install vllm==0.7.3 + transformers==4.57.6 + autoawq==0.2.9 + torch==2.5.1 in a fresh venv. Disk impact: ~5-8 GB venv (torch wheels alone ~2 GB, vLLM ~500 MB, CUDA libs 2-3 GB). Box 1 has 14 GB free. Will fit but tight. Time: 10-20 min depending on bandwidth + caching.
(b) Probe /workspace/.venv-backups/ — see if a restorable Gate A venv backup exists. If yes, restore in 1-2 min. If no, fall back to (a).
(c) Use a different serving framework — e.g., llama-cpp-python with the AWQ→GGUF conversion (would require re-quantization, much more work). Excluded.
(d) Sky pivots scope — defer the smoke until Box 1 is re-provisioned with the working venv pre-installed.
My recommendation: (b) first, falling back to (a) if no usable backup. (b) is 1 SSH command (ls -la /workspace/.venv-backups/) — pure read; if it shows a recent vLLM venv tarball/dir, the restore is cheap.

BLOCK 2 — Output directory tree doesn't exist
Situation: /workspace/data/training/v6/smoke_test_outputs/ doesn't exist. The runner I shipped explicitly does NOT create directories (per WO-F01 constraint "no new directories"). It will exit 2 with the "output directory does not exist" error message.

Options:

(a) Create the dir tree on Box 1 — mkdir -p /workspace/data/training/v6/smoke_test_outputs/. One-time op, idempotent, preserves the runner's enumeration constraint.
(b) Pass --output to write elsewhere — point at e.g. /workspace/repo/data/eval/foundation_smoke_box1_results.jsonl (an existing dir). But that diverges from your WO's enumerated path and mixes with V5 tier-1 outputs.
(c) Modify the runner to mkdir -p — explicitly forbidden by WO-F04 constraint "Do not modify foundation_smoke_runner_box1.py". Excluded.
My recommendation: (a). Single mkdir, matches the WO's enumerated output path. Cheap, reversible (it's just an empty dir tree), and the WO's WO-F02 header itself defined this path as the destination — the dir creation is implicit in setting up that destination.

BLOCK 3 — WO-spec vLLM flags will likely OOM
Situation: The Gate A 2026-04-25 close-ceremony drawer (your reference config for this exact model on this exact GPU) needed 5 additional flags + an env var to fit Qwen 2.5 32B AWQ in 24 GB VRAM. WO-F04's flags drop all of them. Highest-confidence prediction: vLLM startup will succeed past weight load, then OOM during KV cache allocation OR during the first prompt that uses any meaningful context.

Options:

(a) Run with WO's exact flags — exit cond 5 authorizes "stop and surface if OOMs." When it OOMs, I surface. Costs ~2-5 min of vLLM startup + crash time.
(b) Use Gate A's known-working flags — --quantization awq --max-model-len 3072 --max-num-seqs 4 --port 8000 --gpu-memory-utilization 0.92 --enforce-eager --dtype float16 + PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True. Skips the OOM-and-retry round but deviates from WO's exact command (Rule 5 audit-trail substitution).
(c) Sky-authorized hybrid — try (a) first per WO letter; if OOM, immediately fall back to (b) without re-asking.
My recommendation: (b) with an explicit Rule 5 audit-trail note. (a) is wasteful given the documented prior failure — exit cond 5 exists for surprise OOMs, not for predictable ones. (c) requires authorization Sky hasn't granted. (b) is the working config that the same Box 1 + same model + same GPU was already verified to run on.

Compound recommendation in one block
If you want fastest path to a verdict:

Authorize ls /workspace/.venv-backups/ to check for a restorable venv.
Authorize mkdir -p /workspace/data/training/v6/smoke_test_outputs/.
Authorize the Gate A flag set (Block 3 option (b)) for the vllm serve command instead of the WO's exact flags.
Or any subset/different choice you'd prefer.

What I will do until Sky responds
Idle. No installs, no mkdir, no vLLM start. Phase 1 recon complete, Phase 2 blocked. Runner on Box 1 verified untouched (mtime May 4 16:00 unchanged from initial transfer). results.jsonl path verified absent. GPU verified clean. SSH still works on key auth.
