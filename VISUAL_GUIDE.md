# Block-LoRA: Visual Guide

## System Overview Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                         BLOCK-LORA SYSTEM                            │
│                                                                      │
│  Privacy-Preserving + Efficient + Secure + Transparent + Decentralized │
└─────────────────────────────────────────────────────────────────────┘

┌──────────────────────┐
│   PROBLEM SPACE      │
└──────────────────────┘

Organizations want to collaborate on AI but:
❌ Can't share data (privacy laws)
❌ Don't trust central server
❌ Need audit trail
❌ Face malicious participants

┌──────────────────────┐
│   SOLUTION SPACE     │
└──────────────────────┘

Block-LoRA combines:
✅ Federated Learning (privacy)
✅ LoRA (efficiency)
✅ Blockchain (trust)
✅ Proof-of-Validation (security)
```

---

## Component Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        LAYER ARCHITECTURE                        │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  APPLICATION LAYER                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   Clients    │  │  Validators  │  │  Aggregator  │         │
│  │              │  │              │  │              │         │
│  │ • Train      │  │ • Evaluate   │  │ • Combine    │         │
│  │ • Submit     │  │ • Vote       │  │ • Publish    │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  COORDINATION LAYER (BLOCKCHAIN)                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Smart Contract: BlockLoRA.sol                            │  │
│  │  • Round management                                       │  │
│  │  • Update registry                                        │  │
│  │  • Voting mechanism                                       │  │
│  │  • Trust scores                                           │  │
│  │  • Acceptance logic                                       │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  STORAGE LAYER (IPFS)                                            │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Decentralized File Storage                               │  │
│  │  • LoRA adapters                                          │  │
│  │  • Global models                                          │  │
│  │  • Content-addressed (CID)                                │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  ML LAYER                                                        │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Base LLM + LoRA Adapters                                 │  │
│  │  • GPT-2 / LLaMA / Mistral                                │  │
│  │  • Frozen base weights                                    │  │
│  │  • Trainable adapters                                     │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    FEDERATED LEARNING ROUND                      │
└─────────────────────────────────────────────────────────────────┘

PHASE 1: INITIALIZATION
═══════════════════════
Coordinator
    │
    ├─► startRound() ──► Blockchain
    │                        │
    │                        └─► Event: RoundStarted
    │
    └─► selectValidators() ──► Blockchain
                                   │
                                   └─► Event: ValidatorsSelected

PHASE 2: LOCAL TRAINING (OFF-CHAIN)
════════════════════════════════════
Client 1                Client 2                Client 3
    │                       │                       │
    ├─► Download Global     ├─► Download Global     ├─► Download Global
    │   Model from IPFS     │   Model from IPFS     │   Model from IPFS
    │                       │                       │
    ├─► Train on Private    ├─► Train on Private    ├─► Train on Private
    │   Data (Hospital)     │   Data (Bank)         │   Data (Research)
    │                       │                       │
    └─► Generate LoRA       └─► Generate LoRA       └─► Generate LoRA
        Adapter                 Adapter                 Adapter

PHASE 3: SUBMISSION (ON-CHAIN)
═══════════════════════════════
Client 1                Client 2                Client 3
    │                       │                       │
    ├─► Upload to IPFS      ├─► Upload to IPFS      ├─► Upload to IPFS
    │   → CID₁              │   → CID₂              │   → CID₃
    │                       │                       │
    ├─► Calculate Hash      ├─► Calculate Hash      ├─► Calculate Hash
    │   → Hash₁             │   → Hash₂             │   → Hash₃
    │                       │                       │
    └─► submitUpdate()      └─► submitUpdate()      └─► submitUpdate()
        ├─► Blockchain          ├─► Blockchain          ├─► Blockchain
        └─► Event: Update       └─► Event: Update       └─► Event: Update
            Submitted               Submitted               Submitted

PHASE 4: VALIDATION (HYBRID)
═════════════════════════════
Validator 1             Validator 2             Validator 3
    │                       │                       │
    ├─► Read Updates        ├─► Read Updates        ├─► Read Updates
    │   from Blockchain     │   from Blockchain     │   from Blockchain
    │                       │                       │
    ├─► Download from       ├─► Download from       ├─► Download from
    │   IPFS (CID₁,₂,₃)     │   IPFS (CID₁,₂,₃)     │   IPFS (CID₁,₂,₃)
    │                       │                       │
    ├─► Verify Hashes       ├─► Verify Hashes       ├─► Verify Hashes
    │   ✓ Match             │   ✓ Match             │   ✓ Match
    │                       │                       │
    ├─► Evaluate            ├─► Evaluate            ├─► Evaluate
    │   • Accuracy          │   • Accuracy          │   • Accuracy
    │   • Divergence        │   • Divergence        │   • Divergence
    │   • Backdoors         │   • Backdoors         │   • Backdoors
    │                       │                       │
    └─► submitVote()        └─► submitVote()        └─► submitVote()
        ├─► Accept/Reject       ├─► Accept/Reject       ├─► Accept/Reject
        └─► Blockchain          └─► Blockchain          └─► Blockchain

PHASE 5: FINALIZATION (ON-CHAIN)
═════════════════════════════════
Smart Contract
    │
    ├─► Count Votes
    │   Update 1: 3 Accept, 0 Reject → ACCEPTED ✓
    │   Update 2: 3 Accept, 0 Reject → ACCEPTED ✓
    │   Update 3: 0 Accept, 3 Reject → REJECTED ✗
    │
    ├─► Update Trust Scores
    │   Client 1: +50 (now 550)
    │   Client 2: +50 (now 550)
    │   Client 3: -100 (now 400)
    │
    └─► Emit Events
        • UpdateFinalized (×3)
        • TrustScoreUpdated (×3)

PHASE 6: AGGREGATION (OFF-CHAIN)
═════════════════════════════════
Aggregator
    │
    ├─► Query Blockchain
    │   getAcceptedUpdates(round) → [Update 1, Update 2]
    │
    ├─► Download Accepted
    │   IPFS(CID₁) → Adapter₁
    │   IPFS(CID₂) → Adapter₂
    │
    ├─► Get Trust Scores
    │   Client 1: 550/1000 = 0.55
    │   Client 2: 550/1000 = 0.55
    │
    ├─► Weighted Average
    │   θ_global = (0.55×θ₁ + 0.55×θ₂) / (0.55 + 0.55)
    │            = 0.5×θ₁ + 0.5×θ₂
    │
    └─► Publish New Global Model
        → IPFS → Ready for Round N+1
```

