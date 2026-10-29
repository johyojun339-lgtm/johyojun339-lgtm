| [KR 한국어](./ko.md) | [US English](../../README.md) | [JP 日本語](./ja.md) | [CN 简体中文](./zh-CN.md) | [ES Español](./es.md) |
| :---: | :---: | :---: | :---: | :---: |
| [FR Français](./fr.md) | [DE Deutsch](./de.md) | [RU Русский](./ru.md) | [SA العربية](./ar.md) | [IN हिन्दी](./hi.md) |

---

# Hyojun Jo (조효준)

> **Systems Software Engineer** mit Schwerpunkt auf Linux-Kernel-Observability, Hochdurchsatz-Paketverarbeitung und extrem latenzarme Netzwerkinfrastruktur.

Ich entwerfe und optimiere systemnahe Software, bei der Mikrosekunden (μs) zählen. Meine Schwerpunkte liegen in der Kernel-Instrumentierung mittels Linux eBPF, XDP-beschleunigten Netzwerk-Pipelines, speichereffizienten Datenstrukturen und deterministischer Telemetrie für verteilte Produktionscluster.

- 📍 Standort: Seoul, Südkorea (UTC+9)
- 💼 Fokusbereiche: Linux-Kernel-Subsysteme, eBPF/XDP, Netzwerk-Performance-Engineering
- 💬 Fragen Sie mich zu: Cache-Lokalität, Zero-Copy-Socket-Puffer, Ring-Puffer-Architekturen, eBPF-Verifier-Verhalten, speicherabgebildete E/A (mmap)
- 📬 Kontakt: `johyojun339@gmail.com`

---

## 🛠️ Technisches Arsenal (Technical Arsenal)

### Programmiersprachen und Kern-Laufzeitumgebungen
| Schicht | Technologien und Werkzeuge |
| :--- | :--- |
| **Primäre Systemsprachen** | Rust (nightly/stable), C (C11/C23), Go (1.23+), x86-64 / AArch64 Assembler |
| **Kernel und Tracing** | Linux eBPF (`libbpf`, `aya-rs`), XDP, `kprobes`/`uprobes`, `tracepoints`, `perf_events` |
| **Skripte und System-Tools** | POSIX Shell / Bash, Python 3, PowerShell Core, AWK / Sed |

### Betriebssysteme und Infrastruktur
| Domäne | Stack und Umgebung |
| :--- | :--- |
| **Kernel-Subsysteme** | Linux VFS, Netfilter / `nftables`, Socket-Puffer (`sk_buff`), cgroups v2, tc-bpf |
| **Observability und Metriken** | OpenTelemetry, Prometheus, DuckDB, ClickHouse, Grafana, Vector |
| **Profiling und Werkzeuge** | `bpftool`, `perf`, `gdb`, `valgrind`, `strace`, `flamegraph`, LLVM / Clang 18+ |
| **Netzwerkprotokolle** | TCP/IP, UDP, QUIC / HTTP3, WireGuard, BGP / EVPN Grundlagen |

---

## 🚀 Aktive Open-Source-Projekte

```
+--------------------------------------------------------------------------+
|  flow-kprobe  [Rust / C / eBPF]                                          |
|  Zero-Overhead Linux-Kernel-Probe-Engine zur Verfolgung von TCP-Socket-   |
|  Lebenszyklen und mikrosekundengenauen Paketlatenz-Zuordnung.            |
+--------------------------------------------------------------------------+
|  wire-sieve   [C / XDP]                                                  |
|  Hochdurchsatz-L4-Stateless-Paket-Dispatcher auf Netzwerk-Treiber-Ebene  |
|  mit bis zu 14,8 Millionen Paketen/s (14.8M pps) pro 10GbE-Kern.         |
+--------------------------------------------------------------------------+
|  nanotrace    [Go / Rust / libbpf]                                       |
|  Leichtgewichtiges CLI-Tool zur Analyse von Tail-Latenzen bei Linux-     |
|  Systemaufrufen mit Sub-Mikrosekunden-Histogrammen im Terminal.          |
+--------------------------------------------------------------------------+
```

