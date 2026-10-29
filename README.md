| [KR 한국어](./docs/locales/ko.md) | [US English](./README.md) | [JP 日本語](./docs/locales/ja.md) | [CN 简体中文](./docs/locales/zh-CN.md) | [ES Español](./docs/locales/es.md) |
| :---: | :---: | :---: | :---: | :---: |
| [FR Français](./docs/locales/fr.md) | [DE Deutsch](./docs/locales/de.md) | [RU Русский](./docs/locales/ru.md) | [SA العربية](./docs/locales/ar.md) | [IN हिन्दी](./docs/locales/hi.md) |

---

# Hyojun Jo (조효준)

> **Systems Software Engineer** focusing on Linux Kernel Observability, High-Throughput Packet Processing, and Low-Latency Network Infrastructure.

I design and optimize low-level software where microseconds matter. My work revolves around eBPF kernel instrumentation, XDP-driven network pipelines, memory-efficient data structures, and deterministic telemetry for distributed production clusters.

- 📍 Location: Seoul, South Korea (UTC+9)
- 💼 Primary Focus: Linux Kernel Subsystems, eBPF/XDP, Network Performance Engineering
- 💬 Ask me about: Cache locality, zero-copy socket buffers, ring buffers, eBPF verifier quirks, memory-mapped I/O
- 📬 Contact: `johyojun339@gmail.com`

---

## 🛠️ Technical Arsenal

### Languages & Core Runtimes
| Layer | Technologies & Tools |
| :--- | :--- |
| **Primary Systems** | Rust (nightly/stable), C (C11/C23), Go (1.23+), x86-64 / AArch64 Assembly |
| **Kernel & Tracing** | Linux eBPF (`libbpf`, `aya-rs`), XDP, `kprobes`/`uprobes`, `tracepoints`, `perf_events` |
| **Scripting & Automation** | POSIX Shell / Bash, Python 3, PowerShell Core, AWK / Sed |

### Operating Systems & Infrastructure
| Domain | Stack & Environment |
| :--- | :--- |
| **Kernel Subsystems** | Linux VFS, Netfilter / `nftables`, Socket Buffers (`sk_buff`), cgroups v2, tc-bpf |
| **Observability & Metrics** | OpenTelemetry, Prometheus, DuckDB, ClickHouse, Grafana, Vector |
| **Tooling & Profiling** | `bpftool`, `perf`, `gdb`, `valgrind`, `strace`, `flamegraph`, LLVM / Clang 18+ |
| **Networking Protocols** | TCP/IP, UDP, QUIC / HTTP3, WireGuard, BGP / EVPN fundamentals |

---

## 🚀 Active Open Source Projects

```
+--------------------------------------------------------------------------+
|  flow-kprobe  [Rust / C / eBPF]                                          |
|  Zero-overhead Linux kernel probe engine for TCP socket life-cycle       |
|  tracking and microsecond packet latency attribution.                    |
+--------------------------------------------------------------------------+
|  wire-sieve   [C / XDP]                                                  |
|  High-throughput L4 stateless packet dispatcher running entirely at the |
|  NIC driver level, capable of handling 14.8M pps per 10GbE core.         |
+--------------------------------------------------------------------------+
|  nanotrace    [Go / Rust / libbpf]                                       |
|  Lightweight CLI utility to inspect tail latencies across Linux system   |
|  calls with sub-microsecond histogram visualizers in the terminal.       |
+--------------------------------------------------------------------------+
```

### 1. `flow-kprobe`
- **Core Architecture**: Hooks into `tcp_v4_connect`, `tcp_set_state`, and `tcp_close` via modern CO-RE (`Compile Once - Run Everywhere`) eBPF programs.
- **Key Feature**: Streams low-overhead ring buffer events to user-space without context-switching churn.
- **Benchmark**: Under a 100,000 req/sec benchmark load, CPU utilization stays below `0.8%` on a standard 8-core host.

### 2. `wire-sieve`
- **Core Architecture**: Bypasses the Linux network stack using XDP native mode (`XDP_TX` / `XDP_PASS`) to drop malformed traffic and re-route validated packets directly at the network interface layer.
- **Design Philosophy**: Strict mechanical sympathy, zero-allocation memory pools, cache-line-aligned packet metadata structures.

### 3. `nanotrace`
- **Core Architecture**: Attaches to `sys_enter` and `sys_exit` tracepoints to measure exact system call duration across storage and network threads.
- **Output**: Terminal-based ASCII percentile ladders (`p50`, `p95`, `p99`, `p99.9`) rendered without external web dependencies.

---

## 📊 Engineering Principles & Code Aesthetics

1. **Mechanical Sympathy Above All**: Software runs on hardware. Cache-line alignment, branch predictor ergonomics, and NUMA node locality take precedence over theoretical abstractions.
2. **Explicit Over Implicit**: Zero-magic codebases. Explicit error returns, strict ownership semantics, and minimal runtime indirection.
3. **Observability is Not an Afterthought**: If a system cannot be inspected under load with zero performance penalty, it is incomplete.
4. **Data-Oriented Design**: Optimize for contiguous memory arrays and vectorized operations over deep object hierarchies.

---

## 📈 System Benchmark Snapshot

| Metric | Baseline (Stock Linux TCP) | Tuned eBPF / XDP Bypass | Improvement |
| :--- | :--- | :--- | :--- |
| **L4 Packet Filtering** | 1.82 Mpps (Single Core) | 14.45 Mpps (Single Core) | **+693.9%** |
| **TCP State Tracing Overhead** | 4.2% Host CPU | 0.4% Host CPU | **-90.4%** |
| **System Call Tail Latency (p99)** | 142.6 microseconds | 18.2 microseconds | **-87.2%** |
| **Telemetry Buffer Memory Footprint** | 64 MB Heap Buffers | 4 MB Ring Buffer | **-93.7%** |

---

## 📬 Get In Touch

- **GitHub Issues & Discussions**: Feel free to open an issue or start a thread in any of my public repositories.
- **Email**: `johyojun339@gmail.com`
- **GPG Key**: `4A8F B120 9C7E 38A1` (Signed commits only)
- **Time Zone**: KST (UTC+9), usually active during daytime and late-night coding sessions.
