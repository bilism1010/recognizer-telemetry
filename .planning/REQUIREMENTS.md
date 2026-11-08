# REQUIREMENTS.md: Recognizer Telemetry

## Functional Requirements
- **FR-1: High-Rate Event Ingestion**: Ingest synthetic and real telemetry streams at >= 100k events/sec per core.
- **FR-2: Zero-Copy Parsing**: Deserialization using SIMD instructions without intermediate heap allocation per event.
- **FR-3: Lock-Free Aggregation**: Ring-buffer / circular rolling windows producing statistical summaries (p50, p90, p99, p99.9 via HdrHistogram/t-digest).
- **FR-4: Compressed Binary Framing**: Pack metric frames into binary packets using delta-of-delta encoding and Gorilla/zstd compression.
- **FR-5: Real-Time Network Transport**: Low-latency binary push over WebTransport / WebSocket with adaptive backpressure handling.
- **FR-6: 60 FPS WebGL Dashboard**: Browser interface displaying particle cascades, heatmaps, and topology graphs without garbage collection stalls.

## Non-Functional Requirements
- **NFR-1: Latency**: End-to-end ingestion-to-render latency under 50ms.
- **NFR-2: Zero GC / Allocation Ceiling**: No dynamic heap allocations in hot paths on both Rust server and browser render loop.
- **NFR-3: Cross-Platform & Kernel Support**: Linux (eBPF probes supported), macOS (fallback synthetic generator & sysinfo metrics for development).