---

## Attack Detection Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    MALICIOUS UPDATE DETECTION                    │
└─────────────────────────────────────────────────────────────────┘

Malicious Client
    │
    ├─► ATTACK TYPE 1: Data Poisoning
    │   └─► Train on corrupted data
    │       └─► Submit update
    │
    ├─► ATTACK TYPE 2: Model Poisoning
    │   └─► Reverse gradients
    │       └─► Submit update
    │
    └─► ATTACK TYPE 3: Backdoor
        └─► Embed trigger
            └─► Submit update

                    │
                    ▼
            ┌───────────────┐
            │  Blockchain   │
            │  Stores CID   │
            └───────┬───────┘
                    │
                    ▼
            ┌───────────────┐
            │  Validators   │
            │  Download     │
            └───────┬───────┘
                    │
                    ▼
        ┌───────────────────────┐
        │  VALIDATION PIPELINE  │
        └───────────────────────┘

CHECK 1: Accuracy
─────────────────
Evaluate on clean validation set
    │
    ├─► Accuracy ≥ 70%? ──► ✓ Pass
    │
    └─► Accuracy < 70%? ──► ✗ REJECT
        Reason: "Low accuracy"
        Effect: Trust -100

CHECK 2: Divergence
───────────────────
Compare to baseline
    │
    ├─► Divergence ≤ 50%? ──► ✓ Pass
    │
    └─► Divergence > 50%? ──► ✗ REJECT
        Reason: "High divergence"
        Effect: Trust -100

CHECK 3: Backdoor
─────────────────
Test known triggers
    │
    ├─► No suspicious output? ──► ✓ Pass
    │
    └─► Trigger detected? ──► ✗ REJECT
        Reason: "Backdoor detected"
        Effect: Trust -100

                    │
                    ▼
        ┌───────────────────────┐
        │  VOTING               │
        │  submitVote()         │
        └───────┬───────────────┘
                │
                ▼
        ┌───────────────────────┐
        │  Smart Contract       │
        │  Counts Votes         │
        └───────┬───────────────┘
                │
                ├─► ≥51% Accept ──► ACCEPTED ✓
                │                   Trust +50
                │
                └─► <51% Accept ──► REJECTED ✗
                                    Trust -100
