---

### `P0-004_shared_memory_buffers_api.md`
```markdown
# PRD P0-004 – Shared Memory Buffers API

## Problem
Serializing large scrollback & file diffs for LLM agents kills latency.

## Solution
Memory-mapped ring buffers read-only to the container, zero-copy.

### Layout
[Header][Cmd Ring][File Slice Ring][Agent Scratch]

* 4 KiB-aligned; checksum dedup.

## Functional Requirements
* Store ≥ 10 K commands, ≥ 2 K file lines.
* Fallback to `memfd_create` when `/dev/shm` unavailable.

## Success Metrics
| Metric | Target |
|--------|--------|
| Buffers latency | < 1 µs |
| Extra RSS per pane | ≤ 256 KB |

## Dependencies
* `memmap2`
* `crossbeam`
