# Pachyterm PRD Priority Mapping

## Overview
This document establishes a unified priority ordering for all Pachyterm PRDs, integrating both the original P0-P1-P2 numbered specifications and the detailed component PRDs organized in subdirectories.

## Priority Classification

### P0 - Core Foundation (Weeks 1-6)
**Must-have components for basic terminal functionality**

| Priority | Component | Location | Dependencies |
|----------|-----------|----------|-------------|
| P0-001 | Dual Render Back-Ends | `P0-001_dual_render_back_ends.md` | TTY Engine |
| P0-002 | tmux-Compatible Multiplexer | `P0-002_tmux_compatible_multiplexer.md` | TTY Engine, Renderer, Input/Keymap |
| P0-003 | OCI Session Launcher | `P0-003_oci_crun_session_launcher.md` | TTY Engine, Model Host |
| P0-004 | Shared Memory Buffers API | `P0-004_shared_memory_buffers_api.md` | TTY Engine, OCI Session Launcher |
| P0-005 | OS-Awareness Agent | `P0-005_os_awareness_agent.md` | Model Host, Command Parser |
| P0-006 | Privacy & Telemetry Controls | `P0-006_privacy_and_telemetry.md` | Config System |
| P0-007 | Minimal Config & Auto-Theme | `P0-007_minimal_config_and_auto_theme.md` | Plain-Text Config |
| P0-101 | TTY Engine | `core-runtime/tty-engine-prd.md` | None (foundation) |
| P0-102 | GPU Renderer | `core-runtime/renderer-prd.md` | TTY Engine |
| P0-103 | Input/Keymap System | `core-runtime/input-keymap-prd.md` | TTY Engine, Config System |
| P0-104 | Plain-Text Config | `config-state/plain-text-config-prd.md` | None |

### P1 - Agent Integration (Weeks 7-10)
**AI functionality and advanced features**

| Priority | Component | Location | Dependencies |
|----------|-----------|----------|-------------|
| P1-008 | Image/Media Display | `P1-008_image_media_display.md` | Renderer, Shared Memory |
| P1-009 | Model Hot-Swap VRAM Arbitration | `P1-009_model_hot_swap_vram_arbitration.md` | Model Host, OCI Launcher |
| P1-010 | Extensible Agent API | `P1-010_extensible_agent_api.md` | Model Host, Plugin SDK |
| P1-201 | Command Prefix Parser | `agent-subsystem/command-parser-prd.md` | Input/Keymap, Config |
| P1-202 | Model Host | `agent-subsystem/model-host-prd.md` | Command Parser, Config, Profile Cache |
| P1-203 | Streaming UI | `agent-subsystem/streaming-ui-prd.md` | Model Host, Renderer |
| P1-204 | Profile Cache | `config-state/profile-cache-prd.md` | Plain-Text Config, Model Host |

### P2 - Distribution & Polish (Weeks 11-14)
**Distribution, optimization, and platform-specific features**

| Priority | Component | Location | Dependencies |
|----------|-----------|----------|-------------|
| P2-011 | macOS Metal Backend | `P2-011_macos_metal_backend.md` | Renderer |
| P2-012 | Enhanced Font/Ligature System | `P2-012_enhanced_font_ligature_system.md` | Renderer |
| P2-301 | Plugin SDK | `extensibility/plugin-sdk-prd.md` | Core Runtime, Config |
| P2-302 | Installer | `distribution-operations/installer-prd.md` | Build System |
| P2-303 | Security | `distribution-operations/security-prd.md` | All Components (cross-cutting) |
| P2-304 | Telemetry | `distribution-operations/telemetry-prd.md` | Config System, Privacy Controls |

## Implementation Dependencies

### Critical Path
The following represents the critical dependency path that determines minimum implementation time:

```
TTY Engine (P0-101) → 
GPU Renderer (P0-102) → 
Input/Keymap (P0-103) → 
Command Parser (P1-201) → 
Model Host (P1-202) → 
Streaming UI (P1-203)
```

### Parallel Development Tracks

**Track 1: Core Terminal**
- P0-101: TTY Engine
- P0-102: GPU Renderer  
- P0-001: Dual Render Back-Ends
- P0-002: tmux-Compatible Multiplexer

**Track 2: Configuration**
- P0-104: Plain-Text Config
- P0-007: Minimal Config & Auto-Theme
- P1-204: Profile Cache
- P0-006: Privacy & Telemetry

**Track 3: AI Integration**
- P1-201: Command Prefix Parser
- P1-202: Model Host
- P1-203: Streaming UI
- P0-005: OS-Awareness Agent

**Track 4: Containerization**
- P0-003: OCI Session Launcher
- P0-004: Shared Memory Buffers
- P1-009: VRAM Arbitration

## Priority Rationale

### P0 Components
- **Essential for MVP**: These components are required for basic terminal functionality
- **User-visible impact**: Directly affects core user experience
- **Dependency foundation**: Other components build upon these

### P1 Components  
- **Differentiating features**: What makes Pachyterm unique (AI integration)
- **Power user functionality**: Advanced features for target audience
- **Performance critical**: Components that affect responsiveness

### P2 Components
- **Platform optimization**: Platform-specific enhancements
- **Distribution needs**: Required for production deployment
- **Ecosystem features**: Enable third-party development

## Milestone Alignment

### Week 2: MVP Foundation
- P0-101: TTY Engine (basic functionality)
- P0-104: Plain-Text Config (basic)
- P0-103: Input/Keymap (essential keys only)

### Week 6: Alpha Release
- All P0 components functional
- P0-001: Dual backends working
- P0-002: Basic multiplexing
- P1-201: Command parser (p prefix detection)

### Week 10: Beta Release  
- All P1 components integrated
- P1-202: Local model hosting
- P1-203: AI streaming responses
- P0-003: Container isolation

### Week 14: Production Release
- All P2 components complete
- P2-302: Installers for all platforms
- P2-303: Security hardening
- P2-304: Telemetry infrastructure

## Update Protocol

When priorities change:
1. Update this mapping document
2. Adjust milestone timelines accordingly  
3. Communicate changes to all development tracks
4. Update individual PRDs if dependencies change

This priority system ensures systematic development while allowing parallel work streams.