# PROJECT.md: Recognizer Telemetry

## Project Overview
**Recognizer Telemetry** is an ultra-low-overhead, high-throughput systems observability streamer engineered in Rust. It ingests host, container, and kernel-level metrics/events at **100,000+ events/sec per core** with zero GC pauses, downsamples them in lock-free ring-buffers, and streams compressed binary frames over WebSockets / WebTransport into an interactive, locked-60fps WebGL/WebGPU visualization dashboard.

## Core Value Proposition
- **Extreme Throughput Systems Pipeline**: Zero-copy SIMD parsing, lock-free aggregation queues, eBPF probes.
- **Microsecond Latency Windows**: Circular rolling histogram buffers (HdrHistogram / t-digest) with Gorilla/zstd delta compression.
- **GPU-Accelerated 60 FPS Visuals**: WebGL/WebGPU instanced meshes and particle clouds rendering topology graphs and time-series cascades directly via Float32Array VBO buffer swapping with zero DOM/GC overhead.

## Architecture Stack
- **Backend / Core**: Rust, Tokio, Crossbeam lock-free queues, SIMD deserialization (`simd-json` / Cap'n Proto / FlatBuffers), eBPF (`aya` / `libbpf-rs`).
- **Transport**: WebTransport / WebSocket binary streaming server.
- **Frontend / Client**: TypeScript, WebGL 2.0 / WebGPU (Three.js / raw shaders), instanced geometry rendering.
