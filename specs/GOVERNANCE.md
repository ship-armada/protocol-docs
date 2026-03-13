# Governance

⚠️ Subject to change! Let's discuss ⚠️

## Model

**Proposals fail by default unless they achieve majority quorum.**

Anyone can propose. Only proposals with genuine support pass.

## Parameters ⚠️ TBD ⚠️

| Parameter | Value | Notes |
|-----------|-------|-------|
| Proposal threshold | 0.1% of total supply | Low barrier; spam controlled by quorum |
| Quorum | 20% of circulating voting power at snapshot | See Voting Power section |
| Majority | >50% of votes cast | Simple majority |
| Proposal delay | 2 days | Review period before voting |
| Voting period | 5 days | Active voting duration |
| Execution delay | 2 days | Timelock after passing |

### Extended Parameters

For high-impact proposals (core parameters, >5% treasury allocation):

| Parameter | Value |
|-----------|-------|
| Quorum | 30% of circulating voting power at snapshot |
| Voting period | 7 days |
| Execution delay | 4 days |

---

## Voting Power

### How it works

ARM tokens vote directly — no locking required. Voting power equals your token balance at a fixed snapshot block, regardless of what you do with your tokens afterward.

**Token balances are checkpointed continuously.** Every time ARM moves — through a transfer, a claim, or a delegation change — the token contract records a new checkpoint for that address. When a proposal is created, the snapshot is fixed at `block.number - 1`. All voting power calculations for that proposal use the checkpoints at that block, permanently. Nothing that happens after the snapshot affects that vote.

This means:
- You can vote and then sell. Your voting power was already fixed.
- You cannot buy ARM after a proposal is created and use it on that vote.
- Flash loan attacks are structurally impossible — borrowed tokens have no checkpoint history.

### Delegation

**Voting power is inactive until delegated.** To participate in governance — either by voting directly or contributing your weight to a delegate — you must assign a delegatee. This is a one-time transaction that can be changed at any time.

| Option | Who votes | When to use |
|--------|-----------|-------------|
| Self-delegate | You vote directly | Active governance participants |
| Delegate to another address | Your delegate votes on your behalf | Passive holders who trust a representative |
| Delegate to abstain address | Tokens count toward quorum; no votes cast | Holders who want their weight in the denominator without participating |

Delegation is free to change between proposals. Redelegating costs one transaction and takes effect immediately — it applies to proposals created after the next block.

**One level of delegation only.** A delegate cannot redelegate to a third party. Voting power terminates at the delegatee.

### Claim-time delegation requirement

**ARM tokens cannot be claimed without simultaneously designating a delegatee.** The claim transaction requires a `delegatee` parameter. Self-delegation is valid; it is still an explicit choice.

This ensures every token in circulation is immediately active in the governance denominator. There is no inert circulating supply.

**If you plan to receive delegation from others:** call `delegate(yourAddress)` before those delegators claim. This establishes your checkpoint history. A delegate without prior checkpoint history can still receive delegation but may have edge cases in historical voting power lookups.

### Quorum denominator

Quorum is measured as a percentage of **circulating voting power at the snapshot block** — not total supply, not all non-treasury tokens.

**Circulating voting power includes:**
- All claimed crowdfund tokens (Round 1 and Round 2)
- Team and airdrop tokens that have cleared their revenue milestone and been claimed
- Any ARM distributed from treasury that has been claimed

**Not included:**
- Treasury ARM (never votes)
- Revenue-locked team/airdrop tokens not yet unlocked
- Allocated-but-unclaimed crowdfund tokens (vote-inert until claimed)
- Unexercised treasury claims (vote-inert until exercised and claimed)

Both the numerator (votes cast) and the denominator (circulating voting power) are snapshotted at proposal creation. A batch of revenue-milestone unlocks mid-vote cannot move the goalposts.

### Example: How a vote is counted

Suppose at snapshot:
- 2,400,000 ARM is circulating and delegated (both crowdfund rounds, fully claimed)
- Quorum threshold: 20% = 480,000 ARM

