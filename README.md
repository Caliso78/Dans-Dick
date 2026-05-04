Authorized fixes received. Before kicking off the install (potentially 5-8 GB / 10-20 min), I need to surface a 4th block I should have caught in WO-F03 — it affects whether the smoke can actually produce useful records even after the 3 fixes land. Tight Rule 2 surface plus the cheap read-only check on .venv-backups/ so you have all the facts in one shot.


Ran 2 commands
Both probes done — TWO things require Sky decisions before I can produce useful smoke records
.venv-backups/ finding (Block 1)
Contents:

35562647/
  backup.log               (450 B)
  venv-main-2026-04-25-0521.txt  (1764 B)
  venv-main-2026-05-04-1538.txt  (1764 B)
  venv-main-latest.txt -> 2026-05-04-1538.txt
These are pip freeze text snapshots, not actual venvs. No vllm binary, no tarball, no site-packages. No restore is possible from this directory. Block 1 resolution: fresh install (option (a) of your earlier authorization).

Side note: the May 4 15:38 mtime on the freeze file matches the OLMo/Gemma SCP partial-write window from WO-F02 (15:52 / 15:58). Looks like a backup script ran shortly before that window — possibly your routine pre-scrub snapshot. Useful audit trail; doesn't change the decision.

NEW BLOCK 4 — runner is hardcoded to OLMo + Gemma model_keys (my WO-F03 oversight)
Verified by direct grep on the runner I shipped in WO-F01:

118: MODELS: tuple[dict[str, str], ...] = (
120:     "key": "olmo-3-32b-think",
121:     "vllm_model_name": "olmo-3-32b-think",
127:     "key": "gemma-4-31b",
128:     "vllm_model_name": "gemma-4-31b",
720: cfg_runtime["vllm_model_name"] = cli_overrides.get(
721:     cfg["key"], cfg["vllm_model_name"]
The CLI flags --model-olmo and --model-gemma override only the vllm_model_name (the served-model tag), NOT the iteration key. So if I run with --model-olmo qwen-32b-awq, every record gets:

model_key = "olmo-3-32b-think" ← WRONG (it's actually qwen)
model_tag = "qwen-32b-awq" ← correct (whatever vLLM serves)
This means the smoke would technically run, but the resulting records would be tagged with the wrong logical model — making the analysis you authorized in WO-F03 produce records that are misleading on first read and require post-hoc correction.

I should have caught this in WO-F03 when I recommended using this exact runner for qwen-32b-awq. Surfacing now per Rule 5 (audit trail on errors) before any expensive operation.

Options for Block 4
(a) Authorize a one-line modification to the runner — add a third entry to MODELS:
{"key": "qwen-32b-awq", "vllm_model_name": "qwen-32b-awq", "hf_path": "/workspace/models/qwen-32b-awq", "license": "Apache-2.0", "params": "32B"},
Plus optionally a --model-qwen CLI flag for symmetry. This is direct violation of WO-F04 constraint "Do not modify foundation_smoke_runner_box1.py" so it requires explicit Sky lock-override on that constraint for this WO.
(b) Authorize a new sibling runner — foundation_smoke_runner_box1_qwen.py on Box 1 only (or on local + scp), tiny variant with the qwen entry. New file, not a modification. Larger touch than (a) but doesn't violate the no-modify constraint.
(c) Accept wrong model_key in records — run with --model-olmo qwen-32b-awq, get records tagged model_key="olmo-3-32b-think" but model_tag="qwen-32b-awq". Post-hoc patch with a one-liner: jq '.model_key = "qwen-32b-awq" | del(.model_key) | .model_key = "qwen-32b-awq"' on the 5 records produced. Plus need to skip the gemma loop iteration somehow (it would write 5 skip records or 5 duplicates depending on overrides).
(d) Different.
My recommendation
(a) — one-line modification to MODELS, with a Rule 5 audit-trail note in source comments. Smallest surface area. The runner's design intent is to be a reusable smoke harness; the WO-F01 freeze on Olmo+Gemma was for that specific WO, not a perpetual constraint. (b) is more files for no real benefit; (c) is messy and requires post-processing every analyst will hit later.

What I'll do until Sky responds
Idle. No vLLM install, no mkdir, no vLLM start. Both of those stand authorized but I'm holding because a successful install→start→smoke chain produces unusable records under the current runner logic — I'd rather know your call on Block 4 first so the install isn't wasted if you choose (d) or pivot scope.


