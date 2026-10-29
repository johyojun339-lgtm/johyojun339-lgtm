| [KR 한국어](./ko.md) | [US English](../../README.md) | [JP 日本語](./ja.md) | [CN 简体中文](./zh-CN.md) | [ES Español](./es.md) |
| :---: | :---: | :---: | :---: | :---: |
| [FR Français](./fr.md) | [DE Deutsch](./de.md) | [RU Русский](./ru.md) | [SA العربية](./ar.md) | [IN हिन्दी](./hi.md) |

---

# Hyojun Jo (조효준)

> **Ingénieur Logiciel Systèmes** — Spécialisé dans l'Observabilité du Noyau Linux, le Traitement de Paquets à Haut Débit et les Infrastructures Réseau à Très Faible Latence.

Je conçois et optimise des logiciels de bas niveau où chaque microseconde (μs) est déterminante. Mes travaux portent sur l'instrumentation du noyau Linux via eBPF, les pipelines réseau ultra-rapides propulsés par XDP, les structures de données à haute efficacité mémoire et la télémétrie déterministe pour clusters distribués en production.

- 📍 Localisation : Séoul, Corée du Sud (UTC+9)
- 💼 Domaines de prédilection : Sous-systèmes du noyau Linux, eBPF/XDP, Ingénierie de performance réseau
- 💬 Sujets d'échange : Localité de cache, tampons de socket sans copie (zero-copy), architectures en ring buffer, particularités du vérificateur eBPF, E/S projetées en mémoire (mmap)
- 📬 Contact : `johyojun339@gmail.com`

---

## 🛠️ Arsenal Technique (Technical Arsenal)

### Langages et Runtimes Systèmes
| Couche | Technologies et Outils |
| :--- | :--- |
| **Systèmes Principaux** | Rust (nightly/stable), C (C11/C23), Go (1.23+), Assembleur x86-64 / AArch64 |
| **Noyau et Traçage** | Linux eBPF (`libbpf`, `aya-rs`), XDP, `kprobes`/`uprobes`, `tracepoints`, `perf_events` |
| **Scripts et Automatisation** | POSIX Shell / Bash, Python 3, PowerShell Core, AWK / Sed |

### Systèmes d'Exploitation et Infrastructure
| Domaine | Environnement et Technologies |
| :--- | :--- |
| **Sous-systèmes du Noyau** | Linux VFS, Netfilter / `nftables`, Socket Buffers (`sk_buff`), cgroups v2, tc-bpf |
| **Observabilité et Métriques** | OpenTelemetry, Prometheus, DuckDB, ClickHouse, Grafana, Vector |
| **Profilage et Outils** | `bpftool`, `perf`, `gdb`, `valgrind`, `strace`, `flamegraph`, LLVM / Clang 18+ |
| **Protocoles Réseau** | TCP/IP, UDP, QUIC / HTTP3, WireGuard, Principes fondamentaux BGP / EVPN |

---

## 🚀 Projets Open Source Actifs

```
+--------------------------------------------------------------------------+
|  flow-kprobe  [Rust / C / eBPF]                                          |
|  Moteur de sonde noyau Linux à surcharge nulle pour le traçage du cycle |
|  de vie des sockets TCP et l'analyse de latence de paquets par microseconde|
+--------------------------------------------------------------------------+
|  wire-sieve   [C / XDP]                                                  |
|  Distributeur de paquets L4 sans état s'exécutant au niveau pilote NIC,   |
|  capable d'absorber 14,8 millions de paquets/s (14.8M pps) par cœur 10GbE.|
+--------------------------------------------------------------------------+
|  nanotrace    [Go / Rust / libbpf]                                       |
|  Utilitaire CLI léger visualisant la latence de queue (tail latency) des |
|  appels système Linux via des histogrammes infra-microseconde en terminal|
+--------------------------------------------------------------------------+
```

### 1. `flow-kprobe`
- **Architecture clé** : Se connecte aux fonctions noyaux critiques telles que `tcp_v4_connect`, `tcp_set_state` et `tcp_close` grâce aux programmes eBPF modernes CO-RE (`Compile Once - Run Everywhere`).
- **Points forts** : Transfère les événements de ring buffer vers l'espace utilisateur sans surcoût de changement de contexte.
- **Performances** : Sous une charge de test de 100 000 requêtes/seconde, l'utilisation processeur reste inférieure à `0,8%` sur une machine à 8 cœurs.

### 2. `wire-sieve`
- **Architecture clé** : Court-circuite la pile réseau Linux standard en mode natif XDP (`XDP_TX` / `XDP_PASS`) afin de rejeter les flux anormaux dès l'entrée de la carte réseau et router les paquets valides à la vitesse du câble.
- **Philosophie de conception** : Sympathie mécanique rigoureuse, pools de mémoire sans allocation dynamique et métadonnées de paquets alignées sur les lignes de cache.

### 3. `nanotrace`
- **Architecture clé** : Se fixe aux points de trace `sys_enter` et `sys_exit` pour chronométrer au microseconde près la durée des appels système liés aux E/S stockage et réseau.
- **Rendu** : Génère des représentations graphiques de percentiles en ASCII pur (`p50`, `p95`, `p99`, `p99.9`) au sein même de la console, sans dépendance Web externe.

---

## 📊 Principes d'Ingénierie et Philosophie de Code

1. **La Sympathie Mécanique d'Abord** : Les logiciels tournent sur du matériel concret. L'alignement sur les lignes de cache, la prédiction de branchements et la localité NUMA priment toujours sur les abstractions théoriques.
2. **L'Explicite l'Emporte sur l'Implicite** : Aucun code magique. Gestion d'erreurs sans équivoque, sémantique d'appartenance mémoire stricte et indirection minimale à l'exécution.
3. **L'Observabilité N'est Pas une Option Tardive** : Un système qui ne peut être ausculté en temps réel en production sans pénalité de performance est un système inachevé.
4. **Conception Orientée Données (Data-Oriented Design)** : Priorité aux tableaux de mémoire contigus et aux opérations vectorisées face aux hiérarchies d'objets complexes.

---

## 📈 Synthèse des Métriques et Benchmarks

| Métrique Évaluée | Pile TCP Linux Standard | Pipeline Optimisé eBPF / XDP | Gain de Performance |
| :--- | :--- | :--- | :--- |
| **Filtrage de Paquets L4** | 1,82 Mpps (Cœur unique) | 14,45 Mpps (Cœur unique) | **+693,9%** |
| **Surcharge Traçage d'États TCP** | 4,2% CPU Hôte | 0,4% CPU Hôte | **-90,4%** |
| **Latence de Queue des Syscalls (p99)** | 142,6 microsecondes | 18,2 microsecondes | **-87,2%** |
| **Empreinte Mémoire Télémétrie** | 64 Mo de tampons Heap | 4 Mo de Ring Buffer | **-93,7%** |

---

## 📬 Échanges et Coordonnées

- **GitHub Issues & Discussions** : N'hésitez pas à ouvrir un ticket ou lancer une discussion sur mes dépôts publics.
- **E-mail** : `johyojun339@gmail.com`
- **Clé GPG** : `4A8F B120 9C7E 38A1` (Seuls les commits signés sont intégrés)
- **Fuseau Horaire** : KST (UTC+9), actif en journée ainsi que lors de sessions de programmation nocturne.