A proposal receives:
- 520,000 FOR
- 180,000 AGAINST
- 100,000 ABSTAIN

Result: **Passed.** Quorum is 800,000 votes cast (520k + 180k + 100k) — well above 480,000. Majority is FOR (520k > 180k). ABSTAIN counts toward quorum but not majority.

Note: a coalition could push a proposal to quorum using ABSTAIN votes, with only a small majority of FOR needed to pass. This is a known property of simple majority governance. The proposal bond (see Proposal Lifecycle) partially mitigates frivolous use.

### Attack surface

**Accumulate-before-snapshot, vote, sell:** An attacker buys ARM before the snapshot block, votes, then dumps. Cost is real capital exposed during at minimum the 2-day proposal delay plus the voting period. At extended parameters this is 9+ days. The treasury outflow rate limit (see Treasury) is the primary defense against the capture loop — voting mechanics alone are not sufficient.

**Quorum suppression:** A large holder never delegates, keeping their tokens out of the denominator. This shrinks the absolute quorum threshold, making it easier for a small coalition to pass proposals. Mitigation: claim-time delegation requirement prevents tokens from sitting inert. A holder can still self-delegate to an abstain address but cannot leave tokens completely unaccounted.

**Exchange delegation concentration:** Tokens held on exchanges may be delegated in bulk by the exchange, creating concentrated voting power not representing individual holders. The crowdfund's non-custodial claim structure reduces this risk at launch; it becomes more relevant as secondary market volume grows.

---

## Scope

### Governable

| Category | Items |
|----------|-------|
| **Economics** | Fee tiers, volume thresholds, yield fee rate |
| **Operations** | Treasury allocations, grants, partnerships |
| **Parameters** | Batch windows, relayer config, yield sources |
| **Upgrades** | Contract upgrades, new adapters |
| **Integrators** | Custom terms for specific integrators |

### Immutable

| Category | Items |
|----------|-------|
| **Cryptography** | ZK circuits, BN254 verifier |
| **Note structure** | Commitment format, nullifier derivation |
| **Privacy guarantees** | Shielded set membership, unlinkability |

---

## Treasury Steward ⚠️ TBD ⚠️

### Role

Elected role responsible for:
- Day-to-day treasury operations
- Executing approved proposals
- Emergency response coordination

### Authority

**Can:**
- Execute pre-approved recurring expenses
- Deploy up to 1% of treasury/month (operational budget)
- Fast-track security-critical actions

**Cannot:**
- Unilaterally change protocol parameters
- Change fee rates (governance only)
- Deploy >1% without governance approval
- Override vetoed proposals

### Election

- Initial steward: Core team
- Term: 6 months
- Re-election: Requires new vote
- Removal: Via standard proposal (majority quorum)

---

## Token Distribution ⚠️ TBD ⚠️

| Allocation | % | Lock | Notes |
|------------|---|------|-------|
| MVP crowdfund | 10% | None | $1.2M at $12M FDV |
| Airdrop | 5% | Revenue-gated | Namada community & github |
| Team | 10% | Revenue-gated | Unlocks as protocol earns |
| v1 crowdfund | 10% | None | $1.5M at $15M FDV |
| Treasury | 65% | Governance | Outflow limits |

"Word-of-mouth whitelist" crowdfund will be described soon.

⚠️ This is a draft: all token mechanics are subject to change ⚠️

### Revenue-Gated Unlocks

Team and airdrop tokens unlock based on cumulative protocol fee revenue.

| Cumulative Revenue | % Unlocked |
|--------------------|------------|
| $10k | 10% |
| $50k | 25% |
| $100k | 40% |
| $250k | 60% |
| $500k | 80% |
| $1M | 100% |

**No time-based fallback.** If revenue never reaches $1M, tokens never fully unlock.

**Rationale:**
- Team gets ARM when protocol earns
- No unlock calendar to front-run
- Unlocks signal traction, not dilution
- Fully aligned incentives

---

