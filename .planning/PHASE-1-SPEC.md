# Phase 1: Deep Research & Architecture Decision Record (ADR)

## 1. Selected Technical Architecture

Based on the initial alignment:
- **Serialization & Ingestion**: **Cap'n Proto / FlatBuffers**
  - **Rationale**: True zero-copy memory-mapped structures. Reading a field is just pointer arithmetic with no decoding allocations, satisfying the >= 100k events/sec per core ceiling.
- **Client Rendering**: **WebGL 2.0 with Instanced Meshes / Three.js**
  - **Rationale**: Universal hardware acceleration across desktop/mobile browsers. By keeping persistent `InstancedBufferAttribute` backing arrays (`Float32Array`) and updating via `gl.bufferSubData`, we eliminate GC pauses during the 60 FPS animation loop.
- **Telemetry Sources & Platform Strategy**: **Dual Engine (Synthetic Generator + Linux eBPF feature flag)**
  - **macOS Dev & CI**: High-rate synthetic particle/event generator simulating system load + process metrics.
  - **Linux Target**: Native eBPF probes via `aya` / `libbpf-rs` to capture scheduler runqueues and socket events.

---

## 2. Ingestion & Rendering Pipeline Blueprint

```mermaid
flowchart LR
    subgraph Host / Kernel
        eBPF[Linux eBPF Probes] -.-> RingBuf[Lock-free Ring Buffer]
        Synth[High-Rate Synthetic Generator] --> RingBuf
    end

    subgraph Rust Systems Core
        RingBuf --> Parser[Zero-Copy Deserializer]
        Parser --> Windows[Circular Rolling Windows & HdrHistogram]
        Windows --> Pack[Gorilla / zstd Delta Compression]
    end

    subgraph Transport Gateway
        Pack --> WS[WebSocket / WebTransport Streamer]
    end

    subgraph Browser Client (60 FPS)
        WS --> ArrayBuffer[Binary ArrayBuffer Stream]
        ArrayBuffer --> FloatBuf[Float32Array VBO Buffer]
        FloatBuf --> WebGL[WebGL 2.0 Instanced Meshes / Shaders]
    end
```

---

## 3. High-Level Milestones Breakdown

1. **Phase 1: Architecture & Technical Decision Record** *(Current)*
   - Finalize Cap'n Proto / FlatBuffer schema definitions.
   - Establish binary frame protocol over WebSocket.
2. **Phase 2: Ingestion Engine & Ring Buffer Core**
   - High-throughput synthetic generator emitting 100k+ events/s.
   - Crossbeam ring-buffer and rolling HdrHistogram aggregation.
   - Criterion micro-benchmarks.
3. **Phase 3: WebSocket Streaming Server & Protocol Encoding**
   - Binary frame packaging with delta compression.
   - Axum / Tokio-tungstenite binary streaming gateway.
4. **Phase 4: WebGL 2.0 Real-time Visualization Client**
   - WebGL / Three.js instanced rendering dashboard with zero-allocation render loop.
   - Particle cascades and latency heatmap matrix.
5. **Phase 5: Linux eBPF Probe Integration & Load Testing**
   - Optional `aya` eBPF loader for Linux runqueues.
   - End-to-end sustained 100k events/sec validation.
