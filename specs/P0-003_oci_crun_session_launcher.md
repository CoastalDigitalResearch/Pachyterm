# PRD P0-003 – OCI Session Launcher (crun) with vLLM Runtime

## Objective
Isolate each terminal pane in a rootless OCI container embedding a **Ramalama-wrapped vLLM** process, preserving host cleanliness and enabling on-the-fly model swaps.

## Functional Requirements
1. Cold-start container ≤ 40 ms (rootless, seccomp).
2. Load vLLM-7B model ≤ 12 s; hot-start subsequent sessions ≤ 3 s.
3. CLI examples  
   ```bash
   pterm new --model mistral-7b-instruct --gpu
   pterm new --no-gpu --model codellama-7b

	4.	Overlayfs layer sharing for model weights.
	5.	Logs & crash dumps stored under ~/.pachyterm/sessions/<id>.

Security
	•	Default seccomp profile; no CAP_SYS_ADMIN.
	•	Image signatures verified with Cosign.

Success Metrics

Metric
Target
Session cold-start
≤ 40 ms
First token after load
≤ 400 ms
Platforms
Ubuntu 22, Fedora 40, WSL 2 – all pass


Dependencies
	•	crun ≥ 1.14
	•	ramalama base image
	•	vllm ≥ 0.4
	•	Kernel cgroup-v2 + overlayfs

Risks & Mitigations
	•	VRAM double-load – addressed by PRD P1-009 pooling.
	•	Namespace leakage – audit via bats-core integration tests.
