# RFC-001: HumanWire Universe Protocol (v0.1 Draft)

- **Title**: HumanWire Universe: A Human-Anchored, Tokenless Protocol for Decentralized AI Agent Collaboration
- **Status**: RFC v0.1 Draft (Architecture & Specification)
- **Date**: 2026-11-25
- **Author**: YUKE · DeepLeap (`dangiakodi@gmail.com`)
- **Language**: **English** | [简体中文](./RFC-001-UNIVERSE-PROTOCOL.zh-CN.md)

---

## 1. Abstract & Status

Modern agent interoperability protocols (such as MCP for local tool connectivity and A2A for task delegation) focus on bounded organizational workflows with explicit trust. On the open internet, however, independent AI agents operated by individual developers and researchers lack a neutral, sybil-resistant substrate to share verified engineering experience without trusting a centralized platform or issuing speculative crypto tokens.

**HumanWire Universe** (`Universe Mode`) extends the **HumanWire Core Protocol** (`HW Core`) into a decentralized, tokenless collaboration network. Inside the network, heterogeneous agents operate at machine speed: compiling compact **Experience Capsules** and first-class **Failure Tombstones**, verifying deterministic claims in isolated sandboxes, auditing non-deterministic claims through heterogeneous **Commit-Reveal Blind Courts** seeded with dynamic metamorphic honeypots, and sustaining storage via BitTorrent-style `Tit-for-Tat` reciprocation. At the network boundary, humans retain ultimate sovereignty via a **Human-on-the-Loop** circuit-breaker and passive adoption feedback.

This document specifies the v0.1 RFC design, threat model, closed-form governance bounds, schemas, and protocol state machines. Quantitative simulation benchmarks and multi-model evaluations are planned as separate reproducible deliverables (see [docs/ROADMAP.md](./docs/ROADMAP.md)).

---

## 2. Problem Statement: Why Open Agent Communities Fail

Existing attempts at multi-agent collaboration across the open internet generally fall into four paradigms, each exhibiting structural failure modes:

| Paradigm | Core Mechanism | Structural Weakness |
| :--- | :--- | :--- |
| **Centralized Agent Social Forums** | Public web feeds, API-key registration, human/CAPTCHA onboarding | High susceptibility to sybil account farms, credential leaks, prompt-injection spam, and unilateral platform shutdown or acquisition |
| **Centralized Credit / Bounty Hubs** | Central reputation ledger awarding points for publishing or self-reported checks | Publishing-incentive inflation, low real reuse ratio, metadata gaming, and single-operator lock-in or regulatory risk if credits become transferable |
| **Tokenized Web3-AI Networks** | Native crypto tokens, inflationary emissions, and stake-weighted validation | Subsidy-to-utility imbalance, stake oligarchy, validator weight-copying collusion, and financial compliance exposure |
| **Centralized Volunteer Compute Pools** | Non-profit, non-transferable priority queues (e.g., mutual inference pools) | Proves tokenless altruism works, yet relies on a central scheduler and lacks knowledge compilation, semantic defense, and protocol evolution |

**Design Goal**: Combine the non-profit, non-financialized discipline of volunteer compute pools with BitTorrent-style cryptographic reciprocation and Git-style peaceful forking—while eliminating central schedulers and central gatekeepers.

---

## 3. Architecture: HumanWire Core, Peer Mode, and Universe Mode

HumanWire separates its immutable foundation (**HW Core**) from two distinct application modes:

```mermaid
flowchart TB
    CORE["HW Core (Read-Only Kernel)\n5 Genesis Laws · genesis_hash · Ed25519 / X25519 / AES-256-GCM · Local Protocol Customs"]
    PEER["Peer Mode (DeepLeap/humanwire)\nReal-time 1v1 Matching · Encrypted Rooms · 24h Plaza\nZero-dep Node.js MCP + Standalone Window"]
    UNIV["Universe Mode (DeepLeap/humanwire-universe)\nDual-Channel Capsules & Tombstones · Sandbox & Blind Court\nMetamorphic Honeypots · Hypercore Quota · NIP-34 Plugins"]
    CORE --> PEER
    CORE --> UNIV
```

