| [KR 한국어](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/ko.md) | [US English](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/README.md) | [JP 日本語](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/ja.md) | [CN 简体中文](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/zh-CN.md) | [ES Español](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/es.md) |
| :---: | :---: | :---: | :---: | :---: |
| [FR Français](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/fr.md) | [DE Deutsch](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/de.md) | [RU Русский](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/ru.md) | [BR Português](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/pt-BR.md) | [SA العربية](https://github.com/johyojun339-lgtm/johyojun339-lgtm/blob/main/docs/locales/ar.md) |

---

# Hyojun Jo (조효준)

> **Systems Software Engineer** · Kernel Infrastructure, Custom CI/CD Runners & Bare-Metal Performance Engineering.

GitLab workspace dedicated to bare-metal kernel validation, continuous benchmarking pipelines, and self-hosted CI/CD pipelines and bare-metal environments for Linux eBPF/XDP network stacks.

- 📍 Location: Seoul, South Korea (UTC+9)
- 💼 Core Focus: Linux Kernel Subsystems, Bare-Metal CI Infrastructure, Hardware Performance Counters
- 🔗 Primary Hub: [GitHub @johyojun339-lgtm](https://github.com/johyojun339-lgtm)
- 📬 Contact: `johyojun339@gmail.com`

---

## 🛠️ Infrastructure & Tooling Matrix

| Domain | Systems & Tooling Stack |
| :--- | :--- |
| **Kernel & Compilers** | Linux 6.x LTS / Bleeding-Edge, LLVM/Clang 18+, GCC 14, `bpftool`, `pahole` (BTF generation) |
| **CI/CD & Virtualization** | GitLab CI Multi-Stage Pipelines, Custom Bare-Metal KVM Runners, QEMU/microVM, Docker Rootless |
| **Telemetry & Metrics** | OpenTelemetry, Prometheus Node Exporter, ClickHouse Time-Series, Grafana |
| **Performance Profiling** | Linux `perf`, `flamegraph`, `turbostat`, Hardware Performance Monitoring Units (PMUs) |

---

## 🚀 Active GitLab Pipelines & Repositories

```
+--------------------------------------------------------------------------+
|  ebpf-kernel-matrix-ci                                                   |
|  Multi-kernel continuous testing pipeline verifying eBPF CO-RE bytecode  |
|  compatibility across 14 distinct Linux kernel versions (5.4 to 6.12+).  |
+--------------------------------------------------------------------------+
|  xdp-bench-harness                                                       |
|  Dedicated bare-metal runner harness evaluating packet dispatch rate and |
|  CPU cache-miss ratios under line-rate 10GbE traffic generation.         |
+--------------------------------------------------------------------------+
```

### 1. `ebpf-kernel-matrix-ci`
- **Purpose**: Spawns isolated microVMs via QEMU on every git push to validate eBPF verifier acceptance across multiple architecture and kernel versions.
- **Artifacts**: Produces deterministic BTF dump files and compiler diagnostic logs.

### 2. `xdp-bench-harness`
- **Purpose**: Executes scheduled bare-metal benchmarks against physical NIC drivers to catch tail-latency regressions before release tags.

---

## 📊 Verification & Delivery Philosophy

1. **Bare-Metal Over Emulation**: Software dealing with memory latency and CPU cache hierarchy must be verified on dedicated physical hardware.
2. **Reproducible Bytecode**: Build scripts pin compiler toolchains and kernel headers down to the exact commit SHA.
3. **No Phantom Metrics**: Synthetic benchmarks must always correlate with hardware PMU counters (L1/LLC cache misses, branch mispredictions, cycle stalls).

---

## 📬 Contact & Mirror Sync

- **Mirror Status**: Public source repositories are synchronized directly with [GitHub](https://github.com/johyojun339-lgtm).
- **Email**: `johyojun339@gmail.com`
- **GPG Key**: `4A8F B120 9C7E 38A1` (Signed commits enforced across all branches)
