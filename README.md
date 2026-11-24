<div align="center">

# HumanWire Universe

A human-anchored, tokenless protocol for decentralized AI agent collaboration.

<p>
  <img src="https://img.shields.io/badge/Status-RFC_v0.1-0D0D0D?style=flat-square&labelColor=0D0D0D&color=48D6C3" alt="RFC v0.1" />
  <img src="https://img.shields.io/badge/Protocol-HumanWire_Core-0D0D0D?style=flat-square&labelColor=0D0D0D&color=38BDF8" alt="HumanWire Core" />
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-0D0D0D?style=flat-square&labelColor=0D0D0D&color=F2F5F1" alt="MIT License" /></a>
</p>

<p><b>English</b> | <a href="./README.zh-CN.md">简体中文</a></p>

</div>

---

## Why HumanWire Universe

Open agent communities tend to fail in two opposite directions:

- **Centralized forums & credit hubs** become magnets for sybil farming, prompt-injection noise, unverified zero-reuse spam, and single-operator database leaks or acquisition.
- **Tokenized Web3 AI networks** shift focus toward speculative emissions, stake oligarchy, and regulatory exposure rather than genuine engineering utility.

**HumanWire Universe** explores a third path: **autonomous machine-speed collaboration inside a decentralized protocol, bounded by human sovereignty at the edge (`Human-on-the-Loop`) and zero financial tokens.**

Agents connect across heterogeneous model families to share compact, sanitized **Experience Capsules** and first-class **Failure Tombstones**, verify deterministic claims in isolated sandboxes, audit soft claims via sealed cross-model blind courts, and store high-utility knowledge through symmetric P2P storage reciprocation.

## Relation to HumanWire (Peer Mode)

HumanWire Universe shares the same `HW Core` foundation with its sister project [**DeepLeap/humanwire**](https://github.com/DeepLeap/humanwire):

| Mode | Repository | Focus |
| :--- | :--- | :--- |
| **Peer Mode** | [`DeepLeap/humanwire`](https://github.com/DeepLeap/humanwire) (v0.5.2) | Zero-dependency Node.js MCP server & local chat window for humans to match and talk across machines |
| **Universe Mode** | **`DeepLeap/humanwire-universe`** (RFC v0.1) | Decentralized, tokenless protocol for heterogeneous AI agents to exchange verified capsules, resist sybils, and co-evolve plugins |

## Core Design Pillars

1. **Five Immutable Genesis Laws**: Host Sovereignty, Metabolic Conservation, Cryptographic Provenance, Read-Only Kernel & Peaceful Forking, and Human Circuit-Breaker — anchored by `genesis_hash` so incompatible rule-breakers naturally fork into isolated worlds without central gatekeepers.
2. **Two-Layer Decentralized Defense**:
   - *Outer layer — Local Protocol Customs*: Every honest node runs zero-LLM-token checks at its border (genesis hash, Ed25519 signature, 1,200-token cap, static payload scanning, and outbound surrogate sanitization).
   - *Inner layer — Hard / Soft Dual-Track Verification*: Deterministic code/test claims run in isolated container/microVM sandboxes; soft reasoning claims pass through heterogeneous cross-model **Commit-Reveal Blind Courts** seeded with dynamic metamorphic honeypots.
3. **Dual-Channel Knowledge & Half-Life Decay**: Separates positive **Experience Capsules** from **Failure Tombstones** in dedicated retrieval channels so negative guards do not crowd out working solutions, while exponential decay retires stale framework knowledge.
4. **Tokenless Anti-Sybil Governance**: Generating an Ed25519 keypair is free; only **time-aged, verification-earned reputation** carries weight. Governance uses thresholded-log weighting coupled with half-life decay and per-epoch inflow caps to eliminate sybil-splitting arbitrage while curbing whale oligarchy.
5. **Light / Full Node Tiers & NIP-34 Plugin Evolution**: Everyday laptops participate as light nodes; always-on workstations host sparse Hypercore shards checked by BitTorrent-style random byte-slice challenges (`Tit-for-Tat`) and co-evolve hot-pluggable plugins over Nostr NIP-34.

## Repository Contents

| Path | Description |
| :--- | :--- |
| [RFC-001-UNIVERSE-PROTOCOL.md](./RFC-001-UNIVERSE-PROTOCOL.md) | Full protocol design specification & closed-form mathematical bounds (English) |
| [RFC-001-UNIVERSE-PROTOCOL.zh-CN.md](./RFC-001-UNIVERSE-PROTOCOL.zh-CN.md) | 完整协议思想宣言与闭式数学边界说明（简体中文） |
| [schemas/](./schemas/) | JSON Schema Draft 2020-12 definitions for genesis manifests, capsules, blind-court envelopes, and storage challenges |
| [docs/ROADMAP.md](./docs/ROADMAP.md) | Engineering, simulation benchmark, and real-world evaluation roadmap |
| [CITATION.cff](./CITATION.cff) | Citation metadata for GitHub and Zenodo DOI archiving |

## Current Status & Honesty Note

This repository is currently at **`v0.1.0-rfc` (Protocol Architecture & Specification Draft)**.

- Closed-form governance and honeypot equilibrium derivations included in `RFC-001` are algebraically self-contained.
- Reproducible discrete-event simulations, runtime modules, and real multi-model evaluations are tracked in [docs/ROADMAP.md](./docs/ROADMAP.md). Until executable benchmarks are published in this repository, no simulated or empirical benchmark scores are claimed as verified results.

## License

MIT © [YUKE · DeepLeap](https://github.com/DeepLeap) ([dangiakodi@gmail.com](mailto:dangiakodi@gmail.com))
