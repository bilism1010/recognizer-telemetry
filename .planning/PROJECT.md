# PROJECT.md: Recognizer Telemetry

## Project Overview
**Recognizer Telemetry** is an ultra-low-overhead, high-throughput systems observability streamer engineered in Rust. It ingests host, container, and kernel-level metrics/events at **100,000+ events/sec per core** with zero GC pauses, downsamples them in lock-free ring-buffers, and streams compressed binary frames over WebSockets / WebTransport into an interactive, locked-60fps WebGL/WebGPU visualization dashboard.

## Core Value Proposition
- **Extreme Throughput Systems Pipeline**: Zero-copy SIMD parsing, lock-free aggregation queues, eBPF probes sustained at >= 100,000 events/sec per core.
- **Defeating the Observer Effect**: Collector overhead strictly bounded (< 2.5% CPU, zero GC / heap churn in hot paths) so monitoring never interferes with high-frequency workloads.
- **Microsecond Latency Windows**: Continuous sub-millisecond rolling quantile histograms (HdrHistogram / t-digest) with Gorilla/zstd delta compression, capturing transient microbursts lost in 15-second Prometheus scrapes.
- **GPU-Accelerated 60 FPS Visuals**: WebGL 2.0 / WebGPU instanced meshes and particle clouds rendering 100,000 active telemetry points directly via `Float32Array` VBO buffer swapping with zero DOM/GC overhead.

## Defense & Differentiation Against Existing Stacks
Traditional observability stacks (Datadog, Prometheus, Vector, Grafana) are fundamentally optimized for historical storage and coarse periodic reporting (5s-15s intervals). Recognizer Telemetry occupies an unaddressed niche: **real-time, zero-overhead systems interrogation**. It acts like an oscilloscope rather than an auditor, exposing sub-millisecond thread contention, scheduler delays, and network socket queue spikes live via GPU visualization before they trigger catastrophic outages.

## Architecture Stack
- **Backend / Core**: Rust, Tokio, Crossbeam lock-free queues, SIMD deserialization (`simd-json` / Cap'n Proto / FlatBuffers), eBPF (`aya` / `libbpf-rs`).
- **Transport**: WebTransport / WebSocket binary streaming server.
- **Frontend / Client**: TypeScript, WebGL 2.0 / WebGPU (Three.js / raw shaders), instanced geometry rendering.
