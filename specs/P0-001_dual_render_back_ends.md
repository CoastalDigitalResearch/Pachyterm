# PRD P0-001 – Dual Render Back-Ends (Pure Vulkan + ANSI Fallback)

## Purpose
Deliver *best-of-both* worlds: < 3 ms frame-time on GPUs **and** full usability over SSH/TTY without consuming GPU cycles.

## Personas
* **Laptop Developer** – RTX 4060 / Wayland; needs 144 Hz smoothness.  
* **Remote SRE** – nested tmux over 4 G link or single-user-mode console.

## Functional Scope
**Must**
1. Auto-detect backend; runtime override `--no-gpu`.
2. Maintain identical glyph shaping, colors, input-latency semantics.
3. Image handling: Vulkan → DMA-Buf textures; ANSI → Sixel / iTerm inline PNG.

**Out (v1.0)**
* macOS Metal and Windows DX12 back-ends (see PRD P2-011).

## Success Metrics
| Metric | Target |
|--------|--------|
| 1 M-line scroll (GPU) | ≥ 450 FPS · p95 frame < 3 ms |
| 1 M-line scroll (ANSI) | ≥ 60 FPS · CPU ≤ 200 % of one core |
| Renderer RSS | ≤ 40 MB |

## Dependencies
* `ash`, `vk-mem` (Vulkan)
* `ratatui` (ANSI path)
* Shared `TerminalRenderer` trait

## Risks & Mitigations
* **Code Divergence** – Keep one trait & golden reference tests across paths.
* **Driver Bugs** – Maintain deny-list for broken Mesa / NVIDIA versions.

## Acceptance Test
1. Run `bench_scroll --lines 1000000` locally and over SSH.  
2. Verify FPS and RSS meet targets.  
3. Swap backend with `--no-gpu`; screenshot hashes identical.
