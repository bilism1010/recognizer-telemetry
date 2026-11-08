# Recognizer Telemetry

> Real-time system telemetry streamer capable of parsing 100k+ events/sec with high-performance WebGL visualization.

Named after the massive, all-seeing Recognizers scanning the Grid for rogue programs and system anomalies.

---

## 1. Goal

Provide an ultra-low overhead, high-throughput observability pipeline that collects host/kernel/container metrics, kernel eBPF probes, and application event streams, parses and aggregates over 100,000 events/second per core with zero GC pauses, and streams compressed frames over WebSockets/WebTransport into an interactive 60fps WebGL/Three.js dashboard.

---

## 2. Specification

### 2.1 Core Ingestion & Processing Engine (Systems Layer)
- **High-Throughput Ingestion**:
  - Zero-copy buffer ingestion using SIMD-accelerated deserialization (e.g. `simd-json`, Cap'n Proto, or Apache Arrow).
  - Ring-buffer based aggregation queues to prevent thread contention.
  - Optional Linux eBPF probe collection (CPU scheduling runqueues, network socket telemetry, disk I/O latency distributions).
- **Time-Series Windowing & Downsampling**:
  - Lock-free circular rolling windows with histogram and percentiles calculation (HdrHistogram / t-digest).
  - Delta-of-delta encoding and Gorillas/zstd compression for streaming frames over network boundaries.

### 2.2 Streaming & Visualization Layer (Web / WebGL)
- **WebTransport / WebSocket Server**:
  - Broadcast server streaming binary packed metrics frames directly to browser clients with adaptive backpressure.
- **Client WebGL Rendering**:
  - GPU-accelerated particle systems and instanced meshes rendering real-time heatmaps, topology graphs, and timeline cascades at a locked 60+ FPS.
  - Zero DOM allocations during the render loop (Float32Array updates pushed straight into GPU VBOs).

---

## 3. Recommended Prompt for Next Refinement Phase

```text
Act as a Principal Observability & Graphics Systems Engineer. We are designing 'Recognizer Telemetry', a real-time systems telemetry streamer parsing 100k+ events/sec and displaying them via WebGL.
Please provide:
1. Systems architecture diagram: Ingestion pipeline -> SIMD parsing -> Lock-free rolling window buffer -> WebTransport stream -> WebGL instanced rendering.
2. Comparative analysis of serialization formats (simd-json vs Cap'n Proto vs FlatBuffers vs Arrow Flight) for streaming 100k events/sec.
3. WebGL/WebGPU instancing and VBO buffer swapping strategy to maintain 60 FPS without garbage collection pauses.
4. Minimal proof-of-concept design for the binary streaming protocol.
```

---

## 4. Scaffolding & Getting Started

```bash
# Verify Rust toolchain
cargo check

# Run tests
cargo test

# Start streaming daemon
cargo run -- --port 8080 --metrics-rate 100000
```
