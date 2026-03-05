# Armada Crowdfunding Spec
⚠️ **in flux** ⚠️

This is a first draft, open for commentary ([Discord](https://discord.gg/PHJDgHMA)). Its complexity will need to be balanced with the need for simplicity.

## Overview

Armada will raise funds by "word-of-mouth whitelisting." The mechanism is algorithmic, predeclared, and non-discretionary.

### Round 1

| Parameter | Value |
|---|---|
| Purpose | Fund MVP |
| Min raise | 1,000,000 ARM ($1.0M) |
| Base raise | 1,200,000 ARM ($1.2M) |
| Max raise (elastic) | 1,800,000 ARM ($1.8M) |
| Percent of supply | 10 - 15% |
| Price | $1.00 per ARM |
| Timing | Pre-product |

### Round 2 (Not Guaranteed)

Round 2 depends on MVP progress, market conditions, and governance decisions. Round 1 participants are acquiring ARM tokens in a pre-product protocol with governance power and no promise of future crowdfunding or liquidity events.

If Round 2 occurs:

| Parameter | Value |
|---|---|
| Purpose | Fund v1 |
| Raise | 1,200,000 ARM ($1.5M) |
| Supply | 10% |
| Price | $1.25 per ARM |
| Timing | MVP launch |

---

## Token Distribution ⚠️ in flux ⚠️

**Total supply: 12,000,000 ARM**

| Allocation | ARM | % | Unlock | Voting Power |
|---|---|---|---|---|
| Round 1 crowdfund | 1,200,000 - 1,800,000 | 10-15% | Governance proposal | Immediate |
| Round 2 crowdfund | 1,200,000 | 10% | Governance proposal | Immediate |
| Team | 1,200,000 | 10% | Revenue milestones | Revenue-gated |
| Airdrop | 600,000 | 5% | Revenue milestones | Revenue-gated |
| Treasury | 7,200,000 - 7,800,000 | 60-65% | Governance-controlled | None |

### Pre-Crowdfund Distribution

Before Round 1 opens, the following revenue-locked allocations are distributed and published:

**Team Allocation (1,200,000 ARM):**
- Distributed to founding team multisig
- Multisig assigns to individual team members
- All assignments are revenue-locked
- Distribution is published before crowdfund

**Airdrop Allocation (600,000 ARM):**
- Distributed to qualifying recipients
- Revenue-locked (same terms as team)
- Criteria and recipient list TBD

**Transparency:**

Crowdfund participants can evaluate the full distribution before committing: who holds what and under what terms.

### Team Allocation

The team allocation (1,200,000 ARM, 10%) is controlled by a founding team multisig.

**Properties:**
- Revenue-locked transferability (same schedule as all team/airdrop tokens)
- Voting power unlocks proportionally with revenue milestones
- Portions within the multisig can be reassigned to individual team members
  - Internal splits do not require governance approval

**Constraints:**
- Cannot change the revenue-lock schedule
- Cannot pull from treasury or other allocations
- All reassigned portions inherit the same unlock terms

**Use cases:**
- Assigning tokens to new team members
- Adjusting allocations as roles change
- Recovering allocation if a team member departs

### Revenue-Gated Unlocks (Team + Airdrop)

Team and airdrop tokens unlock and gain voting power only via cumulative protocol revenue:

| Cumulative Revenue | Unlocked | Voting Power |
|---|---|---|
| $0 | 0% | 0% |
| $10k | 10% | 10% |
| $50k | 25% | 25% |
| $100k | 40% | 40% |
| $250k | 60% | 60% |
| $500k | 80% | 80% |
| $1M | 100% | 100% |

Governance power follows economic commitment. Until the protocol generates revenue, only people who paid decide.

---

## Word-of-Mouth Whitelist

### Design Philosophy

This is a trusted-network sale, not viral growth. Participation requires invitation from someone closer to the core. Each hop has a ceiling — the maximum fraction of the raise it can absorb — but no floor. Actual allocation is demand-driven: each hop receives whatever it committed, up to its ceiling, and anything unused rolls forward to later hops.

### Seed Selection

Seed criteria are determined by the founding team based on demonstrated alignment, contribution, and long-term interest in privacy infrastructure.

### Hop Structure

| Hop | Description | Ceiling | Cap | Invites |
|---|---|---|---|---|
| 0 | Seeds | 70% | $15,000 | 3 |
| 1 | Seed invites | 45% | $4,000 | 2 |
| 2 | Hop-1 invites | 10% | $1,000 | 0 |

Ceilings are overlapping by design — they sum to 125%, not 100%. No hop is guaranteed its ceiling. Each hop absorbs up to its ceiling based on actual demand; anything unused rolls forward. If all hops are fully subscribed, earlier hops fill first and later hops absorb whatever remains up to their ceiling.

### Ceiling Amounts

**Base raise ($1.2M):**

| Hop | Ceiling | Max allocation |
|---|---|---|
| 0 | 70% | $840,000 |
| 1 | 45% | $540,000 |
| 2 | 10% | $120,000 |

**Expanded raise ($1.8M):**

| Hop | Ceiling | Max allocation |
|---|---|---|
| 0 | 70% | $1,260,000 |
| 1 | 45% | $810,000 |
| 2 | 10% | $180,000 |

### Invitation Limits

Invitation limits are hop-specific and predeclared:

- Hop-0 → may invite up to 3 addresses to hop-1
- Hop-1 → may invite up to 2 addresses to hop-2
- Hop-2 → may not invite

Invitations are signed on-chain (inviter → invitee). An address can only participate at one hop level.

### Sybil Resistance

The mechanism does not attempt to perfectly prevent same-entity self-invites. Instead, it relies on:

1. **Opportunity cost:** A seed who self-invites trades access to up to $15,000 (hop-0 cap) for access to up to $4,000 (hop-1 cap). The opportunity cost is concrete and cap-based, independent of how full each hop is. If a seed prefers to fill all their allocation space themselves rather than pass it along, they can — they simply do so at the hop-0 level. Self-inviting to hop-1 is a strictly worse outcome for anyone who wants significant allocation.
2. **Threshold gates:** Rollover requires minimum unique participants, making sybil capture of leftover capacity structurally harder.
3. **Graph visibility:** Post-sale reveal links inviters to invitee behavior.
4. **Non-transferability:** Accumulating governance power in a pre-product protocol has low ROI.

Note: with demand-driven ceilings rather than fixed reserves, the sybil opportunity cost is cap-based rather than reserve-based. The argument is slightly different from a fixed-reserve design but the practical deterrent is equivalent — the cap differential ($15k vs $4k) is what matters, not the ceiling percentage.

Residual risk—sybiling to accumulate votes—is mitigated by trusted-network seed selection.

---

## Sale Mechanics

### Timeline

| Phase | Duration |
|---|---|
| Invitation window | 2 weeks |
| Commitment window | 1 week |

### Commitment

- **Currency:** USDC
- **Method:** Commit to escrow contract
- **Visibility:** Address, amount, hop are public; invite edges hidden until after sale

Real-time aggregate statistics show current commitment levels per hop, allowing participants to see oversubscription before committing.

### Allocation

Each hop has a ceiling — the maximum it can absorb from the total raise. Allocation is demand-driven: each hop receives `min(actual demand, ceiling)`. Anything below the ceiling is leftover and rolls forward. Oversubscription (demand exceeding ceiling) is resolved pro-rata, bounded by per-address caps.

```python
for hop in [0, 1, 2]:
    ceiling = hop_ceilings[hop]  # ceiling × sale_size
    effective = {a: min(a.committed, hop_cap[hop]) for a in hop_addresses}
    total_demand = sum(effective.values())
    
    if total_demand <= ceiling:
        for a in hop_addresses:
            a.allocation = effective[a]
        leftover[hop] = ceiling - total_demand
    else:
        scale = ceiling / total_demand
        for a in hop_addresses:
            a.allocation = effective[a] * scale
        leftover[hop] = 0
```

### Rollover Rules

Unused ceiling capacity rolls down to later hops, subject to participation thresholds:

| Condition | Action |
|---|---|
| Hop-1 unique committers ≥ 30 | Hop-0 leftover → hop-1 (increases hop-1 effective ceiling) |
| Hop-1 unique committers < 30 | Hop-0 leftover → treasury |
| Hop-2 unique committers ≥ 50 | Hop-1 leftover → hop-2 (increases hop-2 effective ceiling) |
| Hop-2 unique committers < 50 | Hop-1 leftover → treasury |

Leftover never rolls back to earlier hops. This prevents seeds from benefiting from failed propagation.

When leftover rolls forward, the receiving hop's effective ceiling increases by the leftover amount. The hop then allocates against this expanded ceiling using the same demand-driven logic. Total allocation across all hops cannot exceed the sale size.

### Elastic Expansion (Round 1 Only)

Round 1 may expand if demand significantly exceeds base supply. Round 2 (if occurs) is fixed.

```python
if total_capped_demand >= 1.5 * BASE_SALE:
    sale_supply = MAX_SALE  # 1,800,000 ARM
else:
    sale_supply = BASE_SALE  # 1,200,000 ARM
```

Expansion is binary: either base ($1.2M) or max ($1.8M). If triggered, supply expands directly to maximum; excess demand beyond $1.8M is refunded pro-rata.

### Refunds

```
refund_usdc = committed_usdc - (allocation_arm × price)
```

Refunds are automatic and immediate after allocation finalizes.

---

## Round 2 Whitelist Access

If Round 2 occurs, Round 1 participants receive automatic hop-0 status:

- Same cap as other hop-0 seeds
- Can invite up to 3 addresses to hop-1
- Compete for hop-0 ceiling alongside new seeds

Additional hop-0 seeds may be added based on MVP-phase relationships.

Round 1 participation is rewarded via access, not superlinear allocation.

---

## Governance

### Voting Power

Voting power depends on token type and protocol revenue:

| Token Type | Voting Power |
|---|---|
| Crowdfund (Round 1 + Round 2) | Full, immediate |
| Team | Proportional to revenue-based unlock |
| Airdrop | Proportional to revenue-based unlock |
| Treasury | None |

**Example at $0 revenue:**
- Only crowdfund tokens vote
- Team and airdrop have no governance power
- All decisions made by people who paid

**Example at $100k cumulative revenue:**
- Crowdfund: 1.5M ARM voting (100%)
- Team: 480k ARM voting (40% of 1.2M)
- Airdrop: 240k ARM voting (40% of 600k)
- Treasury: 0 ARM voting

### Rationale

Governance power follows economic commitment. Crowdfund participants paid for their tokens and bear the risk of failure. Team and airdrop recipients earn governance influence by generating protocol revenue—the same milestone that unlocks their tokens.

This removes ambiguity: only people who paid decide unlock timing, treasury strategy, and protocol direction until the protocol proves itself.

### Commitment Requirement

ARM must be committed to vote. Uncommitted ARM does not count toward quorum or voting.

Voting power can be delegated to any address.

### Quorum and Passing

```
quorum = 20% of committed voting-eligible ARM
passing = >50% of votes cast
```

**Example at Round 1 close (no revenue):**
- 1.2M ARM voting-eligible (crowdfund only)
- If 800k ARM committed: 160k quorum, 80k+ to pass

**Example after $250k revenue:**
- 1.2M crowdfund + 720k team + 360k airdrop = 2.28M voting-eligible
- If 1.5M committed: 300k quorum, 150k+ to pass

### Proposal Timeline

| Phase | Duration |
|---|---|
| Voting delay | 2 days |
| Voting period | 7 days |
| Enactment delay | 7 days |
| **Total** | **16 days** |

### Token Unlock Proposal

Crowdfund tokens become transferable via governance proposal. There are no predeclared conditions—holders decide when.

Team and airdrop tokens cannot be unlocked by governance. They unlock only via revenue milestones.

### Treasury Control

Treasury tokens (60-65%) are controlled by governance but do not vote.

### Treasury Distributions

**USDC Operations (Steward):**
- One-time payments up to $40k: steward proposal, pass by default, 5-day veto window
- Steward submits monthly operations proposals as batched payments
- No recurring or streaming payments in v1

**ARM Distributions (Governance Only):**
- Require standard governance proposal
- Distributed ARM has full voting power immediately

### Steward Role ⚠️ in flux ⚠️

A steward can be elected with special powers for treasury fund operations.

**Scope:**
- USDC operations only (grants, expenses, service payments)
- Cannot propose ARM distributions
- Cannot modify protocol parameters
- Cannot vote on unlock proposals

**Limits:**
- Per-proposal maximum: $40,000 USDC
- Monthly maximum: $120,000 USDC
- Proposals exceeding limits require standard governance

**Process:** 
- Steward proposals pass by default unless voted down
- Veto window: 7 days
- Only voting-eligible ARM can veto

**Term:**
- 6 months, renewable by governance proposal
- Ejection by standard governance proposal (immediate effect)

---

## Invite Graph Visibility

| Phase | Visibility |
|---|---|
| During sale | Aggregates only (counts, totals by hop) |
| After finalization | Full graph (all edges, all hops) |

Post-sale transparency strengthens legitimacy. The privacy-sensitive moment is during commit, not after.

---

## Wind-Down

If integrator demand or volume does not materialize, governance may vote to wind down:

1. Shielded pool enters withdraw-only mode (users can always exit)
2. Treasury distributed to circulating ARM holders
3. Team and airdrop tokens have no treasury claim (zero-cost-basis exclusion)

Those who paid have priority over those who received tokens at zero cost basis.

Idea: there could be an automated wind-down, for example if 10k USDC in revenue has not been achieved in 8 months.

---

## Risk Acknowledgments

- **Seed quality is critical.** The mechanism scales trust outward from seeds.
- **Governance capture is possible.** Whitelist is the mitigation, not a guarantee.
- **Propagation may fail.** Unused supply flows to treasury.
- **Round 2 is not guaranteed.** Round 1 is a standalone commitment.
- **This is a trusted-network sale.** Not designed for viral distribution.
- **Non-transferability is intentional.** Prioritizes governance participation over liquidity during formative phase.

---

## Summary of Design Decisions

| Decision | Resolution | Rationale |
|---|---|---|
| Treasury voting | Treasury does not vote | Avoids control ambiguity |
| Team/airdrop voting | Revenue-gated | Only people who paid decide until protocol proves itself |
| Steward voting eligibility | Voting-eligible ARM only | Treasury decisions by economic stakeholders |
| Invite graph | Full reveal post-sale | Transparency strengthens legitimacy |
| Enactment delay | 7 days | Prevents coordination without feeling like lockup |
| Round 2 caps | Same as other hop-0 seeds | Rewards access, not superlinear allocation |
| Elastic expansion | Round 1 only, binary to 1.8M ARM | Accommodates demand without Round 2 price distortion |
| Team allocation control | Multisig reassignment | Flexibility without governance overhead |
| Pre-crowdfund distribution | Team + airdrop distributed before Round 1 | Full transparency for funders |
| Recurring payments | Not supported in v1 | Steward submits monthly batches instead |
| Hop allocation model | Demand-driven ceilings, not fixed reserves | Capacity follows actual demand; no hop is guaranteed allocation it didn't earn; unused capacity rolls forward |
| Overlapping ceilings | Sum to 125% (70/45/10) | Intentional — later hops can absorb more if earlier hops underperform, without needing to predeclare exact splits |
| No per-hop floors | Informational only | Global minimum raise ($1M) provides the real floor; per-hop floors add complexity without meaningful protection |

---

## Appendix: Allocation Pseudocode

```python
# Constants
TOTAL_SUPPLY = 12_000_000
BASE_SALE = 1_200_000
MAX_SALE = 1_800_000
EXPANSION_TRIGGER = 1.5

# Overlapping ceilings — sum to 125%, not 100%.
# Each hop absorbs up to its ceiling; unused rolls forward.
HOP_CEILING_PCT = {0: 0.70, 1: 0.45, 2: 0.10}
HOP_CAP = {0: 15_000, 1: 4_000, 2: 1_000}
PRICE = 1.00  # USDC per ARM

ROLLOVER_THRESHOLD = {1: 30, 2: 50}  # minimum unique committers

def allocate(commitments):
    # Step 1: Determine sale size (binary expansion)
    capped_demand = sum(
        min(c.usdc_committed, HOP_CAP[c.hop]) 
        for c in commitments
    )
    
    if capped_demand >= EXPANSION_TRIGGER * BASE_SALE:
        sale_size = MAX_SALE
    else:
        sale_size = BASE_SALE
    
    # Step 2: Calculate per-hop ceilings
    # These are maximums, not guarantees. Effective ceiling may increase via rollover.
    ceilings = {h: HOP_CEILING_PCT[h] * sale_size for h in [0, 1, 2]}
    effective_ceilings = dict(ceilings)  # updated during rollover
    
    # Step 3: Allocate per hop (demand-driven, up to ceiling)
    allocations = {}
    leftover = {}
    
    for hop in [0, 1, 2]:
        hop_commits = [c for c in commitments if c.hop == hop]
        effective = {c.address: min(c.usdc_committed, HOP_CAP[hop]) for c in hop_commits}
        demand = sum(effective.values())
        ceiling = effective_ceilings[hop]
        
        if demand <= ceiling:
            for addr, amt in effective.items():
                allocations[addr] = amt / PRICE
            leftover[hop] = ceiling - demand
        else:
            scale = ceiling / demand
            for addr, amt in effective.items():
                allocations[addr] = (amt * scale) / PRICE
            leftover[hop] = 0
    
    # Step 4: Rollover
    # Leftover increases the next hop's effective ceiling, then reallocates.
    def unique_committers(hop):
        return len(set(c.address for c in commitments if c.hop == hop))
    
    def reallocate_hop(hop, additional_capacity):
        hop_commits = [c for c in commitments if c.hop == hop]
        effective = {c.address: min(c.usdc_committed, HOP_CAP[hop]) for c in hop_commits}
        demand = sum(effective.values())
        new_ceiling = effective_ceilings[hop] + additional_capacity
        effective_ceilings[hop] = new_ceiling
        
        if demand <= new_ceiling:
            for addr, amt in effective.items():
                allocations[addr] = amt / PRICE
            return new_ceiling - demand
        else:
            scale = new_ceiling / demand
            for addr, amt in effective.items():
                allocations[addr] = (amt * scale) / PRICE
            return 0
    
    treasury_leftover = 0
    
    # Roll hop-0 leftover to hop-1
    if leftover[0] > 0:
        if unique_committers(1) >= ROLLOVER_THRESHOLD[1]:
            leftover[1] = reallocate_hop(1, leftover[0])
        else:
            treasury_leftover += leftover[0]
    
    # Roll hop-1 leftover to hop-2
    if leftover[1] > 0:
        if unique_committers(2) >= ROLLOVER_THRESHOLD[2]:
            remaining = reallocate_hop(2, leftover[1])
            treasury_leftover += remaining
        else:
            treasury_leftover += leftover[1]
    
    # Any hop-2 leftover goes to treasury
    treasury_leftover += leftover.get(2, 0)
    
    # Step 5: Calculate refunds
    refunds = {}
    for c in commitments:
        allocated_usdc = allocations.get(c.address, 0) * PRICE
        refunds[c.address] = c.usdc_committed - allocated_usdc
    
    return allocations, refunds, treasury_leftover
```
