| [KR 한국어](./ko.md) | [US English](../../README.md) | [JP 日本語](./ja.md) | [CN 简体中文](./zh-CN.md) | [ES Español](./es.md) |
| :---: | :---: | :---: | :---: | :---: |
| [FR Français](./fr.md) | [DE Deutsch](./de.md) | [RU Русский](./ru.md) | [SA العربية](./ar.md) | [IN हिन्दी](./hi.md) |

---

# Hyojun Jo (조효준)

> **Engenheiro de Software de Sistemas** com foco em Observabilidade do Kernel Linux, Processamento de Pacotes de Alta Vazão e Infraestrutura de Rede de Baixíssima Latência.

Projeto e otimizo software de baixo nível onde cada microssegundo (μs) faz a diferença. Minha atuação concentra-se em instrumentação do kernel com Linux eBPF, pipelines de rede acelerados via XDP, estruturas de dados com eficiência de memória e telemetria determinística para clusters distribuídos em produção.

- 📍 Localização: Seul, Coreia do Sul (UTC+9)
- 💼 Foco Principal: Subsistemas do Kernel Linux, eBPF/XDP, Engenharia de Desempenho de Redes
- 💬 Pergunte-me sobre: Localidade de cache, buffers de socket zero-copy, ring buffers, idiossincrasias do verificador eBPF, E/S mapeada em memória (mmap)
- 📬 Contato: `johyojun339@gmail.com`

---

## 🛠️ Arsenal Técnico (Technical Arsenal)

### Linguagens e Runtimes de Sistemas
| Camada | Tecnologias e Ferramentas |
| :--- | :--- |
| **Sistemas Principais** | Rust (nightly/stable), C (C11/C23), Go (1.23+), Assembly x86-64 / AArch64 |
| **Kernel e Rastreamento** | Linux eBPF (`libbpf`, `aya-rs`), XDP, `kprobes`/`uprobes`, `tracepoints`, `perf_events` |
| **Scripts e Ferramentas de Sistema** | POSIX Shell / Bash, Python 3, PowerShell Core, AWK / Sed |

### Sistemas Operacionais e Infraestrutura
| Domínio | Stack e Ambiente |
| :--- | :--- |
| **Subsistemas do Kernel** | Linux VFS, Netfilter / `nftables`, Socket Buffers (`sk_buff`), cgroups v2, tc-bpf |
| **Observabilidade e Métricas**| OpenTelemetry, Prometheus, DuckDB, ClickHouse, Grafana, Vector |
| **Perfilamento e Depuração** | `bpftool`, `perf`, `gdb`, `valgrind`, `strace`, `flamegraph`, LLVM / Clang 18+ |
| **Protocolos de Rede** | TCP/IP, UDP, QUIC / HTTP3, WireGuard, Fundamentos de BGP / EVPN |

---

## 🚀 Projetos de Código Aberto Ativos

```
+--------------------------------------------------------------------------+
|  flow-kprobe  [Rust / C / eBPF]                                          |
|  Mecanismo de monitoramento do kernel Linux com sobrecarga zero para     |
|  rastreamento de ciclos de vida de sockets TCP e latência de pacotes.    |
+--------------------------------------------------------------------------+
|  wire-sieve   [C / XDP]                                                  |
|  Despachador de pacotes L4 sem estado executado no driver da placa de    |
|  rede (NIC), processando até 14,8 milhões de pacotes/s por núcleo 10GbE. |
+--------------------------------------------------------------------------+
|  nanotrace    [Go / Rust / libbpf]                                       |
|  Utilitário CLI leve para inspecionar latências de cauda em chamadas de |
|  sistema Linux com histogramas sub-microssegundo no terminal.            |
+--------------------------------------------------------------------------+
```

### 1. `flow-kprobe`
- **Arquitetura Central**: Conecta-se a pontos críticos como `tcp_v4_connect`, `tcp_set_state` e `tcp_close` por meio de programas eBPF CO-RE (`Compile Once - Run Everywhere`) modernos.
- **Destaque**: Transmite eventos de ring buffer para o espaço de usuário com latência mínima, eliminando sobrecargas de troca de contexto.
- **Desempenho**: Sob carga sintética de 100.000 requisições/s, a utilização da CPU permanece abaixo de `0,8%` em um servidor com 8 núcleos.

### 2. `wire-sieve`
- **Arquitetura Central**: Contorna a pilha de rede tradicional do Linux através do modo nativo XDP (`XDP_TX` / `XDP_PASS`), descartando pacotes malformados diretamente na placa de rede e roteando pacotes válidos à velocidade do link físico.
- **Filosofia de Design**: Empatia mecânica irrestrita, pools de memória sem alocação dinâmica no caminho crítico e estruturas alinhadas às linhas de cache.

### 3. `nanotrace`
- **Arquitetura Central**: Anexa-se aos tracepoints `sys_enter` e `sys_exit` para medir a duração exata de syscalls em threads de E/S de armazenamento e rede.
- **Exibição**: Gera escalas percentílicas ASCII puras (`p50`, `p95`, `p99`, `p99.9`) diretamente no console sem exigir interfaces Web externas.

---

## 📊 Princípios de Engenharia e Filosofia de Código

1. **Empatia Mecânica Acima de Tudo**: Software é executado em hardware físico. Alinhamento de cache-line, ergonomia para preditores de salto e localidade de nós NUMA têm precedência sobre abstrações puramente teóricas.
2. **O Explícito Prevalece Sobre o Implícito**: Sem código mágico. Retornos de erro explícitos, semântica rígida de posse de memória e menor indireção possível em tempo de execução.
3. **Observabilidade Não é Algo Secundário**: Um sistema que não pode ser inspecionado sob carga em produção sem degradação de performance está incompleto.
4. **Design Orientado a Dados (Data-Oriented Design)**: Otimização de layouts para arrays contíguos de memória e operações vetorizadas no lugar de hierarquias complexas de objetos.

---

## 📈 Resumo de Benchmarks do Sistema

| Métrica Avaliada | Pilha TCP Padrão do Linux | Pipeline Otimizado eBPF / XDP | Ganho de Desempenho |
| :--- | :--- | :--- | :--- |
| **Filtragem de Pacotes L4** | 1,82 Mpps (Núcleo único) | 14,45 Mpps (Núcleo único) | **+693,9%** |
| **Sobrecarga de Rastreamento TCP** | 4,2% da CPU Host | 0,4% da CPU Host | **-90,4%** |
| **Latência de Cauda de Syscalls (p99)** | 142,6 microssegundos | 18,2 microssegundos | **-87,2%** |
| **Consumo de Memória de Telemetria**| 64 MB Buffers de Heap | 4 MB Ring Buffer | **-93,7%** |

---

## 📬 Contato e Colaboração

- **GitHub Issues & Discussions**: Sinta-se à vontade para abrir uma issue ou iniciar uma discussão nos meus repositórios públicos.
- **E-mail**: `johyojun339@gmail.com`
- **Chave GPG**: `4A8F B120 9C7E 38A1` (Somente commits assinados criptograficamente)
- **Fuso Horário**: KST (UTC+9), ativo durante o dia e em sessões noturnas de desenvolvimento.