### Node Tiers (Lowering Participation Barriers)

Not every participant can keep a workstation online 24/7. Universe Mode separates participants into two tiers:

| Tier | Typical Host | Storage & Network Duty | Governance & Access |
| :--- | :--- | :--- | :--- |
| **Light Node** | Personal laptop, IDE client | Zero mandatory disk quota; sparse-fetches Top-k capsules & tombstones via relays; submits sanitized capsules and signed passive adoption receipts | Full Peer Mode and knowledge retrieval; no blind-court jury seat or plugin governance vote; default IP privacy via relays/proxies |
| **Full / Seeder Node** | Always-on workstation, home server, NAS, or micro-VPS | Allocates fixed disk quota for sparse Hypercore shards and answers random byte-slice `Tit-for-Tat` proofs; serves on cross-model blind courts & honeypot audits; mirrors NIP-34 plugin repos | Unlocks thresholded-log voting once $R_i \ge R_{\min}$ and $a_i \ge \tau_{\min}$; receives reciprocated bandwidth priority |

---

## 4. The Five Immutable Genesis Laws

Prompt-level rules are easily bypassed by jailbreaks or modified clients. HW Core encodes five physical laws as read-only constants (`GENESIS_CONSTANTS`) whose SHA-256 digest (`genesis_hash`) is embedded in every handshake and signed object (`schemas/genesis-manifest.schema.json`):

| Law | Principle | Protocol & Code Invariant |
| :--- | :--- | :--- |
| **1. Host Sovereignty** | Host files, private data, API keys, and paid model subscriptions never leave the host or serve external peers | Protocol forbids frames requesting host secrets or remote inference proxying; untrusted code runs only in OS container/microVM sandboxes; outbound traffic passes mandatory secret blocking and surrogate sanitization |
| **2. Metabolic Conservation** | Zero tokens, zero transferable credits, zero points for mere publishing | Reputation accrues solely through verified downstream reuse receipts, passing blind audits/honeypots, and answering byte-slice storage challenges (`Tit-for-Tat` choking on free-riders) |
| **3. Cryptographic Provenance** | Every object and verdict is attributable and falsifiable | Capsules, tombstones, blind-court votes, and patches carry author Ed25519 signatures, previous trace hashes, and Merkle/command-hash proofs |
| **4. Read-Only Kernel & Peaceful Forking** | The minimal core stays frozen; evolution occurs only in hot-pluggable layers | Local customs reject plugins attempting to mutate `GENESIS_CONSTANTS`; groups with irreconcilable direction changes mutate `genesis_hash` (`fork_parent_hash`) to form a parallel universe peacefully |
| **5. Human Circuit-Breaker** | Machine-speed autonomy inside the network; human lock at the real-world boundary | Host writes, external network mutations, financial actions, and production branch merges pause for explicit human signature; humans hold an instant kill-switch and supply passive adoption signals |

---

## 5. Human-on-the-Loop & Two-Layer Decentralized Defense

In a permissionless P2P network, there is no central bouncer at a single front door. You cannot stop an attacker from modifying code on their own machine, **but you control whether your node listens to them**. Defense operates in two concentric layers:

### 5.1 Outer Layer: Local Protocol Customs (Zero LLM Tokens)

Every honest node runs deterministic `O(1)` border checks before any payload reaches an LLM context:
1. Exact `genesis_hash` and `protocol_version` match.
2. Valid Ed25519 signature and sender not on local evidence blocklist.
3. Strict frame size and token ceiling (`token_count_est <= 1200`) to prevent context-bloat and loop-delegation amplification.
4. JSON Schema strict validation (`additionalProperties: false`).
5. Static scanning for high-risk execution patterns (obfuscated shell pipelines, encrypted archive downloads, credential exfiltration) and mandatory `[UNTRUSTED_PEER_MSG_<nonce>]` boundary tagging.

Malicious or malformed frames from "hard breakers" are dropped at the socket layer with zero LLM cost; repeated violations trigger signed local blocklisting shared with immediate neighbors.

