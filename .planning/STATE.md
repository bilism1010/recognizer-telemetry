# STATE.md: Recognizer Telemetry

## Current Phase
- **Phase 1: Architecture & Technical Decision Record (Research & Specification)**

## Status
- **Onboarding completed**: OpenGSD planning initialized (`PROJECT.md`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md`).
- **Phase 1 Spec Defined**: Created `.planning/PHASE-1-SPEC.md` reflecting architecture decisions:
  - Serialization: Cap'n Proto / FlatBuffers zero-copy framing.
  - Visualization: WebGL 2.0 instanced rendering (locked 60 FPS).
  - Target: Dual-engine (Synthetic generator on macOS, eBPF on Linux).
- **Next Step**: Define serialization schema and build Phase 2 ingestion engine.