```

---

## Trust Score Evolution

```
┌─────────────────────────────────────────────────────────────────┐
│                    TRUST SCORE DYNAMICS                          │
└─────────────────────────────────────────────────────────────────┘

HONEST CLIENT TRAJECTORY
════════════════════════

Round 0:  500 ████████████████████████████████████████████████ (Initial)
          │
          ├─► Submit honest update
          │
Round 1:  550 ███████████████████████████████████████████████████ (+50)
          │
          ├─► Submit honest update
          │
Round 2:  600 ██████████████████████████████████████████████████████ (+50)
          │
          ├─► Submit honest update
          │
Round 3:  650 █████████████████████████████████████████████████████████ (+50)
          │
          ...
          │
Round 10: 1000 ████████████████████████████████████████████████████████████████ (MAX)


MALICIOUS CLIENT TRAJECTORY
════════════════════════════

Round 0:  500 ████████████████████████████████████████████████ (Initial)
          │
          ├─► Submit poisoned update
          │
Round 1:  400 ████████████████████████████████████████ (-100)
          │
          ├─► Submit poisoned update
          │
Round 2:  300 ████████████████████████████████ (-100)
          │
          ├─► Submit poisoned update
          │
Round 3:  200 ████████████████████████ (-100)
          │
          ├─► Submit poisoned update
          │
Round 4:  100 ████████████ (-100)
          │
          ├─► Submit poisoned update
          │
Round 5:  0   ░ (-100, floored at 0)


MIXED BEHAVIOR (50% honest)
════════════════════════════

Round 0:  500 ████████████████████████████████████████████████
Round 1:  550 ███████████████████████████████████████████████████ (Accept)
Round 2:  450 █████████████████████████████████████████████ (Reject)
Round 3:  500 ████████████████████████████████████████████████ (Accept)
Round 4:  400 ████████████████████████████████████████ (Reject)
Round 5:  450 █████████████████████████████████████████████ (Accept)
          ...
          Converges to ~450 (equilibrium)
```

---

## Aggregation Weight Impact

```
┌─────────────────────────────────────────────────────────────────┐
│                    AGGREGATION WEIGHTING                         │
└─────────────────────────────────────────────────────────────────┘

SCENARIO: 3 clients submit updates

Client A: Trust = 1000 (High reputation)
Client B: Trust = 500  (Medium reputation)
Client C: Trust = 100  (Low reputation)

WEIGHT CALCULATION
══════════════════

Total Trust = 1000 + 500 + 100 = 1600

Weight_A = 1000 / 1600 = 0.625 (62.5%)
Weight_B = 500  / 1600 = 0.313 (31.3%)
Weight_C = 100  / 1600 = 0.063 (6.2%)

VISUAL REPRESENTATION
═════════════════════

Client A: ████████████████████████████████████████████████████████████████ 62.5%
Client B: ███████████████████████████████████ 31.3%
Client C: ██████ 6.2%

AGGREGATION FORMULA
═══════════════════

θ_global = 0.625 × θ_A + 0.313 × θ_B + 0.063 × θ_C

KEY INSIGHT: High-trust clients dominate the global model!
```

---

## Security Properties

```
┌─────────────────────────────────────────────────────────────────┐
│                    SECURITY GUARANTEES                           │
└─────────────────────────────────────────────────────────────────┘

PROPERTY 1: Privacy
═══════════════════
✓ Raw data never leaves client
✓ Only LoRA adapters shared
✓ Adapters don't reveal raw data
⚠ For stronger privacy: Add Differential Privacy

PROPERTY 2: Integrity
═════════════════════
✓ Validators independently evaluate
✓ Majority vote required (51%)
✓ Malicious updates rejected
✓ Only accepted updates aggregated

PROPERTY 3: Transparency
════════════════════════
✓ All submissions on blockchain
✓ All votes on blockchain
✓ All decisions on blockchain
✓ Immutable audit trail

PROPERTY 4: Byzantine Fault Tolerance
══════════════════════════════════════
✓ Tolerates f < n/3 malicious validators
✓ Example: 9 validators, 3 malicious
  - Honest: 6 votes (67%)
  - Malicious: 3 votes (33%)
  - Honest majority: 67% > 51% ✓

PROPERTY 5: Accountability
══════════════════════════
✓ Trust scores track reputation
✓ Malicious behavior penalized
✓ Honest behavior rewarded
✓ Low-trust clients have low influence

