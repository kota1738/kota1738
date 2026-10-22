<div align="center">

<img src="https://img.shields.io/badge/SPHINX-TRANSMISSION_09_%7C_12-15252a?style=for-the-badge&labelColor=0e1a1e" alt="Sphinx transmission 09 12" />

### `TWELVE`

**Abdurrahmaan Lakhota** — Backend &amp; infrastructure engineering
Services that run reliably, data that survives failure, and the Linux estate they sit on.

<img src="https://img.shields.io/badge/status-final_year,_grad_2026-4a757e?style=flat-square&labelColor=0e1a1e" alt="status" />
<img src="https://img.shields.io/badge/CCNA-in_progress-4a757e?style=flat-square&labelColor=0e1a1e" alt="CCNA" />
<img src="https://img.shields.io/badge/location-United_Kingdom-4a757e?style=flat-square&labelColor=0e1a1e" alt="location" />
<img src="https://img.shields.io/badge/open_to-backend_%2F_platform-24393f?style=flat-square&labelColor=0e1a1e" alt="open to work" />

**[kota.dpdns.org](https://kota.dpdns.org)**

</div>

---

## Signal

Final-year Computer Science student, graduating 2026 (predicted 2:1), working through the **CCNA**. Freelance IT and web services since 2024 — hardware and OS support, network setup, small business sites.

Most of what I learn happens on hardware I own. I run **thirty containers** on ARM64 Fedora with SELinux enforcing, defined in a single Docker Compose file. Egress for selected containers is policy-routed through a VPN gateway, Pi-hole handles DNS filtering, and backups run on a schedule with rsync.

Public services are exposed through a **Cloudflare Tunnel** — an outbound-only connector, so there is no port forwarding and **no inbound port open on the network**. This README's sibling site is served that way.

I am moving that operational experience towards software: writing the services, not only running them. The first of those is below.

Looking for **backend, platform and infrastructure roles**.

---

## Projects

Three projects, built in the open and in this order. Two run on the server described above; the third is a repository you can clone and test. Nothing on this page is a plan dressed up as a finished thing — the status is on every entry.

### 01 — Distributed task queue API &nbsp;`running`

[**distributed-task-queue**](https://github.com/kota1738/distributed-task-queue) — an asynchronous job-processing service. Clients submit work over REST and get an id back immediately; separate worker processes claim jobs and execute them, and every job is trackable through `queued`, `running`, `succeeded`, `failed` and `cancelled`.

Python · FastAPI · PostgreSQL · Redis · Docker · pytest · GitHub Actions

- Job state and JSON results live in **PostgreSQL**. Redis is only a wake-up channel, so a broker outage delays work instead of losing it.
- Workers claim jobs atomically with `SELECT … FOR UPDATE SKIP LOCKED` plus a status guard, so concurrent workers never run the same job twice.
- Retries with **exponential backoff** and per-job attempt limits, idempotency keys so client retries are safe, and a lease reaper that requeues jobs orphaned by a crashed worker.
- JWT bearer auth with per-user job isolation; payloads validated against the job type's schema **before** the row is written, so a bad job fails with `422` rather than inside a worker.
- **68 tests** covering API, worker, auth and handlers, run in CI against real PostgreSQL and Redis; Docker Compose deployment with readiness and liveness probes.
- Deployed as **five containers** on the server, with the API and database bound to loopback and Prometheus scraping its `/metrics` over a private Docker network.

```bash
git clone https://github.com/kota1738/distributed-task-queue
cd distributed-task-queue && python scripts/demo.py   # full lifecycle, no services needed
```

### 02 — Infrastructure monitoring and alerting &nbsp;`deployed`

[**monitoring-stack**](https://github.com/kota1738/monitoring-stack) — a seven-container observability stack running on the server above: **Prometheus**, **Alertmanager**, **Grafana**, node_exporter, cAdvisor and blackbox_exporter, each under a memory limit so monitoring cannot starve the media services it watches.

- **13 alert rules** across host, container, availability and task-queue groups, plus a deliberately permanent Watchdog: a silent pipeline and a healthy system look identical otherwise.
- **Three dashboards** provisioned from version-controlled JSON, so a fresh deployment needs no manual setup.
- **Eight scrape targets, all up**, including the task queue API's own metrics inside the cluster.
- **Delivery proven, not assumed:** a real disk alert at 13% free fired, and a hand-fired test alert and its resolution both reached Discord. `alert-sink` records every notification, which is the only way to show alerts actually arrive.

Stack: Prometheus · Alertmanager · Grafana · cAdvisor · Docker · Linux

### 03 — Staff management system &nbsp;`shipped`

[**staff-management**](https://github.com/kota1738/staff-management) — a C# REST API over EF Core: staff records with search, filtering and paging, cookie sessions with PBKDF2 hashes, and Viewer/Manager/Admin policies enforced at the HTTP layer.

- **Email uniqueness is a database index**, and a test proves it by inserting a duplicate directly rather than trusting the handler.
- **16 integration tests** drive the real pipeline through `WebApplicationFactory`, covering 401 and 403 paths, validation, duplicates and paging; CI builds with warnings as errors.
- SQL Server migrations plus Docker Compose, so the deployment is repeatable rather than remembered. Not deployed: the evidence is the suite and the recipe, not a URL.

Stack: C# · ASP.NET Core · EF Core · SQL Server · xUnit

---

## Apparatus

<div align="center">

<img src="https://img.shields.io/badge/Python-15252a?style=for-the-badge&logo=python&logoColor=7cb2bd" alt="Python" />
<img src="https://img.shields.io/badge/FastAPI-15252a?style=for-the-badge&logo=fastapi&logoColor=7cb2bd" alt="FastAPI" />
<img src="https://img.shields.io/badge/PostgreSQL-15252a?style=for-the-badge&logo=postgresql&logoColor=7cb2bd" alt="PostgreSQL" />
<img src="https://img.shields.io/badge/Redis-15252a?style=for-the-badge&logo=redis&logoColor=7cb2bd" alt="Redis" />
<br />
<img src="https://img.shields.io/badge/TypeScript-15252a?style=for-the-badge&logo=typescript&logoColor=7cb2bd" alt="TypeScript" />
<img src="https://img.shields.io/badge/C%23-15252a?style=for-the-badge&logo=dotnet&logoColor=7cb2bd" alt="C#" />
<img src="https://img.shields.io/badge/Bash-15252a?style=for-the-badge&logo=gnubash&logoColor=7cb2bd" alt="Bash" />
<br />
<img src="https://img.shields.io/badge/Linux-15252a?style=for-the-badge&logo=linux&logoColor=7cb2bd" alt="Linux" />
<img src="https://img.shields.io/badge/Fedora-15252a?style=for-the-badge&logo=fedora&logoColor=7cb2bd" alt="Fedora" />
<img src="https://img.shields.io/badge/Docker-15252a?style=for-the-badge&logo=docker&logoColor=7cb2bd" alt="Docker" />
<img src="https://img.shields.io/badge/Git-15252a?style=for-the-badge&logo=git&logoColor=7cb2bd" alt="Git" />
<br />
<img src="https://img.shields.io/badge/Networking-15252a?style=for-the-badge&logo=cisco&logoColor=7cb2bd" alt="Networking" />
<img src="https://img.shields.io/badge/pandas-15252a?style=for-the-badge&logo=pandas&logoColor=7cb2bd" alt="pandas" />

</div>

| Area | Detail |
| :--- | :--- |
| **Languages** | Python, SQL, C#, TypeScript / JavaScript, Bash |
| **Backend** | FastAPI, ASP.NET Core, EF Core, REST API design, SQLAlchemy, pydantic validation, JWT authentication, background workers, pytest, xUnit |
| **Data** | PostgreSQL, Redis, SQLite, SQL Server |
| **Observability** | Prometheus, Grafana, Alertmanager, node_exporter, cAdvisor, blackbox probes, dashboards as code, runbooks |
| **Systems** | Linux (Fedora, Debian), Windows, macOS / Apple Silicon, Docker &amp; Compose, SELinux |
| **Networking** | TCP/IP, routing &amp; switching, VLANs, DNS &amp; DNS filtering, reverse proxy, tunnelling, VPN gateway routing — CCNA in progress |
| **Support** | Hardware diagnostics, OS deployment, backup &amp; recovery, end-user troubleshooting |

---

## Countdown

```
2024 ─── freelance IT & web services                       [ running ]
2025 ─── homelab: 15 services, 0 inbound ports open        [ running ]
2026 ─── distributed-task-queue, tested end to end         [ shipped   ]
2026 ─── monitoring stack deployed · staff API shipped     [ shipped   ]
2026 ─── BSc Computer Science                              [ 00 : xx  ]
20XX ─── CCNA                                              [ counting ]
```

---

## Contact

| | |
| :--- | :--- |
| **Email** | [abdurrahmaanlakhota@gmail.com](mailto:abdurrahmaanlakhota@gmail.com) |
| **Site** | [kota.dpdns.org](https://kota.dpdns.org) |
| **GitHub** | [@kota1738](https://github.com/kota1738) |

Open to backend, platform, infrastructure and IT support roles. Happy to talk about homelabs at greater length than is strictly reasonable.

<div align="center">
<br />
<sub><code>09 | 12</code> — end of transmission.</sub>
</div>
