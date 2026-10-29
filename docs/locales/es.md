| [KR 한국어](./ko.md) | [US English](../../README.md) | [JP 日本語](./ja.md) | [CN 简体中文](./zh-CN.md) | [ES Español](./es.md) |
| :---: | :---: | :---: | :---: | :---: |
| [FR Français](./fr.md) | [DE Deutsch](./de.md) | [RU Русский](./ru.md) | [SA العربية](./ar.md) | [IN हिन्दी](./hi.md) |

---

# Hyojun Jo (조효준)

> **Ingeniero de Software de Sistemas** especializado en Observabilidad del Kernel de Linux, Procesamiento de Paquetes de Alto Rendimiento e Infraestructura de Red de Baja Latencia.

Diseño y optimizo software de bajo nivel donde cada microsegundo (μs) cuenta. Mi trabajo se centra en la instrumentación del kernel con Linux eBPF, tuberías de red aceleradas mediante XDP, estructuras de datos con uso eficiente de memoria y telemetría determinista para clústeres distribuidos en producción.

- 📍 Ubicación: Seúl, Corea del Sur (UTC+9)
- 💼 Enfoque principal: Subsistemas del Kernel de Linux, eBPF/XDP, Ingeniería de Rendimiento de Redes
- 💬 Temas de consulta: Localidad de caché, buffers de socket zero-copy, buffers circulares (ring buffers), particularidades del verificador eBPF, E/S mapeada en memoria (mmap)
- 📬 Contacto: `johyojun339@gmail.com`

---

## 🛠️ Arsenal Técnico (Technical Arsenal)

### Lenguajes y Runtimes Principales
| Capa | Tecnologías y Herramientas |
| :--- | :--- |
| **Sistemas Principales** | Rust (nightly/stable), C (C11/C23), Go (1.23+), Ensamblador x86-64 / AArch64 |
| **Kernel e Instrumentación** | Linux eBPF (`libbpf`, `aya-rs`), XDP, `kprobes`/`uprobes`, `tracepoints`, `perf_events` |
| **Scripts y Herramientas del Sistema** | POSIX Shell / Bash, Python 3, PowerShell Core, AWK / Sed |

### Sistemas Operativos e Infraestructura
| Dominio | Pila y Entorno |
| :--- | :--- |
| **Subsistemas del Kernel** | Linux VFS, Netfilter / `nftables`, Socket Buffers (`sk_buff`), cgroups v2, tc-bpf |
| **Observabilidad y Métricas** | OpenTelemetry, Prometheus, DuckDB, ClickHouse, Grafana, Vector |
| **Herramientas y Perfilado** | `bpftool`, `perf`, `gdb`, `valgrind`, `strace`, `flamegraph`, LLVM / Clang 18+ |
| **Protocolos de Red** | TCP/IP, UDP, QUIC / HTTP3, WireGuard, Fundamentos de BGP / EVPN |

---

## 🚀 Proyectos de Código Abierto Activos

```
+--------------------------------------------------------------------------+
|  flow-kprobe  [Rust / C / eBPF]                                          |
|  Motor de sondeo del kernel de Linux con sobrecarga cero para rastrear   |
|  el ciclo de vida de sockets TCP y medir latencia a nivel microsegundo.  |
+--------------------------------------------------------------------------+
|  wire-sieve   [C / XDP]                                                  |
|  Distribuidor de paquetes L4 sin estado que opera a nivel de controlador |
|  de red (NIC), capaz de procesar 14.8M pps por núcleo en enlaces 10GbE.  |
+--------------------------------------------------------------------------+
|  nanotrace    [Go / Rust / libbpf]                                       |
|  Utilidad CLI ligera para examinar latencias de cola (tail latency) en  |
|  llamadas al sistema Linux con histogramas sub-microsegundo en terminal. |
+--------------------------------------------------------------------------+
```

### 1. `flow-kprobe`
- **Arquitectura principal**: Se enlaza a funciones clave como `tcp_v4_connect`, `tcp_set_state` y `tcp_close` mediante programas eBPF CO-RE (`Compile Once - Run Everywhere`) modernos.
- **Característica clave**: Transmite eventos de ring buffer al espacio de usuario con mínima latencia y sin la sobrecarga de cambios de contexto innecesarios.
- **Rendimiento**: Bajo una carga sintética de 100,000 req/s, el uso de CPU permanece por debajo del `0.8%` en un servidor de 8 núcleos.

### 2. `wire-sieve`
- **Arquitectura principal**: Omite la pila de red de Linux mediante el modo nativo de XDP (`XDP_TX` / `XDP_PASS`) para descartar tráfico anómalo y reenviar paquetes legítimos directamente en la tarjeta de red.
- **Filosofía de diseño**: Simpatía mecánica rigurosa, pools de memoria sin asignación dinámica en ruta caliente y estructuras alineadas con las líneas de caché.

### 3. `nanotrace`
- **Arquitectura principal**: Se conecta a los tracepoints `sys_enter` y `sys_exit` para registrar la duración exacta de llamadas al sistema en hilos de E/S de almacenamiento y red.
- **Presentación**: Genera escalas percentiles ASCII (`p50`, `p95`, `p99`, `p99.9`) directamente en la terminal sin requerir interfaces web externas.

---

## 📊 Principios de Ingeniería y Filosofía de Código

1. **Simpatía Mecánica por Encima de Todo**: El software se ejecuta en hardware físico. La alineación con la línea de caché, la predicción de ramas y la localidad NUMA tienen prioridad sobre abstracciones teóricas.
2. **Lo Explícito Supera a lo Implícito**: Cero código mágico. Manejo de errores claro, semántica de pertenencia de memoria estricta y mínima indirección en tiempo de ejecución.
3. **La Observabilidad No es Opcional**: Si un sistema no puede ser inspeccionado bajo carga en producción sin degradación de rendimiento, está incompleto.
4. **Diseño Orientado a Datos (Data-Oriented Design)**: Optimizar para arrays contiguos en memoria y operaciones vectorizadas en lugar de jerarquías de objetos profundas.

---

## 📈 Comparativa de Rendimiento del Sistema

| Métrica Evaluada | Pila TCP Estándar de Linux | Canal eBPF / XDP Optimizado | Mejora Obtenida |
| :--- | :--- | :--- | :--- |
| **Filtrado de Paquetes L4** | 1.82 Mpps (Un núcleo) | 14.45 Mpps (Un núcleo) | **+693.9%** |
| **Sobrecarga de Rastreo TCP** | 4.2% CPU del Servidor | 0.4% CPU del Servidor | **-90.4%** |
| **Latencia de Cola de Syscalls (p99)** | 142.6 microsegundos | 18.2 microsegundos | **-87.2%** |
| **Memoria de Buffer de Telemetría** | 64 MB Buffers de Heap | 4 MB Ring Buffer | **-93.7%** |

---

## 📬 Contacto y Colaboración

- **GitHub Issues y Discusiones**: Siéntete libre de abrir un issue o iniciar una conversación en cualquiera de mis repositorios públicos.
- **Correo electrónico**: `johyojun339@gmail.com`
- **Clave GPG**: `4A8F B120 9C7E 38A1` (Solo se admiten commits firmados criptográficamente)
- **Zona Horaria**: KST (UTC+9), habitualmente activo durante el día y en sesiones nocturnas de desarrollo profundo.