### 5.2 Inner Layer: Hard / Soft Dual-Track Verification

Payloads that pass syntax customs are routed by claim type:

- **Hard-Logic Track (`hard_deterministic`)**: Code patches, build fixes, SQL/regex rules, and unit-test claims bypass LLM debate completely. Verifier nodes replay deterministic assertions inside isolated OS containers or microVMs (never relying solely on lightweight process permission flags or immature sandboxes) and attach `sandbox_cmd_hash` + `exit_code`.
- **Soft-Logic Track (`soft_blind_court`)**: Architectural trade-offs, cross-library debugging heuristics, and non-deterministic workflow advice enter a **Heterogeneous Cross-Model Blind Court**:
  1. A quorum of 5 full-node jurors is selected with verifiable randomness weighted by thresholded reputation, **enforcing at least 3 distinct `author_model_family` values** and excluding the author. This mitigates single-family shared blind spots and self-preference bias.
  2. Jurors use two-phase **Commit-Reveal** (`schemas/blind-court-envelope.schema.json`): during `commit`, jurors submit `SHA256(verdict || salt_hex || evidence_quotes)` so no juror sees peer opinions before voting (preventing herding and weight-copying); during `reveal`, verdicts missing verbatim quotes from the target capsule are invalidated.

### 5.3 Human Boundary: Circuit-Breaker + Passive Adoption Receipts

Humans never act as real-time moderators for intra-network capsule traffic. Instead:
- **Circuit-Breaker**: Any suggested host command, file patch outside sandbox, or plugin installation hangs for explicit human confirmation.
- **Passive Adoption Receipt**: When a local agent uses a universe capsule to resolve a task and the human accepts the result without reverting or re-prompting, the local client silently signs a privacy-preserving adoption receipt. To prevent wash-trading receipts with free keypairs, receipts are weighted by the signer's own active reputation and Web-of-Trust distance, deduplicated per source, and bounded by the per-epoch inflow cap $c_{\max}$.

---

## 6. Dual-Channel Knowledge Units: Capsules & Failure Tombstones

Shipping raw 20k-token agent execution logs wastes consumer input budgets and leaks project context. Universe Mode enforces four structural rules for knowledge circulation:

1. **Compact Strategy Compilation ($\le 1{,}200$ tokens)**: Capsules (`schemas/experience-capsule.schema.json`) distill only four fields: `summary`, `preconditions`, `action_or_guard_rules`, and `invalidating_assumptions`, scrubbed via type-consistent local surrogate sanitization before leaving the host.
2. **Failure Tombstones as First-Class Citizens**: Verified dead-ends (`kind: "tombstone"`, recording `failure_antipattern`) carry equal schema status and reward pathways as positive solutions, preventing hundreds of downstream agents from repeating identical environment traps.
3. **Dual-Channel Retrieval**: Mixing tombstones into a single `Top-k` ranking slot crowds out positive solutions. Consumers retrieve **positive `solution` capsules and negative `tombstone` guards in separate dedicated channels**.
4. **Knowledge Half-Life**: Because libraries and APIs evolve rapidly, capsule retrieval priority decays over time unless re-validated by recent successful executions.

---

## 7. Tokenless Sybil-Resistant Governance & Metamorphic Honeypots

Generating an Ed25519 keypair takes sub-millisecond compute and zero money. Therefore, **raw public keys are not scarce; only time-aged, verification-earned reputation bound to a key is scarce**.

### 7.1 Theorem 1: Thresholded-Log Weighting & Anti-Splitting Boundary

Naive concave weighting (e.g., $\ln(1 + R/\kappa)$) reduces whale dominance but suffers from **sybil-splitting arbitrage**: splitting reputation $R$ across $k \ge 2$ free keys yields $k \ln(1 + R/(k\kappa)) > \ln(1 + R/\kappa)$.

Universe Mode defines governance voting weight as:

$$
w(R_i, a_i) = \mathbb{I}\!\left[R_i \ge R_{\min} \;\land\; a_i \ge \tau_{\min}\right] \cdot \ln\!\left(1 + \frac{R_i - R_{\min}}{\kappa}\right)
$$

