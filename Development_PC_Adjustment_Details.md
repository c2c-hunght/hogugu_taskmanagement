# Development PC Adjustment – Supporting Details

**Requester:** Hoang Tuan Hung  **Date:** 2026-10-16

Measurements and workload behind the main document (sections 1–4).

## 5. Measured problems
The Mac restarted at 13:01 on 2026-10-16, so the figures below compare a fresh idle state, the dev stack running, and normal use before the restart.

**Memory: fine at idle, full as soon as the dev stack runs**

| State (2026-10-16) | Free RAM | Compressed memory | Load average (1 min)* |
|---|---|---|---|
| Idle: no containers, only VS Code, Claude Code and a terminal open (13:25) | about **6 GB** | 4.3 GB squeezed into 1.8 GB | 3.1 (about 40%) |
| Dev stack running (13:10, see section 6) | about **80 MB** | 8.7 GB squeezed into 4.5 GB | **12.1 (about 150%)** |
| 1 minute after stopping the stack (13:14) | about **80 MB** | 9.9 GB squeezed into 5.5 GB | 1.9 (about 25%) |
| Before the reboot, after 8 days of normal use | about **70 MB** | 12 GB squeezed into 6.5 GB, plus **2.6 GB swap** | 4–5 (about 50–60%) |

\* Load average is the average number of tasks running on or waiting for the CPU. With 8 cores, 8.0 equals 100% (all cores busy all the time).

- At idle the Mac is usable. Starting the HOGUGU stack takes it from about 6 GB free to almost nothing.
- Memory does not come back right away after stopping the stack.
- During normal day-to-day use (8 days of uptime) the Mac also ran out of RAM and had to use swap.
- Docker is allowed up to 8 GB, half of all RAM.
- Microsoft Defender was still using about 57% of one CPU core even at idle.

**Disk: 83% full**
- Only **36–38 GB free** out of 245 GB (the figure moves slightly between measurements)

Where the 245 GB goes (measured 2026-10-16; sizes are approximate):

| Area | Size | What it is |
|---|---|---|
| macOS itself (System, Preboot, Recovery volumes) | 26 GB | Operating system; cannot be reduced |
| Docker | 20 GB | Images, volumes and containers for the HOGUGU stack (MongoDB, OpenSearch, Elasticsearch and Node images alone take 1–1.4 GB each) |
| Project source code (`~/Desktop/dev`) | 17 GB | HOGUGU 11 GB (repos, data exports, analytics); other projects about 6 GB |
| System-wide `/Library` | 15 GB | Shared app data and frameworks used by installed apps |
| Dev toolchains and package caches | 11 GB | npm 2.4, Node versions (nvm) 2.3, Homebrew 1.7, Android SDK 1.7, Rust 1.3, pnpm 1.3, Go/Cargo 0.5 |
| Claude desktop app | 11 GB | Mostly its local VM bundle (9.8 GB) |
| Applications (`/Applications`) | 11 GB | Installed apps |
| VS Code data and extensions | 8 GB | Workspace storage 6.3 GB, extensions 1.4 GB |
| iOS Simulator data | 5 GB | Left over from earlier mobile work |
| App caches (`~/Library/Caches`) | 5 GB | Can be cleared, but grows back |
| Other app data and files | 11 GB | Slack, Chrome, Downloads, logs and system temp files |
| Not visible without admin rights | about 55–60 GB | System databases, Spotlight index, other accounts on this Mac and file-system overhead |
| **Free** | **36–38 GB** | |

**Cleanup would not solve this:** the space that can safely be freed is about 10–12 GB (unused Docker images and stopped containers about 5 GB, old project zip files about 2 GB, caches about 5 GB). That is still less than the 45–65 GB Xcode and Android Studio need.

**Signs of instability**
- Several local service containers (booking-api, account-api, coupon-stub, gmo-stub) last exited with code **137**, which means they were force-killed. That is often caused by running out of memory.

## 6. Load with the full dev stack running
Measured on 2026-10-16 at 13:10, with the backend stack started (15 of 16 containers up). This was **9 minutes after a reboot**, with no Xcode or Android Studio and few apps open, so it is a **best case**.

| Metric | Value | Meaning |
|---|---|---|
| Load average (1 / 5 / 15 min) | **12.1 / 16.9 / 13.0** on 8 cores (about **150% / 210% / 160%**) | CPU demand is 1.5–2× what the cores can handle; the excess waits in a queue |
| Free RAM | **80 MB** | RAM was already full within 9 minutes of booting |
| Compressed memory | 8.7 GB of data squeezed into 4.5 GB of RAM | macOS is compressing memory hard to avoid swapping |
| Docker VM process | **5.6–8.2 GB** RAM, about 45% CPU | Half of the Mac's memory goes to Docker |
| Microsoft Defender | **77% CPU** | Antivirus scanning adds CPU load on top of the dev stack |
| Free disk | 36 GB (83% used) | Not enough room to install Xcode and Android Studio |

Memory used by each container (total about 3.7 GB, plus the overhead of the Docker VM itself):

| Container | RAM | Container | RAM |
|---|---|---|---|
| elasticsearch (OpenSearch) | 1.14 GB | rabbitmq | 185 MB |
| dashboard-app | 998 MB | account-api | 171 MB |
| mongodb | 324 MB | matching / payment / payout | about 100 MB each |
| web-booking | 260 MB | booking-api | 81 MB (54% CPU) |
| other services and stubs | under 30 MB each | api-gateway | not running (config error, unrelated to hardware) |

The next steps in daily work (opening VS Code, Chrome and Slack, then Xcode or Android Studio) all need more memory on top of this, and none is left.

## 7. Workload this machine has to handle
**Docker stack** (`hogugu-docker` plus per-service compose files):
- Infrastructure: MongoDB, Redis, RabbitMQ, OpenSearch (JVM), nginx gateway
- Node services: booking-api, account-api, matching, payment, payout, media, review, dashboard-app, web-booking, coupon/GMO stubs
- 16 containers in total, built on Node 16, 18 and 20 images (plus a separate MariaDB test container)

**At the same time:**
- VS Code with multiple repos open: about 3 GB
- **Xcode + iOS Simulator** for mobile app builds: about 4–8 GB RAM; about 30–40 GB disk for Xcode, iOS runtimes and DerivedData
- **Android Studio + Emulator** for mobile app builds: about 6–10 GB RAM (IDE and Gradle about 3–4 GB, emulator about 2–4 GB); about 15–25 GB disk for the SDK, system images and Gradle cache
- AI coding tools (Claude Code, Rovo Dev): about 0.5–1 GB
- Slack, Chrome, Defender: about 1.5 GB
- Data and analytics work: Mongo exports

**Other projects:** if this Mac is also used for other company projects, each one brings its own:
- Docker containers and databases, which run on top of the HOGUGU stack or have to replace it
- Runtime versions and dependencies (Node)
- IDE window and build processes

Today, switching to another project means stopping the HOGUGU stack first, because there is not enough RAM to run both. Assigning new work to a different project is limited by this machine.

Xcode and Android Studio are **not currently installed** because there is not enough free disk space. Together they need about 45–65 GB, and only 36 GB is free. Simulator data (5 GB) and an Android SDK (1.7 GB) are already on the machine.

**Rough RAM needed:**
- Backend stack only: Docker 10–14 GB + IDE and tools 5–6 GB + macOS 3–4 GB ≈ **18–24 GB**
- Backend + mobile (Xcode or Android Studio running): ≈ **26–34 GB**
- HOGUGU stack + another project's stack running at the same time: adds about **4–8 GB** to either case

All cases are well above 16 GB.