## Fee Structure ⚠️ TBD ⚠️

### Overview

Users pay: **Armada take + Integrator fee**

| Component | Who sets | Behavior |
|-----------|----------|----------|
| Armada take | Governance (via tiers) | Decreases as integrator volume grows |
| Integrator fee | Integrator | Base fee (self-set) + bonus (from Armada reduction) |

Fees are charged **only at shield (deposit)**. All other operations are free:

| Action | Fee |
|--------|-----|
| Shield (deposit) | Armada take + Integrator fee |
| Shielded transfer | Free |
| Shielded swap | Free |
| Shielded lend | Free |
| Unshield (withdraw) | Free |
| Yield redemption | 10% of yield (separate) |

### Volume Tiers

As integrators drive volume, Armada's take decreases. The reduction becomes integrator bonus:

| Cumulative Volume | Armada Take | Integrator Bonus |
|-------------------|-------------|------------------|
| $0 - $250k | 50 bps (0.5%) | 0 bps |
| $250k+ | 40 bps (0.4%) | 10 bps (0.1%) |

Governance can add additional tiers later if needed.

### Integrator Fee Calculation

```
Integrator total fee = Integrator base fee + Integrator bonus
```

- **Base fee:** Set by integrator via `setIntegratorFee(bps)`, can be 0
- **Bonus:** Automatically calculated from volume tier
- Integrator can change base fee anytime
- Integrator can lower total fee (pass savings to users) but bonus is the floor

### Example: New Integrator "Newb1"

1. Newb1 registers with 0 bps base fee
2. Users pay 50 bps (all to Armada)
3. Newb1 reaches $250k cumulative volume
4. Armada take drops to 40 bps, Newb1 gets 10 bps bonus
5. Users still pay 50 bps (40 bps Armada + 10 bps Newb1)
6. Newb1 could lower their fee to 5 bps → users pay 45 bps
7. Or Newb1 keeps 10 bps and earns on volume

### Example: Premium Integrator

1. Premium integrator sets 25 bps base fee (for extra features)
2. At $0 volume: users pay 75 bps (50 Armada + 25 integrator)
3. At $250k volume: users pay 75 bps (40 Armada + 35 integrator)
4. User cost unchanged; integrator captures more as volume grows

### Direct Shields

Users who shield directly (no integrator) pay base Armada take (50 bps) only.

### Yield Fee

Separate from shield fees. Treasury takes 10% of yield at redemption.

| Parameter | Value | Governance |
|-----------|-------|------------|
| Yield fee | 10% of yield earned | Adjustable |

### Fee Changes

**All fee parameters require governance proposal.** Steward has no discretion over fee rates.

Governance can adjust:
- Base Armada take (default 50 bps)
- Volume tier thresholds
- Armada take at each tier
- Yield fee percentage
- Custom terms for specific integrators

---

## Integrator Custom Terms

Governance can grant specific integrators custom terms via proposal:

```solidity
setIntegratorTerms(
    address integrator,
    uint256 customArmadaTakeBps,  // override Armada take
    uint256 customVolumeThreshold, // override qualification threshold
    bool hasCustomTerms
)
```

**Use cases:**
- Strategic partnerships (reduced Armada take)
- Ecosystem grants (waived volume threshold)
- Promotional periods (temporary discounts)

---

## On-Chain Tracking

Integrator volume tracked on-chain per address:

```solidity
// Query any integrator's stats
function getIntegratorVolume(address integrator) external view returns (uint256);
function getIntegratorEarnings(address integrator) external view returns (uint256);
function getIntegratorBonus(address integrator) external view returns (uint256);
function getArmadaTake(address integrator) external view returns (uint256);
function getUserFee(address integrator) external view returns (uint256);
```

Events emitted for indexing:

```solidity
event Shield(
    address indexed asset,
    uint256 amount,
    address indexed integrator,
    uint256 integratorCumulativeVolume,
    uint256 armadaTakeBps,
    uint256 integratorFeeEarned
);
```

Leaderboards and dashboards can be built without centralized indexers.