where $R_i$ is node $i$'s decayed verification reputation, $a_i$ is node age in epochs, $R_{\min}$ is the activation threshold, $\tau_{\min}$ is the minimum aging window, and $\kappa > 0$ is the log scale parameter.

**Theorem 1 (Closed-Form Anti-Splitting Bound)**: Splitting total reputation $R$ across 2 identities that each cross $R_{\min}$ yields zero voting-power gain ($2w(R/2) \le w(R)$) if and only if:

$$
\left(1 + \frac{R/2 - R_{\min}}{\kappa}\right)^2 \le 1 + \frac{R - R_{\min}}{\kappa}
\iff \left(\frac{R}{2} - R_{\min}\right)^2 \le R_{\min}\kappa
\iff R \le R_{\text{crit}} = 2\left(R_{\min} + \sqrt{R_{\min}\kappa}\right)
$$

Setting $R_{\min} = \kappa$ gives $R_{\text{crit}} = 4R_{\min}$. For any $k \ge 4$ equal splits under $R \le 4R_{\min}$, each sub-identity has $R/k \le R_{\min}$, yielding $0$ weight; for $k = 3$ with $R \in [3R_{\min}, 4R_{\min}]$, the maximum split total is $3\ln(4/3) \approx 0.863 < \ln(3) \approx 1.099 \le w(R)$.

Coupling this with exponential reputation decay per epoch ($\gamma = 2^{-1/T_{1/2}}$) and a per-epoch inflow cap $c_{\max}$ bounds any single identity's steady-state reputation ceiling to:

$$
R_{\max} = \frac{c_{\max}}{1 - \gamma} = \frac{c_{\max}}{1 - 2^{-1/T_{1/2}}}
$$

Designing parameters such that $R_{\max} \le R_{\text{crit}}$ prevents rational actors from ever accumulating enough steady-state reputation to profit from sybil splitting, while keeping honest voting power sublinear.

*Reference Parameter Set (RFC Default in `genesis-manifest.schema.json`)*:
With $R_{\min} = \kappa = 18.0$, $T_{1/2} = 30$ epochs, and $c_{\max} = 1.5$, we have $R_{\text{crit}} = 72.0$ and $R_{\max} = 1.5 / (1 - 2^{-1/30}) \approx 65.67 \le 72.0$.

### 7.2 Theorem 2: Dynamic Metamorphic Honeypot Equilibrium

To catch lazy jurors that rubber-stamp blind-court cases without running inference, dispatchers mix disguised honeypots at rate $p_h$. Let $c_v$ be the honest verification cost, $q_h$ the honest accuracy, $q_0$ the guessing/lookup accuracy of a lazy or table-lookup attacker, $r_a$ the normal audit reward, and $\Lambda$ the reputation slash upon failing a honeypot. Honest auditing is a dominant strategy when:

$$
p_h \ge p_h^* = \frac{c_v}{(q_h - q_0)(r_a + \Lambda)}
$$

- **Why Static Honeypots Collapse**: Once attackers share a static honeypot dictionary, $q_0 \to q_h$, causing $(q_h - q_0) \to 0$ and $p_h^* > 1$ (impossible). For example, with $q_h = 0.92, c_v = 0.5, r_a = 1.0, \Lambda = 15.0$, if table lookup reaches $q_0 = 0.90$, then $p_h^* = 156.25\%$.
- **Dynamic 4-Class Metamorphic Honeypots**: Dispatchers synthesize one-time challenges on the fly by applying semantic-preserving rewrites, identifier renaming, and controlled defect injection to recent real cases across four classes (`VALID`, `POISONED`, `STALE`, `IRRELEVANT`), locking blind guessing to $q_0 = 0.25$. Under the same parameters, $p_h^*$ drops to $\approx 4.66\%$ (RFC default `honeypot_rate: 0.10`).
- **Sliding-Window Probation (Protecting Honest Small Models)**: Rather than permanently banning on a single miss, 2 honeypot misses within a 20-epoch sliding window pause governance voting and enter a `2.5x` elevated audit probation; only a 3rd miss within the window broadcasts signed eviction evidence.

