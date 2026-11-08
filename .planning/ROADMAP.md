# ROADMAP.md: Recognizer Telemetry

## Milestones & Phases

- [ ] **Phase 1: Architecture & Technical Decision Record (Research & Specification)**
  - Establish protocol specifications for binary metric framing.
  - Benchmark serialization candidates (`simd-json`, Cap'n Proto, FlatBuffers, Apache Arrow).
  - Clarify kernel probe integration strategy (eBPF on Linux vs sysinfo cross-platform fallback).
  - Align on client rendering target (WebGL 2.0 vs WebGPU vs Three.js instancing).

- [ ] **Phase 2: High-Performance Ingestion Engine & Ring Buffer Core**
  - Implement zero-allocation parser and concurrent ring buffer ingestion pipelines.
  - Implement lock-free circular window aggregation (percentile calculations via HdrHistogram/t-digest).
  - Benchmark 100k events/sec throughput with Criterion benchmarks.

- [ ] **Phase 3: Binary Streaming Gateway (WebTransport & WebSocket)**
  - Delta-of-delta encoder and frame compression (zstd / Gorilla).
  - WebSocket / WebTransport broadcast server with adaptive backpressure.

- [ ] **Phase 4: WebGL / WebGPU 60 FPS Visualizer**
  - Scaffold frontend client with zero-GC render loops.
  - Instanced particle systems, time-series cascades, and heatmap grids.
  - Direct Float32Array to VBO updates.

- [ ] **Phase 5: End-to-End Integration, eBPF & Stress Testing**
  - End-to-end stress test under 100k+ events/sec.
  - Linux eBPF probes for runqueue and socket telemetry.