---

## Relayer Economics ⚠️ TBD ⚠️

Relayers should ultimately operate a competitive market, separate from protocol fees. But it's probably more realistic that relayer operations will be more altruistically run by Armada community members.

| Aspect | Design |
|--------|--------|
| Fee setting | Relayers set their own fees |
| Fee structure | Gas cost + markup (% of gas, not tx amount) |
| Selection | Wallets auto-select cheapest relayer |
| Payment | Deducted from user's shielded note |
| Protocol cut | None (Armada takes no cut of relayer fees) |

Users can self-relay (pay their own gas) to avoid relayer fees, trading privacy for cost savings.

---

## Treasury Distributions ⚠️ TBD ⚠️

Treasury can distribute ARM via two methods:

### Direct Distribution

ARM tokens sent directly to recipient wallet.

- Immediate liquidity
- Taxable at receipt (in most jurisdictions)
- Use for: contractors, one-time payments, immediate needs

### Claims

Right to receive ARM tokens, exercisable at recipient's discretion.

- Recipient controls when to trigger taxable event
- No expiration
- Use for: grants, ongoing contributors, long-term alignment

```solidity
// Recipient can exercise anytime
function exerciseClaim(uint256 amount) external {
    require(!windingDown, "Claims frozen in wind-down");
    require(claimable[msg.sender] >= amount, "Insufficient claim");
    claimable[msg.sender] -= amount;
    ARM.transferFrom(treasury, msg.sender, amount);
}
```

**Tax note:** Claims defer the taxable event until exercise. Recipients control timing. Consult a tax advisor for your jurisdiction.

---

## Wind-Down ⚠️ TBD ⚠️

If governance votes to wind down the protocol:

### Sequence

1. Wind-down proposal passes
2. MASP enters withdraw-only mode (immediate)
3. Claims convert to vault shares on pro-rata terms (same as unlocked ARM)
4. 30-day exit window for shielded users
5. Treasury liquidated to stables
6. Distribution to circulating ARM holders pro-rata

### MASP Withdraw-Only Mode

- `shield()` — disabled
- `transfer()` — disabled
- `unshield()` — always available, indefinitely

Users can always exit their shielded positions. MASP contract stays live forever for withdrawals.

### Treasury Distribution

Pro-rata to all **circulating** ARM holders.

**Circulating includes:**
- Crowdfund tokens (Round 1 and Round 2)
- Unlocked team/airdrop tokens
- Exercised claims and other treasury distributions

**No treasury claim:**
- Revenue-locked team/airdrop tokens not yet unlocked
- Unexercised claims (convert to vault shares, not frozen)

### Rationale

Those who paid for tokens (crowdfund participants) have priority in failure scenarios. Locked tokens only unlock if protocol earns revenue — if it failed before earning revenue, those tokens stay locked and have no claim on remaining assets.

---

## Security Council ⚠️ TBD ⚠️

3-of-5 multisig with authority to:
- Pause new shields only (24h max without ratification) — unshields are never pauseable
- Fast-track security patches
- Veto proposals compromising security

**Composition:** Core team (2), external security (2), community (1)

**Limitations:**
- Cannot deploy treasury funds
- Cannot change fee parameters
- Cannot permanently change parameters
- All actions require 7-day retroactive ratification

---

## Proposal Lifecycle

```
1. DRAFT       → Proposer creates with 0.1% ARM bond
2. PENDING     → 2-day review period (proposer can cancel)
3. ACTIVE      → 5-7 day voting (FOR / AGAINST / ABSTAIN)
4. OUTCOME     → DEFEATED (quorum not met, or majority AGAINST)
               → SUCCEEDED (quorum met + majority FOR)
5. QUEUED      → 2-4 day timelock
6. EXECUTED    → On-chain, irreversible
```

**Proposal bond:** Returned on success, and on honest failures (did not reach quorum). Slashed on proposals defeated by active majority AGAINST vote. Discourages bad-faith proposals without penalizing honest attempts that simply lacked support.