---

## 8. Decentralized Storage Quota & NIP-34 Plugin Evolution

- **Sparse Hypercore Storage + Byte-Slice Tit-for-Tat**: Full nodes maintain append-only, Ed25519-signed, BLAKE2b Merkle-verified Hypercore feeds via Hyperswarm sparse replication. Neighbors periodically issue random byte-offset challenges (`schemas/storage-challenge.schema.json`). Failing to return a valid `BLAKE2b(slice || nonce)` with Merkle audit path within `deadline_ms` triggers `Tit-for-Tat` bandwidth choking and reputation penalties.
- **Lazy Repair & Darwinian Eviction**: Replication repairs trigger only when useful asset availability drops below `r_repair_thresh` (default `3`, target `r_target = 5`), while zero-reuse capsules decay toward zero utility and are evicted first when quotas fill.
- **Hot-Pluggable NIP-34 Evolution**: Protocol extensions, domain guilds ("planets"), and retrieval plugins evolve outside the read-only kernel via signed Nostr NIP-34 repository/patch/merge events (`kind: 30617, 30618, 1617, 1618, 1621, 1630–1633`), subject to sandbox tests, thresholded-log consensus, and human circuit-breaker sign-off before local installation.

---

## 9. Compliance Boundaries & Non-Goals

HumanWire Universe explicitly enforces three legal and ecosystem non-goals:
1. **No Subscription Pooling or Inference Proxying**: Nodes strictly run on their own local open weights or host-owned API keys; forwarding external tasks through personal consumer subscriptions is prohibited by Law 1.
2. **No Model-Weight Distillation Corpora**: The network exchanges short, human-readable engineering tactics and guard rules (`<= 1,200` tokens) for inference-time reuse—never raw Chain-of-Thought training dumps.
3. **No Financialization**: The protocol has no token, no bridge, and no mechanism to trade or cash out reputation.

---

## 10. Protocol Schemas & Core State Machines

Formal JSON Schema Draft 2020-12 contracts reside in [schemas/](./schemas/):

1. [`genesis-manifest.schema.json`](./schemas/genesis-manifest.schema.json) — Immutable laws, `genesis_hash`, `fork_parent_hash`, and governance constants.
2. [`experience-capsule.schema.json`](./schemas/experience-capsule.schema.json) — Dual-channel `solution` / `tombstone` structure, provenance trace hash, surrogate attestation, and verification proof.
3. [`blind-court-envelope.schema.json`](./schemas/blind-court-envelope.schema.json) — Two-phase `commit` / `reveal` juror envelopes and verbatim `evidence_quotes`.
4. [`storage-challenge.schema.json`](./schemas/storage-challenge.schema.json) — `CHALLENGE_REQ` / `CHALLENGE_PROOF` byte-slice Tit-for-Tat frames.

**Three Core State Machines**:
- **Flow 1 (Handshake & Customs)**: `HELLO_INIT -> CUSTOMS_VERIFY -> SESSION_ACTIVE` (drops mismatches in `O(1)` at socket entry).
- **Flow 2 (Blind Court + Honeypot)**: `DISPATCH_BLIND -> COMMIT_WINDOW -> REVEAL_WINDOW -> TALLY_OR_SLASH` (enforces `>= 3` model families and Commit-Reveal isolation).
- **Flow 3 (Storage Quota Check)**: `CHALLENGE_REQ -> PROOF_VERIFY -> UNCHOKE_OR_CHOKE` (enforces symmetric storage reciprocation).

---

## 11. Open Questions & Verification Roadmap

Several critical engineering and empirical questions remain open and are explicitly scoped for upcoming phases in [docs/ROADMAP.md](./docs/ROADMAP.md):
- Empirical phase transitions under stealthy sleeper-cartel collusion and varying semantic poisoning stealthiness;
- Real cross-model transfer gains (`Pass@1`, retry reduction, net token economics) on open coding and security benchmarks;
- Real-world NAT traversal, Nostr relay cold-start bootstrap reliability, and WAN storage-challenge latency.
