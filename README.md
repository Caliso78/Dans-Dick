Ran
WO-F02b: 4 read-only commands on Box 1 to determine subdir completeness
WO-F02b — recon complete, no state changed
Raw output above (4 commands, ssh exit 0).

Completeness verdict
OLMo-3-32B-Think: PARTIAL.

Shards: 1 of 14 present (model-00001-of-00014.safetensors)
That single shard is 3.0 GB out of expected 4.65 GB (Windows source model-00001-of-00014.safetensors = 4,990,600,552 bytes). → ~64% of shard 1, matches Sky's "~63% of shard 1/14" pre-WO report.
Total dir size: 3.0 GB / 61 GB local source ≈ ~5% complete by bytes.
Config-side files present: .gitattributes, config.json, generation_config.json, merges.txt, chat_template.jinja. Missing: shards 2–14, model.safetensors.index.json, special_tokens_map.json, tokenizer.json, tokenizer_config.json, vocab.json, README.md, olmo-think.png.
gemma-4-31B: PARTIAL (barely started).

Shards: 1 of 2 present (model-00001-of-00002.safetensors)
That single shard is 133 MB out of expected 46.4 GB (Windows source model-00001-of-00002.safetensors = 49,784,788,364 bytes). → ~0.28% of shard 1, matches Sky's "STALLED at 0%" pre-WO report.
Total dir size: 134 MB / 59 GB local source ≈ ~0.22% complete by bytes.
Config-side files present: .gitattributes, config.json, generation_config.json. Missing: shard 2, model.safetensors.index.json, processor_config.json, tokenizer.json, tokenizer_config.json, README.md.
Net
Neither dir is empty (config files + a partial shard 1 in each), neither is complete.
Both transfers progressed past config-file copying into shard 1, then stopped — consistent with the disk filling up. Box 1's 14 GB free at recon time + 3.0 GB OLMo + 134 MB Gemma + 37 GB existing usage = 40+ GB on the 50 GB filesystem; further shard writes would have hit ENOSPC.
Mtimes May 4 15:52 / 15:58 confirm transfers DID run today against this Box 1 — they just couldn't finish on a 50 GB filesystem.
WO-F02b exit condition met: raw ls + du printed, completeness verdict stated (both PARTIAL), no state changed on Box 1. Idle, awaiting Sky's call on resolution path (P1/P2/P3/P4 from prior surface).
