<div align="center">

# Hi, I'm Kirill 👋

**Systems & Fullstack Software Engineer** · Rust · TypeScript · Networking

*I build network protocols from the socket up — and the web apps that sit on top of them.*

[![Website](https://img.shields.io/badge/netrunner--vpn.com-000?style=for-the-badge&logo=cloudflare&logoColor=white)](https://netrunner-vpn.com)
[![Telegram](https://img.shields.io/badge/Telegram-@ktr2102-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/ktr2102)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Kirill_Trapeznikov-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/kirill-trapeznikov-57b450374/)
[![Email](https://img.shields.io/badge/trkir21@gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:trkir21@gmail.com)

📍 Vladivostok → Pattaya, Thailand 🇹🇭 · 🌍 **Open to remote work worldwide**

</div>

---

## ⚡ TL;DR

- 🦀 Designed and built **NRXP / Netrunner** — a multiplexed, multipath L3 VPN in **Rust** that travels as ordinary HTTPS (TLS 1.3, Chrome fingerprint). Client for Linux / OpenWrt / Android, server node, mesh, docs, tests.
- 🧠 Went down to the TCP/IP layer: forked **smoltcp** and added a from-scratch **BBR** congestion controller, BDP-sized dynamic buffers and AQM for a userspace tunnel.
- 🌐 Before that: **fullstack product work** (React / Next.js / Nest.js) on a high-traffic fintech platform — **-50 % page load, -40 % build time, re-renders cut from 20–30 to 2–3**.
- 🧰 Broad market stack beyond the core focus: **C# / .NET** ecosystem (Unity, fintech teams), **Java / Kotlin** mobile, **Spring**, Node, React.
- 📦 Ship open-source: a **WebAssembly game library** on npm and a **state-management library** for React.

---

## 🛰️ Netrunner — what I've built

> The main project of the last year. Everything from wire format to node deployment to the landing page. Public pieces below; the production multiplexer and apps are closed source.

### 🔐 [`netrunner-core`](https://github.com/selbstnull/netrunner-core) — VPN-proxy core · Rust · AGPL-3.0
*~2.6 MB of Rust, 300+ commits, 244 unit tests in the protocol crate.*

| Area | What's in it |
|---|---|
| **Transport** | Up to 4 parallel TCP "legs" per session; streams migrate seamlessly when a leg dies; credit-window end-to-end flow control; adaptive buffers; BBR per leg |
| **Crypto** | Authenticated handshake (X25519 + static node key), forward secrecy, AES-GCM / ChaCha20-Poly1305 AEAD |
| **Censorship resistance** | Chrome 140 TLS fingerprint (JA3/JA4, post-quantum `key_share`), server-side cover flights, irregular length padding, decoy site for scanners and active probes |
| **Browser profiles** | `profile record` captures a real browser's TLS fingerprint, QUIC Initial and traffic shape, verifies the engine reproduces them, and emits one JSON file |
| **Beyond TCP** | Optional UDP leg (QUIC/WebRTC mimicry), multi-node mesh with onion routes |
| **L3 VPN** | TUN + userspace stack (no TCP-in-TCP), kill-switch, router mode |
| **Platforms** | Linux, OpenWrt, Android (UniFFI bindings) |
| **Docs** | Full protocol spec, threat model with honest limitations, deployment & hardening guide — in Russian and English |

### 📡 [`nrxp-smoltcp`](https://github.com/selbstnull/nrxp-smoltcp) — TCP/IP stack fork · Rust
A fork of [smoltcp](https://github.com/smoltcp-rs/smoltcp), retargeted from bare-metal to a **userspace VPN client**:
- **BBRv1 from scratch** — full Startup → Drain → ProbeBW → ProbeRTT state machine, per-ACK delivery-rate sampling, windowed min/max filters. Debugged against real traffic traces (fixed RTprop filter window, ProbeRTT duration, round counting).
- **Dynamic BDP-sized socket buffers** that resize at runtime from the congestion controller's estimate.
- **Active queue management** for UDP/ICMP/raw sockets — drop stale packets instead of delivering them late.
- **Tunnel-aware RTT API** — feed in the RTT measured one layer down, where it actually matters.
- **`ChannelDevice`** — bounded, backpressure-aware bridge between a tokio TUN loop and smoltcp's synchronous `poll()`.

### 🧭 [`nrxp-browser`](https://github.com/selbstnull/nrxp-browser) + [`nrxp-servo`](https://github.com/selbstnull/nrxp-servo) — experimental browser · Rust *(WIP)*
A browser on the [Servo](https://servo.org) engine where **all** traffic goes through NRXP — no TUN, no root, no local DNS — and the browser doubles as an ingress/egress node (`nrb node init / invite / deploy`).
- Engine embedder patched via a Servo fork (custom `NetworkConnector`); verified with a PID-level leak watcher that nothing bypasses the tunnel; a hermetic mode runs the engine in an empty network namespace.
- Real-world page corpus of 57 sites: **~88 % fully OK in release**, ~100 % open.
- Ad/tracker blocking (EasyList), userscripts, signed updates, downloads, in-engine find, WebRTC relay-only mode, ~110 tests + GUI scenarios in nested Xephyr.
- ⚠️ *Work in progress — the repo ships an honest feature/limitations table. Not for production use yet.*

### 🛠️ The rest of the ecosystem (private)
Backend with auth and subscriptions, payments and donations, landing page and mirror, Android app, a multi-region node fleet, CI/CD on a self-hosted Gitea, a DPI test stand (SNI / JA3 / signature / entropy / active-probe / throttle checks) used to validate the protocol against detection, and a netns-based harness for measuring flow-control and RTT regressions.

---

## 🧰 Other open-source

| Project | What |
|---|---|
| 🎮 [**Cyberhack**](https://github.com/selbstnull/Cyberhack) | The Cyberpunk 2077 *Breach Protocol* minigame as a **React-ready WebAssembly component** — logic in Rust (Yew → wasm), neon UI in Tailwind v4, published to **npm**, with a server-authoritative mode and localisation. |
| 🧠 [**fluxless-core**](https://github.com/selbstnull/fluxless-core) · [**fluxless-react**](https://github.com/selbstnull/fluxless-react) | Minimal TypeScript state manager (`Observer` / `Slice` / `Store`) plus a React binding — a lightweight alternative to Redux-style boilerplate. tsup build, Jest tests, Apache-2.0. |

---

## 💼 Commercial experience

### Fullstack Developer — **FintechIQ**
*"Stratosphere", a high-traffic microfinance platform.*

- ⚡ **+50 % page-load speed** by diagnosing and fixing complex SSR hydration bugs.
- 🔁 **Re-renders 20–30 → 2–3** on heavy forms after upgrading MUI and React Hook Form.
- 🏗️ **Build time 20 → 12 min** by moving the build to Turbopack.
- 📊 Fixed critical client-side analytics bugs so the business got trustworthy user data.
- 🧱 Refactored the legacy UI component library (DRY, easier onboarding for new devs).

---

## 🚀 Tech stack

| | |
|---|---|
| **🎯 Core focus** | 🦀 **Rust** (tokio, UniFFI, wasm) · **network protocols** & transport design · TCP/IP internals, congestion control · TLS/QUIC fingerprints · TUN / netns · AEAD / X25519 |
| **Frontend** | ⚛️ React, Next.js (SSR/SSG/ISR), TypeScript, MUI, Tailwind, SCSS, WebAssembly |
| **Backend** | 🟢 Node.js, Nest.js, Express · ☕ Java, Spring (familiar) · PostgreSQL, TypeORM, Redis · JWT / OAuth2, Supabase |
| **.NET / C#** | 🟣 C# (Unity); .NET ecosystem — worked closely with .NET teams in fintech projects |
| **Mobile** | 🤖 Java / Kotlin (Android); Rust core via UniFFI in the Netrunner Android client |
| **Game dev & 3D** | 🎮 Unity / C#, Rust → WASM games, Blender (Geometry Nodes, procedural shaders) |
| **Infra & DevOps** | 🐳 Docker, Nginx, systemd, Cloudflare, GitHub Actions, GitLab CI, Gitea, NX monorepo, Webpack / Turbopack |

---

## 🎯 What you get if you hire me

- **Full-cycle engineering** — I can take a feature from protocol spec to deployed node to UI.
- **Fluent across stacks** — comfortable working next to .NET, JVM / Spring and mobile teams; I pick up a new stack fast, while Rust and protocols are where I go deepest.
- **Depth when it's needed** — congestion control, flow control, DPI evasion: I read the RFCs, build the test stand, and measure.
- **Performance by measurement, not by feel** — numbers above come from profiling and benchmarks.
- **Honest engineering** — my READMEs list limitations, threat models and what is *not* verified.
- **Documentation habit** — specs, security models and runbooks in two languages.

---

## 🌱 Off the clock

🎨 3D in **Blender** (Geometry Nodes, procedural shaders, arch-viz) · 🎸 guitar & music theory (CAGED) · 🍵 Pu-erh tea · 🎮 games · 🗣️ levelling up my English in an international language programme.

<div align="center">

**Let's talk → [Telegram](https://t.me/ktr2102) · [Email](mailto:trkir21@gmail.com) · [LinkedIn](https://www.linkedin.com/in/kirill-trapeznikov-57b450374/)**

</div>
