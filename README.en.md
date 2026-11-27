<div align="right">

[![Portuguese](https://img.shields.io/badge/Language-Portuguese-green?style=flat-square)](https://github.com/daviroodrigues/daviroodrigues/blob/main/README.md)

</div>

# Davi Rodrigues
### Full-Stack Developer | Go (Backend & Distributed Systems) • Front-end / Mobile • Infrastructure & Security

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/daviroodrigues/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:daviroodrigues@gmail.com)
[![X](https://img.shields.io/badge/X-000000?style=for-the-badge&logo=x&logoColor=white)](https://twitter.com/daviroodriguess)

**Full-Stack** Developer focused on **Go backend architecture**, **event-driven distributed systems**, and **production Linux infrastructure with containers**, alongside **web and mobile interface** development. Hands-on experience from data modeling and asynchronous pipelines to edge-isolated deployments, applied cryptography, and operational resilience.

---

## Projects & Technical Deliverables

### [Esmaga Link | Distributed URL Shortener & Telemetry API](https://github.com/daviroodrigues/esmaga-link)
*High-concurrency backend platform designed under event-driven architecture and CQRS, sustaining redirects at **P99 < 50 ms** latency.*
- **Stack:** Go, Apache Cassandra, Redis, PostgreSQL, RabbitMQ, Nginx, Docker.
- **Engineering & Architectural Decisions:**
  - **Collision-Free O(1) ID Generation:** Deterministic modular bijection over a finite ring with progressive eras, eliminating distributed locks and prior uniqueness lookups.
  - **Stateless Horizontal Scaling:** Sequence block allocation via *Batch Leasing* in **Redis** paired with in-memory atomic counters (`sync/atomic`) and intra-batch permutation.
  - **Hot Path vs. Cold Path Isolation:** `302` redirects (*zero body allocation*) served through a two-tier cache (in-memory L1 + **Redis** L2) with **Cassandra** as the *Single Source of Truth*, while click events are ingested in micro-batches via **RabbitMQ** by dedicated *workers*.
  - **Resilience & Edge Security:** State self-recovery via Cassandra checkpoints using LWT (Paxos), SSRF protection with DNS validation, distributed *Rate Limiting* (*Token Bucket* via Redis Lua scripts), JWT authentication, and **Swagger** documentation.

### [ChatNip | Multi-Tenant WhatsApp Gateway & Queue-Driven Dispatcher](https://github.com/daviroodrigues/ChatNip)
*Modular Go infrastructure built on `whatsmeow` to integrate ERPs, CRMs, and automations with WhatsApp featuring egress isolation, behavioral emulation, and an asynchronous pipeline.*
- **Stack:** Go, RabbitMQ (AMQP), SQLite, Nginx, Docker, FFmpeg, OpenGPG, S3-compatible Object Storage.
- **Engineering & Architectural Decisions:**
  - **Behavioral Bio-Emulation (HSEM v3.1) & Preemptive Serialization:** *Full Spectrum Bio-Emulation* engine featuring `SessionSerializer`, dedicated `SessionWorker`, and `SessionGate` (per-session mutual exclusion with cooperative fast-lane preemption), burst batching, persistent SQLite state tracking (fatigue, per-chat warmth, sleep window, and circadian rhythm), persona profiles (`PADRAO`, `APRESSADO`, `METICULOSO`, `DISTRAIDO`), and stochastic/biometric digraph typing modeling.
  - **Single-Edge Perimeter & Per-Session Isolation:** Only **Nginx** exposes host ports while all other services run inside an isolated Docker network; each session requires a **dedicated HTTP/HTTPS proxy** with runtime validation and rotation support, preventing shared IP reputation.
  - **Prioritized AMQP Messaging & DLQ:** Dedicated queues for low-latency (`whatsapp_fast_lane`) and standard (`whatsapp_standard_lane`) traffic, asynchronous group creation, a *Dead Letter Exchange* (`whatsapp_dlx`), and granular event dispatching for delivery, read receipts, presence, and calls.
  - **Security, Cryptography & Fail-Fast Startup:** Separation between master admin key (`X-Internal-Key`) and client keys (`X-API-Key`, shown once and stored with **Argon2id** hashing), sensitive credentials encrypted at rest in SQLite via **AES-256-GCM**, and strict environment validation with zero silent fallbacks.
  - **Zero-Disk-Secret Backup (GFS + OpenGPG):** Hot SQLite snapshots with *Grandfather-Father-Son* retention, asymmetric OpenGPG encryption keeping only the public key on the server, and automated sync to any **S3-compatible Object Storage** (AWS S3, Cloudflare R2, MinIO, etc.).

### [Anker C200 Control | Native Linux GUI for the Anker PowerConf C200 Webcam](https://github.com/daviroodrigues/anker-c200-ui)
*Desktop application built with Tauri, Rust, and C11 bringing full feature parity with the vendor's official software to Linux, combining direct V4L2/UVC hardware control with real-time video preview.*
- **Stack:** Rust, C11 (V4L2 / UVC), Tauri, HTML5, CSS3, JavaScript (ES6+), WebRTC, Linux.
- **Engineering & Architectural Decisions:**
  - **C11 Driver Layer + Rust FFI:** Low-level mapping of UVC *Extension Units* and direct `ioctl` calls on `/dev/video*` linked to the **Tauri/Rust** core via `cc`, expanding the controls from the base [`anker-powerconf-c200-linux-tools`](https://github.com/erans/anker-powerconf-c200-linux-tools) project through USB traffic analysis.
  - **IPC Command Queue & Bidirectional Sync:** Serialized front-end/backend command execution to prevent kernel driver race conditions, initial hardware state sync (`get_camera_control`), and automatic dependency handling between auto and manual controls.
  - **Lightweight Vanilla UI & WebRTC Live Preview:** Zero-framework front-end featuring real-time `getUserMedia` preview, digital ePTZ controls, FOV/resolution presets (up to 2K), and native Linux packaging (`.deb` and `AppImage`).

### Linux Infrastructure, Containers & Edge Security
*Server provisioning, isolation, and operations for web applications and APIs.*
- **Stack:** Linux (Ubuntu), Docker (Rootless), Cloudflare, AWS (S3).
- **Deliverables:**
  - Service deployment and orchestration on **Ubuntu** VPS servers using **rootless Docker** for process isolation and attack surface reduction.
  - DNS, reverse proxy, SSL/TLS certificates, and edge protection via **Cloudflare**, along with object storage on **Amazon S3**.

---

## Tech Stack

### Languages & Frameworks
![Go](https://img.shields.io/badge/Go-00ADD8?logo=Go&logoColor=white&style=for-the-badge)
![Rust](https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white)
![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![Tauri](https://img.shields.io/badge/Tauri-24C8D8?style=for-the-badge&logo=tauri&logoColor=white)
![Flutter](https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white)
![Dart](https://img.shields.io/badge/dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white)
![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/css3-%231572B6.svg?style=for-the-badge&logo=css3&logoColor=white)

### Data, Cache & Messaging
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Cassandra](https://img.shields.io/badge/Cassandra-%231287B1.svg?style=for-the-badge&logo=apache-cassandra&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/Rabbitmq-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)

### Infrastructure, Cloud & Edge
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=Cloudflare&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-web-services&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)

### APIs, Design & Tooling
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=Swagger&logoColor=black)
![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white)
