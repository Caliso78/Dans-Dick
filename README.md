SURFACE — WO-F02 — Phase 0 recon results
Finding 1: NO SCP transfers running on this Windows box
Verified via Win32_Process (CommandLine + Name) + Get-NetTCPConnection + ps -W:

Only one SSH/SCP-family process: ssh.exe PID 27040, started 10:41:22 AM, command "C:\Program Files\Git\usr\bin\ssh.exe" -p 36585 root@209.146.116.50 — that's an interactive shell, no scp/sftp invocation
One ESTABLISHED TCP connection to 209.146.116.50:36585 owned by PID 27040 (matches the interactive shell)
Zero processes matching: scp.exe, sftp.exe, pscp.exe, rsync.exe, huggingface-cli.exe, wsl.exe, wslhost.exe
Three python.exe processes — all are mempalace.mcp_server (Claude's memory MCP), none transferring anything
OLMo + Gemma source files mtimes: May 1 (untouched in 3+ days) — no active write activity
The WO's premise ("OLMo SCP in progress ~63% of shard 1/14, gemma SCP stalled at 0%") doesn't match local state. Either the transfers crashed silently, were running from a different machine, or were never running on this box. Sky's interactive SSH session (PID 27040) is connected but not transferring.

Finding 2: 24 GB 3090 cannot serve either model at BF16
Both config.json files declare "dtype": "bfloat16":

OLMo-3-32B-Think: 64 hidden layers × hidden_size 5120, ~32B params at BF16 ≈ 64 GB weight memory, plus KV cache + activations. Won't fit on 24 GB.
gemma-4-31B: 60 hidden layers × hidden_size 5376, ~31B params at BF16 ≈ 62 GB weight memory, same problem.
vLLM on 24 GB will OOM on vllm serve <hf_path> for either. Real-world options:

(i) Pre-quantize to AWQ/GPTQ INT4 (~16 GB each) — adds a quantization step outside WO scope
(ii) --cpu-offload-gb on vLLM — extremely slow (multi-second per token), 5-prompt smoke could take hours per model
(iii) Use a different/larger Box 1 (e.g. Vast.ai 4090 48GB or 2× 3090 with TP=2)
(iv) Drop these two from foundation reconsideration and pull the Q4 GGUF variants instead (would need llama.cpp not vLLM, different runner)
Finding 3: Gemma-4-31B is multimodal
Config declares Gemma4ForConditionalGeneration with vision_config (gemma4_vision) and audio_config (audio_token_id 258881). Per WO-F01 §0.5 from prior smoke results, Sky already specified "Gemma-4 multimodal: text-only via Gemma3 text-mode loader." vLLM serving this architecture text-only requires specific flags or a model-class override. May fail on default vllm serve.

Verified HF model IDs (from README.md)
OLMo: allenai/Olmo-3-32B-Think (per README line: AutoModelForCausalLM.from_pretrained("allenai/Olmo-3-32B-Think"))
Gemma: google/gemma-4-31B-it (per README: MODEL_ID = "google/gemma-4-31B-it") — note the -it instruction-tuned suffix
Local source state (good news)
OLMo: 14/14 shards present, total 61 GB, all from May 1 (HF download — .cache/huggingface/download/ has metadata files for all 14 shards)
Gemma: 2/2 shards present, total 59 GB, all from May 1 (same HF download pattern)
Both source corpora are complete locally; no re-pull needed on the source side
Options seen by me
(a) Sky clarifies SCP state. Where were the transfers running? A different machine? Did they complete on Box 1 already? Did they crash? I cannot proceed with "kill stalled gemma SCP" if there's no SCP to kill on this box.
(b) Authorize SSH to Box 1 (read-only). I connect via a fresh SSH session, run ls -la /workspace/models/, df -h /workspace, nvidia-smi, pip show vllm, check ps aux | grep scp on Box 1. Tells us: (1) what's already arrived on Box 1, (2) whether the in-progress SCP Sky saw was running from elsewhere, (3) whether the VRAM/vLLM/disk premises hold. Pure read; no kills, no transfers, no model loads.
(c) Re-scope the WO. If the VRAM block is real, the smoke design needs to change — quantize first, use bigger box, or use llama.cpp + GGUF. That's a Sky decision, not mine.
(d) Different.
My recommendation
(b) immediately, then (a)+(c) based on what (b) shows. The Box 1 state is the lowest-cost question to answer and unblocks everything else. If Box 1 already has both models on disk (Sky's described SCP completed before getting cut off), Findings 1 and 3 partially dissolve — we'd skip transfers entirely and go straight to vLLM verify. If Box 1 has neither, we still need (a) for Sky to tell me where the transfers actually are, and (c) for Sky to call the VRAM tradeoff.

What I will do until Sky responds
Idle. No SSH, no scp kill, no transfer start, no edits. Phase 0 recon done; everything from here is pending Sky direction. The 8 todos in my tracker remain at "Phase 0 done, all subsequent phases pending."
