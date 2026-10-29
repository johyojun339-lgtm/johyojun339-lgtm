# Hyojun Jo (조효준)

> **Systems Software Engineer** focusing on low-level inference runtime performance, eBPF-driven network profiling for distributed training clusters, and memory-mapped model streaming.

GitHub: [@johyojun339-lgtm](https://github.com/johyojun339-lgtm) · Email: `johyojun339@gmail.com` · Location: Seoul, South Korea

---

## 🔬 Research & Open-Source Focus on Hugging Face

While my primary software development lives at the Linux kernel and network layer, I utilize Hugging Face to share benchmarks, datasets, and lightweight tooling at the boundary between hardware systems and machine learning workloads:

### 1. Distributed Training Network Telemetry Datasets
- **`nccl-trace-packet-dumps`**: High-resolution packet traces and socket buffer queue telemetry captured via eBPF during multi-node GPU all-reduce / all-gather synchronization phases.
- **`roce-infiniband-tail-latencies`**: Microsecond-scale distribution profiles highlighting transient network congestion, PFC (Priority Flow Control) deadlocks, and tail-latency bottlenecks in RDMA networks.

### 2. Low-Latency Edge Runtimes & Tokenizer Optimization
- **`fast-bpe-simd`**: Experimental Rust-based SIMD/AVX-512 tokenization experiments aimed at minimizing memory bandwidth overhead in pre-inference pipelines.
- **`mmap-safetensors-benchmarks`**: Linux VFS page-cache utilization profiles and memory-mapped model weight streaming metrics across NVMe PCIe Gen4/Gen5 storage arrays.

---

## 🛠️ Stack & Tooling

| Focus Area | Stack & Environment |
| :--- | :--- |
| **Runtimes & Acceleration** | Rust, C, Linux eBPF, AVX-512 / NEON intrinsics, mmap / Direct I/O |
| **Distributed Profiling** | OpenTelemetry, eBPF `kprobes`/`tracepoints`, RoCE / InfiniBand telemetry |
| **Data & Benchmarks** | DuckDB, ClickHouse, Arrow / Parquet, Linux `perf` |

---

## 📬 Collaboration

Interested in zero-overhead kernel profiling for large-scale GPU clusters, memory-mapped model serving bottlenecks, or hardware performance optimization? Feel free to connect via GitHub discussions or email.