PROPERTY 6: Decentralization
════════════════════════════
✓ No single point of failure
✓ No central authority
✓ Peer-to-peer validation
✓ Blockchain consensus
```

---

## Performance Characteristics

```
┌─────────────────────────────────────────────────────────────────┐
│                    PERFORMANCE METRICS                           │
└─────────────────────────────────────────────────────────────────┘

ROUND LATENCY BREAKDOWN
═══════════════════════

Phase                   Time        Percentage
─────────────────────────────────────────────
Local Training          30 min      60% ████████████████████████████████
Validation              15 min      30% ████████████████
IPFS Upload/Download    3 min       6%  ███
Blockchain Txs          2 min       4%  ██
─────────────────────────────────────────────
TOTAL                   50 min      100%

SCALABILITY
═══════════

Clients:        Unlimited (blockchain scales horizontally)
Validators:     Recommended 7-15 (balance security vs cost)
Updates/Round:  Limited by validator bandwidth
Rounds:         Unlimited (blockchain supports infinite rounds)

DATA TRANSFER
═════════════

Component           Size        Per Round (100 clients)
────────────────────────────────────────────────────────
Full Model          14 GB       1.4 TB ❌ (impractical)
LoRA Adapter        10 MB       1 GB ✓ (feasible)
Blockchain Metadata 1 KB        100 KB ✓ (minimal)

COST ANALYSIS (Ethereum Mainnet)
═════════════════════════════════

Operation           Gas         Cost @ 50 gwei
──────────────────────────────────────────────
Deploy Contract     2,500,000   $5.00
Start Round         50,000      $0.10
Submit Update       150,000     $0.30
Submit Vote         100,000     $0.20
Finalize Round      200,000     $0.40

Per Round (10 clients, 3 validators):
- Submissions: 10 × $0.30 = $3.00
- Votes: 30 × $0.20 = $6.00
- Finalization: $0.40
- TOTAL: ~$10/round

💡 Use Layer 2 (Polygon) for 100× cheaper gas!
```

---

## Comparison with Alternatives

```
┌─────────────────────────────────────────────────────────────────┐
│                    SYSTEM COMPARISON                             │
└─────────────────────────────────────────────────────────────────┘

Feature                 Block-LoRA  FedAvg  Centralized  Blockchain-Only
─────────────────────────────────────────────────────────────────────────
Privacy                 ✓✓✓         ✓✓✓     ✗            ✓✓✓
Efficiency (LoRA)       ✓✓✓         ✗       ✓✓✓          ✗
Transparency            ✓✓✓         ✗       ✗            ✓✓✓
Attack Detection        ✓✓✓         ✗       ✓            ✗
Trust System            ✓✓✓         ✗       ✗            ✓✓
Decentralization        ✓✓✓         ✓       ✗            ✓✓✓
Audit Trail             ✓✓✓         ✗       ✓            ✓✓✓
Scalability             ✓✓          ✓✓✓     ✓✓✓          ✗
Cost                    ✓✓          ✓✓✓     ✓✓           ✗

Legend: ✓✓✓ Excellent  ✓✓ Good  ✓ Fair  ✗ Poor
```

---

## Future Enhancements Roadmap

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVELOPMENT ROADMAP                           │
└─────────────────────────────────────────────────────────────────┘

PHASE 1: Current (v1.0) ✓
═════════════════════════
✓ Basic federated learning
✓ LoRA integration
✓ Blockchain coordination
✓ Proof-of-Validation
✓ Attack detection
✓ Trust system

PHASE 2: Enhanced Security (v1.5)
══════════════════════════════════
□ Differential Privacy (DP-SGD)
□ Homomorphic Encryption
□ Zero-Knowledge Proofs
□ Advanced backdoor detection
□ Formal verification

PHASE 3: Scalability (v2.0)
═══════════════════════════
□ Layer 2 deployment (Polygon, Arbitrum)
□ Sharding for parallel validation
□ Optimistic rollups
□ Cross-chain bridges

PHASE 4: Economics (v2.5)
═════════════════════════
□ Staking mechanism
□ Reward distribution
□ Slashing for malicious behavior
□ Token economics
□ Governance (DAO)

PHASE 5: Production (v3.0)
══════════════════════════
□ Web dashboard
□ Automated monitoring
□ Alert system
□ Performance optimization
□ Security audit
□ Mainnet deployment
```

---

This visual guide provides intuitive diagrams for understanding Block-LoRA's architecture, workflow, and security properties!