### 1. `flow-kprobe`
- **Kernarchitektur**: Hängt sich über moderne CO-RE (`Compile Once - Run Everywhere`) eBPF-Programme an zentrale Kernel-Funktionen wie `tcp_v4_connect`, `tcp_set_state` und `tcp_close`.
- **Hauptmerkmal**: Streamt Ring-Puffer-Ereignisse mit minimaler Latenz ohne kostspielige Kontextwechsel in den Userspace.
- **Benchmark**: Unter einer Testlast von 100.000 Anfragen/Sekunde bleibt die CPU-Auslastung auf einem standardmäßigen 8-Kern-Host unter `0,8%`.

### 2. `wire-sieve`
- **Kernarchitektur**: Umgeht den regulären Linux-Netzwerk-Stack über den nativen XDP-Modus (`XDP_TX` / `XDP_PASS`), verwirft schadhaften Datenverkehr direkt an der Netzwerkkarte und leitet legitime Pakete mit Leitungsgeschwindigkeit weiter.
- **Designphilosophie**: Strikte mechanische Sympathie (Mechanical Sympathy), allokationsfreie Speicherpools und an Cache-Lines ausgerichtete Paket-Metadaten.

### 3. `nanotrace`
- **Kernarchitektur**: Klinkt sich in die Tracepoints `sys_enter` und `sys_exit` ein, um die exakte Dauer von Systemaufrufen für Speicher- und Netzwerk-Threads mikrosekundengenau zu messen.
- **Ausgabe**: Erzeugt ASCII-Perzentil-Diagramme (`p50`, `p95`, `p99`, `p99.9`) direkt im Terminal ohne externe Web-Abhängigkeiten.

---

## 📊 Engineering-Prinzipien und Code-Philosophie

1. **Mechanische Sympathie an erster Stelle**: Software läuft auf physischer Hardware. Cache-Line-Ausrichtung, Sprungvorhersage-Ergonomie und NUMA-Knoten-Lokalität haben Vorrang vor theoretischen Abstraktionen.
2. **Explizit vor Implizit**: Verzicht auf magischen Code. Klare Fehlerrückgaben, strikte Besitzsemantiken und minimale Laufzeit-Indirektionen.
3. **Observability ist kein nachträglicher Gedanke**: Ein System, das im Produktionsbetrieb unter Last nicht ohne Performance-Einbußen durchleuchtet werden kann, ist unvollständig.
4. **Datenorientiertes Design (Data-Oriented Design)**: Optimierung auf zusammenhängende Speicherarrays und vektorisierte Operationen statt tiefer Objekthierarchien.

---

## 📈 System-Benchmark-Übersicht

| Metrik | Standard Linux-TCP-Stack | Optimierte eBPF / XDP Pipeline | Leistungssteigerung |
| :--- | :--- | :--- | :--- |
| **L4-Paketfilterungs-Durchsatz** | 1,82 Mpps (Einzellkern) | 14,45 Mpps (Einzelkern) | **+693,9%** |
| **Overhead beim TCP-Status-Tracing** | 4,2% Host-CPU | 0,4% Host-CPU | **-90,4%** |
| **Syscall-Tail-Latenz (p99)** | 142,6 Mikrosekunden | 18,2 Mikrosekunden | **-87,2%** |
| **Telemetrie-Puffer-Speicherbedarf** | 64 MB Heap-Puffer | 4 MB Ring-Puffer | **-93,7%** |

---

## 📬 Kontakt und Austausch

- **GitHub Issues & Discussions**: Eröffnen Sie gerne ein Issue oder starten Sie eine Diskussion in meinen öffentlichen Repositorien.
- **E-Mail**: `johyojun339@gmail.com`
- **GPG-Schlüssel**: `4A8F B120 9C7E 38A1` (Ausschließlich kryptografisch signierte Commits)
- **Zeitzone**: KST (UTC+9), tagsüber sowie in konzentrierten nächtlichen Programmiersitzungen aktiv.

