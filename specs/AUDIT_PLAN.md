# Armada Audit Plan

## Scope

Three audit tracks:

### 1. Crowdfund Contracts
**Priority**: Highest (gates the crowdfunds) 
**Approach**: Traditional audit 
**Timing**: Complete before crowdfund opens  
**Notes**: Longest lead-time item after shielded pool

### 2. Governance Contracts
**Priority**: Medium  
**Approach**: Traditional audit, OR launch with OpenZeppelin Governor as interim and upgrade once custom contracts are audited  
**Timing**: Before governance goes live with real stake  
**Notes**: OpenZeppelin Governor fallback is battle-tested + credible. Scope both paths against audit cost and timeline

### 3. Shielded Pool + Adapters
**Priority**: Required before mainnet launch 
**Approach**: Traditional audit for integration layer (not Railgun's ZK circuits, which are already audited + battle-tested), then crowdsourced contest (eg. code4rena) as second pass  
**Timing**: After crowdfund's done, during build phase  
**Notes**: ZK circuit inheritance from Railgun reduces scope, but modern AI tools should be used. Any deviations from Railgun's design must be documented and flagged explicitly for auditors.

---

## Sequencing

1. **Ian: internal review** — Slither, Foundry invariant testing, known issues addressed before engaging any firm. Cleaner code = lower audit hours = lower cost.
2. **Traditional audit** — crowdfund contracts first, governance second (or interim OpenZeppelin path)
3. **Cantina or Code4rena contest** — shielded pool + adapters, after traditional audit as a second coverage layer
4. **Immunefi** — ongoing bug bounty running continuously post-mainnet

---

## Firms to Evaluate

Shortlist for traditional audit. Priority for firms with ZK or privacy protocol experience, esp familiarity with Railgun-adjacent work (to reduce auditor ramp time).

- **Informal Systems**
- **Trail of Bits**
- **Zellic** (also operates Code4rena)
- **Spearbit**
- **Sigma Prime**

Action: get scope estimate from Armada's smart contract developers (lines of code, contracts, complexity hotspots), then request quotes and lead times from three or four firms.

---

## Crowdsourced / Contest Platforms

For shielded pool + adapters (post-traditional audit):

| Platform | Format | Best for |
|---|---|---|
| **Cantina** | Team + crowdsourced hybrid | Deep coverage with structured component |
| **Code4rena** | Competitive contest (Wardens) | Breadth, public credibility |
| **Sherlock** | Competitive + coverage insurance | Trust signal for integrators — worth evaluating seriously given B2B positioning |
| **Immunefi** | Ongoing bug bounty | Post-launch continuous coverage |

---

## Cost Notes

- ZK-adjacent smart contract audits reportedly $50k - 150k+, depending on scope and firm; is there a way to offer tokens?
- Scope estimate from Armada's smart contract developers is a prerequisite for any meaningful quotes
- OpenZeppelin Governor interim path may meaningfully reduce governance audit cost/timeline
- Internal cleanup before audit engagement is the highest-leverage cost-reduction lever

---

## Open Items

- [ ] Armada's smart contract developers to provide scope estimate (contracts, lines of code, complexity)
- [ ] Source audit firm connections (does anyone in our sphere have existing relationships?)
- [ ] Evaluate OpenZepplin Governor interim path vs. custom governance audit in parallel
- [ ] Get quotes and lead times from 3 or 4 firms once scope is ready
- [ ] Size audit budget against crowdfund targets
