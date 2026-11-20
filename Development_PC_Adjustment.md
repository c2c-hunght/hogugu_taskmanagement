# Development PC Adjustment (HOGUGU and other projects)

**Requester:** Hoang Tuan Hung  **Date:** 2026-10-16

## 1. Summary
The current MacBook Air (M2, 16 GB RAM, 256 GB SSD) is **enough for normal work**.
It is not enough for the following development work:
- Full-stack HOGUGU development (the whole local stack)
- Mobile app development (Xcode / Android Studio)
- Working on other projects in parallel when needed

Section 3 lists 3 options to cover this. Measurements and other details are in the separate document "Development PC Adjustment – Supporting Details" (sections 5–7).

## 2. Current PC
| Item | Spec |
|---|---|
| Model | MacBook Air (Mac14,2), 2022 |
| CPU / GPU | Apple M2: 8-core CPU (4P + 4E), 8-core GPU |
| RAM | **16 GB** (cannot be upgraded) |
| Storage | **256 GB** SSD (245 GB usable) (cannot be upgraded) |
| Cooling | Fanless, so it slows down under sustained load |
| OS | macOS 27.0.1 |
| Battery | 135 cycles, 93% capacity (healthy) |

**What the current PC can handle**

| Use | Current PC | Why |
|---|---|---|
| Normal work (email, Slack, browser, documents, reading and light editing of code) | OK | About 6 GB RAM free and about 40% CPU load at idle |
| Full-stack HOGUGU development (16 Docker containers) | Not enough | Free RAM drops to almost zero and CPU load reaches about 150–210% (section 6) |
| Mobile app development (Xcode / Android Studio) | Not enough | Installing them needs 45–65 GB, but only 36 GB is free (section 5) |
| Other projects in parallel | Not enough | HOGUGU has to be stopped before switching (section 7) |

## 3. Options
The development work above needs **at least 32 GB of RAM** (backend only about 18–24 GB, with mobile about 26–34 GB, plus 4–8 GB for another project in parallel; see section 7).

| Option | Spec | Estimated price | Pros | Cons |
|---|---|---|---|---|
| A. Minimum | MacBook Pro 14" (M5), **32 GB**, 1 TB | **¥411,800** | Runs the backend stack, has a fan for sustained load; 1 TB fits Xcode + Android Studio | RAM is tight when the backend stack and a mobile simulator/emulator run together |
| **B. Recommended** | MacBook Pro 14" (M5 Pro), **48 GB**, 1 TB | **¥567,800** | Runs the backend stack and mobile builds together, room to run another project alongside HOGUGU, faster Xcode/Gradle builds, headroom for 3–4 years | Higher cost |
| C. Budget | MacBook Air 13" (M5), 32 GB, 512 GB | about ¥280,000–320,000 (estimate) | Cheapest fix for RAM | Fanless (slows under load); 512 GB is tight for Docker + Xcode + Android Studio |

\* Prices are estimates for the newest Mac models. Choosing an older model with similar RAM and SSD would likely be somewhat cheaper.

## 4. Expected benefit
- The full local stack can run without containers being killed or the machine swapping heavily
- Xcode and Android Studio can be installed and used alongside the backend stack
- Other projects can be worked on without shutting down HOGUGU first, so the same machine can be assigned to more than one project
- Less waiting during builds and tests, and when switching between IDE, Docker, and browser
- Enough disk for Docker images and data exports without constant cleanup
