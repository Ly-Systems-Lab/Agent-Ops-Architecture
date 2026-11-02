# Agent-Ops-Architecture

**A top-level design for Distributed Embodied AI — one person, commanding a team of agents, driving countless physical nodes.**

🔗 **The trio** · [🔭 Blueprint (this repo)](https://github.com/Ly-Systems-Lab/Agent-Ops-Architecture) · [🧠 Control plane · WeCom-Bridge](https://github.com/Ly-Systems-Lab/Agent-Ops-WeCom-Bridge) · [🔌 Data plane · Edge-Hub](https://github.com/Ly-Systems-Lab/Agent-Ops-Edge-Hub)

This repository is the **blueprint** of the Agent Ops trio. It contains **documentation only — no code**.
It answers *why the system is shaped this way*, not *how to install it*.

> 📄 The detailed documents are written in Chinese (the primary audience).
> This page is a one-screen English summary.

---

## The trio

| Repository | Plane | In one sentence | Status |
|---|---|---|---|
| **This repo** · [`Agent-Ops-Architecture`](https://github.com/Ly-Systems-Lab/Agent-Ops-Architecture) | **Blueprint** | The top-level design for distributed embodied AI, the manifesto, and the human-agent conventions | Public |
| [`Agent-Ops-WeCom-Bridge`](https://github.com/Ly-Systems-Lab/Agent-Ops-WeCom-Bridge) | **Control plane** · human ⇄ agent | Turns WeCom into an agent cockpit: streaming interaction, progress control, permission interception | Public |
| [`Agent-Ops-Edge-Hub`](https://github.com/Ly-Systems-Lab/Agent-Ops-Edge-Hub) | **Data plane** · agent ⇄ physical world | An MQTT-based edge bus that turns agent intent into real device actions | Public |

The control plane and the data plane are **fully decoupled** — they never call each other, they only share one set of conventions.

```
    human  ──chat──▶  [control plane]  ──▶  Agent  ──▶  [data plane]  ──▶  embodied nodes
```

---

## 🎬 Demos

| What you'll see | Video |
|---|---|
| **Data plane** · One natural-language sentence drives three endpoint devices in sequence — 36 s | [▶ Watch](https://www.bilibili.com/video/BV18Zx86KEDT/) |
| **Control plane** · Send a request from your phone; the result lands on the local Windows machine, with task cards (allow / deny / live progress / context budget) and `/stop` | [▶ Watch](https://www.bilibili.com/video/BV1nY4w6kEiy) |
| **Graceful degradation** · Circuit-breaker probe → direct fallback → self-recovery, with the device never noticing | [▶ Watch](https://www.bilibili.com/video/BV1UDh169E3s) |

🖼️ **Screenshot gallery** · [Multi-device control receipts / local Windows work / edge alerts →](docs/showcase/README.md)

> 🌐 Videos and screenshot captions are in Chinese (the primary audience).

---

## The thesis

**AI is trapped in the digital world.** Model output is tokens, not actions.

There are two common replies, and both fall short:

- *Put the model on the device* → not enough compute, slow to iterate, dead when offline.
- *Expose a cloud API* → the device becomes a networked switch, with no local judgment and no fallback.

Both share the same wrong assumption: **intelligence and body must be the same thing.**

Our answer is to **decouple them**:

| Layer | Responsibility |
|---|---|
| **Cloud** | Model inference, complex planning, capability iteration |
| **Edge** | A concentrated brain (agents) plus an always-on hub (bus, adapters, alarms) |
| **End** | Distributed embodied nodes — panels, robots, sensors — each with a small local judgment of its own |

Three design principles:

1. **Central brain, distributed bodies.** Compute is scarce; devices are cheap.
2. **Narrow channel, hard contract.** Frame caps, idempotent commands, cooldowns, honest timeouts — because physical actions cannot be rolled back.
3. **Graceful degradation, edge autonomy.** When the brain goes down, devices must keep doing what they already know how to do. **Continuity matters more than intelligence.**

The acceptance test is one sentence: **unplug the network — can the device still do the few things it already knows?**

---

## Documents

| Document | What it covers |
|---|---|
| [docs/why.md](docs/why.md) | Why this ecosystem exists — six mismatches of the current era |
| [docs/manifesto.md](docs/manifesto.md) | The Collaboration Manifesto |
| [docs/embodied-ai.md](docs/embodied-ai.md) | Distributed Embodied AI — the top-level design |
| [docs/architecture.md](docs/architecture.md) | Panorama: cloud/edge/end, star topology, role table, known limits |
| [docs/conventions.md](docs/conventions.md) | Five conventions and the interaction baseline |

---

## What is actually built

Stated plainly, so the claim and the evidence are on the same page:

- ✅ Brain/body decoupling — running: panel and robot actions are dispatched over MQTT with two-way receipts.
- ✅ Contracted channel — frame caps, QoS 1 end-to-end, idempotent dedupe, cooldowns, concurrency gate, honest timeouts.
- ✅ Device isolation — mutual TLS, topic-scoped ACLs, one credential pair per device.
- 🟡 Edge autonomy — **partial**: device firmware and local entry points survive without the bus; automatic takeover after brain loss is not implemented.
- ✅ Heterogeneous onboarding — the third device type was added by copying an existing adapter, in half a day.
- ❌ Production-grade resilience — no redundancy, no SLA, no failure drills.

**This is an architecture position, not a production guarantee.**

---

## License

Documentation is licensed under **[CC BY 4.0](LICENSE)** — reuse, quote and adapt freely, **with attribution**.

Attribution: **Agent-Ops-Architecture · Ly-Systems-Lab** (with a link back to this repository).

---

> For everyone brave enough to rebuild their own workflow in the age of AI.