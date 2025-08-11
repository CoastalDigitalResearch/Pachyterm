# PRD P0-002 – tmux-Compatible Multiplexer

## Why
Kitty owners love built-in splits. Ghostty users rely on tmux.  
Pachyterm must ship a native multiplexer **with tmux key parity** so users adopt with zero workflow friction.

## Functional Requirements
* Horizontal / vertical splits, tabs, session save / load.
* Broadcast-input across panes.
* Default keybindings: `Ctrl-b` family identical to tmux; overridable via config.

## Non-Functional
* Key-dispatch latency p95 ≤ 2 ms.
* Layout serialize / restore round-trip < 5 ms.
* Works in both GPU and `--no-gpu` modes.

## Success Metrics
* 100 % pass on open-source tmux integration test suite.
* 0 visual artifacts after 24 h soak with 100 pane create/destroy cycles.

## Technical Approach
* Rust port or fresh re-implementation of `libtmt` layout engine.
* Scene diff applied to renderer each frame – minimizes GPU work.

## Risks
* VT escape-sequence edge cases → adopt tmux conformance corpus.
* Memory blow-up with deep splits → lazy grid allocation, hard cap guard.
