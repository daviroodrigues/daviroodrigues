<div align="right">

[![English](https://img.shields.io/badge/Language-English-blue?style=flat-square)](https://github.com/daviroodrigues/daviroodrigues/blob/main/README.en.md)

</div>

# Davi Rodrigues
### Desenvolvedor Full-Stack | Go (Backend e Sistemas Distribuídos) • Front-end / Mobile • Infraestrutura e Segurança

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/daviroodrigues/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:daviroodrigues@gmail.com)
[![X](https://img.shields.io/badge/X-000000?style=for-the-badge&logo=x&logoColor=white)](https://twitter.com/daviroodriguess)

Desenvolvedor **Full-Stack** com foco em **arquitetura backend em Go**, **sistemas distribuídos orientados a eventos** e **infraestrutura Linux em produção com containers**, além de desenvolvimento de **interfaces web e mobile**. Experiência prática da modelagem de dados e pipelines assíncronos ao deploy com isolamento de borda, criptografia aplicada e resiliência operacional.

---

## Projetos e Entregas Técnicas

### [Esmaga Link | API Distribuída de Encurtamento e Telemetria](https://github.com/daviroodrigues/esmaga-link)
*Plataforma backend de alta concorrência projetada sob arquitetura orientada a eventos e CQRS, sustentando redirecionamentos com latência **P99 < 50 ms**.*
- **Stack:** Go, Apache Cassandra, Redis, PostgreSQL, RabbitMQ, Nginx, Docker.
- **Engenharia e Decisões Arquiteturais:**
  - **Geração de IDs em O(1) sem colisões:** Bijeção modular determinística sobre anel finito com eras progressivas, eliminando travas distribuídas e consultas prévias de unicidade.
  - **Escala horizontal (Stateless):** Alocação de blocos de sequências via *Batch Leasing* no **Redis** combinada a contadores atômicos em memória (`sync/atomic`) e permutação intra-lote.
  - **Isolamento Hot Path vs. Cold Path:** Redirecionamento `302` (*zero body allocation*) atendido por cache em dois níveis (L1 em memória + L2 **Redis**) e **Cassandra** como *Single Source of Truth*, enquanto eventos de clique são ingeridos em micro-lotes via **RabbitMQ** por *workers* dedicados.
  - **Resiliência e Segurança de Borda:** Auto-recuperação de estado via checkpoints no Cassandra com LWT (Paxos), proteção contra SSRF com validação DNS, *Rate Limiting* distribuído (*Token Bucket* via scripts Lua no Redis), autenticação JWT e documentação **Swagger**.

### [ChatNip | Gateway de WhatsApp Multi-Tenant e Despachante Orientado a Filas](https://github.com/daviroodrigues/ChatNip)
*Infraestrutura modular em Go sobre `whatsmeow` para integrar ERPs, CRMs e automações ao WhatsApp com isolamento de egressão, emulação comportamental e pipeline assíncrono.*
- **Stack:** Go, RabbitMQ (AMQP), SQLite, Nginx, Docker, FFmpeg, OpenGPG, Object Storage (S3-compatível).
- **Engenharia e Decisões Arquiteturais:**
  - **Bio-Emulação Comportamental (HSEM v3.1) e Virtual Sub-Queues:** Máquina de estados de presença (`DORMANT` → `WARMING UP` → `ACTIVE FLOW` → `COOL DOWN`) combinada a sub-filas virtuais por destinatário (VSQ) para serializar interações e simular cadência humana, mitigando bloqueios.
  - **Perímetro Único (Single-Edge) e Isolamento por Sessão:** Apenas o **Nginx** expõe portas ao host enquanto os demais serviços operam em rede Docker isolada; cada sessão exige um **proxy HTTP/HTTPS dedicado** com suporte a validação e rotação em tempo de execução, evitando compartilhamento de reputação de IP.
  - **Mensageria AMQP com Priorização e DLQ:** Filas dedicadas para baixa latência (`whatsapp_fast_lane`) e fluxo padrão (`whatsapp_standard_lane`), criação assíncrona de grupos, *Dead Letter Exchange* (`whatsapp_dlx`) e despacho granular de eventos de entrega, leitura, presença e chamadas.
  - **Segurança, Criptografia e Startup Fail-Fast:** Separação entre chave administrativa mestre (`X-Internal-Key`) e chaves de clientes (`X-API-Key` exibidas uma única vez e persistidas com hash **Argon2id**), credenciais sensíveis cifradas em repouso no SQLite com **AES-256-GCM** e validação estrita de ambiente sem fallbacks silenciosos.
  - **Backup Zero-Disk-Secret (GFS + OpenGPG):** Snapshots a quente do SQLite com retenção *Grandfather-Father-Son*, criptografia assimétrica OpenGPG mantendo apenas a chave pública no servidor e sincronização automatizada com qualquer **Object Storage compatível com S3** (AWS S3, Cloudflare R2, MinIO, etc.).

### [Anker C200 Control | Interface Nativa Linux para Webcam Anker PowerConf C200](https://github.com/daviroodrigues/anker-c200-ui)
*Aplicação desktop em Tauri, Rust e C11 que leva paridade total com o software oficial do fabricante ao Linux, integrando controle direto de hardware V4L2/UVC com preview em tempo real.*
- **Stack:** Rust, C11 (V4L2 / UVC), Tauri, HTML5, CSS3, JavaScript (ES6+), WebRTC, Linux.
- **Engenharia e Decisões Arquiteturais:**
  - **Camada de Driver em C11 + FFI em Rust:** Mapeamento de extensões UVC (*Extension Units*) e chamadas `ioctl` diretas sobre `/dev/video*` vinculadas ao núcleo **Tauri/Rust** via `cc`, expandindo os controles originais do projeto base [`anker-powerconf-c200-linux-tools`](https://github.com/erans/anker-powerconf-c200-linux-tools) a partir de análise de tráfego USB.
  - **Fila de Comandos IPC & Sincronização Bidirecional:** Execução serializada entre front-end e backend para evitar *race conditions* no driver do kernel, com leitura inicial do estado real do hardware (`get_camera_control`) e resolução automática de dependências entre modos automáticos e manuais.
  - **Front-end Enxuto e Live Preview WebRTC:** Interface sem dependência de frameworks pesados com monitor em tempo real via `getUserMedia`, controles ePTZ digitais, presets de FOV/resolução (até 2K) e empacotamento nativo para Linux (`.deb` e `AppImage`).

### Infraestrutura Linux, Containers e Segurança de Borda
*Provisionamento, isolamento e operação de servidores para aplicações web e APIs.*
- **Stack:** Linux (Ubuntu), Docker (Rootless), Cloudflare, AWS (S3).
- **Entregas:**
  - Implantação e orquestração de serviços em servidores VPS **Ubuntu** utilizando **Docker em modo rootless** para isolamento de processos e redução da superfície de ataque.
  - Configuração de DNS, proxy reverso, certificados SSL/TLS e proteção de borda via **Cloudflare**, além de armazenamento de objetos no **Amazon S3**.

---

## Stack Técnica

### Linguagens & Frameworks
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

### Dados, Cache & Mensageria
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Cassandra](https://img.shields.io/badge/Cassandra-%231287B1.svg?style=for-the-badge&logo=apache-cassandra&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/Rabbitmq-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)

### Infraestrutura, Cloud & Borda
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=Cloudflare&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-web-services&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)

### APIs, Design & Ferramentas
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=Swagger&logoColor=black)
![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white)
