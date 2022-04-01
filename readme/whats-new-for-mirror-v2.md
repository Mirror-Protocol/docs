# What's new for Mirror v2?

## What’s new for Mirror v2?&#x20;

_The below changes are based on the_ [**Mirror V2 Whitepaper**](https://mirror.finance/Mirror\_Protocol\_v2.pdf)**.**

Coming into Mirror Protocol v2, there are new feature additions which supplement existing mechanisms from v1 so that all classes of users are sufficiently incentivized for their given contributions within the protocol.

### **Governance participation incentives**

Governance plays a crucial role in the decision-making and success of the Mirror Protocol, but users were not sufficiently incentivized to actively participate. As a result, the quorums were often not reached as users were disincentivized by the fact that their staked MIR tokens were locked until the end of the poll. This was further exacerbated by the fact that MIR deposits were mandatory to create new polls, so poll creators had a relatively high chance to lose their MIR due to lack of poll participation.\
\
In Mirror Protocol v2, active voters will be eligible for additional voting rewards in addition to existing governance staking rewards. An `ABSTAIN` vote option is also added for users who want to participate actively in governance but feel that they do not adequately understand the proposal. In addition, a snapshot of the quorum will be saved once it reaches a `Snapshot Period` to ensure that additional MIR staked in the governance contract does not affect the minimum amount of MIR needed to reach quorum.\
\
These changes will drive governance participation upwards by reducing risks related to poll creators’ deposits and incentivizing active participation.&#x20;

### **New Collaterals**

A highly requested feature addition by the community was to add MIR to the list of accepted collaterals for mint positions.&#x20;

Including MIR, [new collateral types](../protocol/mirrored-assets-massets.md#collateral) from Terra ecosystem have been added in Mirror v2. All collaterals will be given a new governance-decided parameter called the `multiplier` which is multiplied to the `min_collateral_ratio` of minted mAsset. Stable assets such as UST or aUST will have `multiplier`=1, and volatile collaterals including LUNA, MIR and ANC will be initially set to 1.3333334.

### **Short Incentives**

One of the largest issues in Mirror Protocol v1 was the persisting price premium between the Terraswap and Oracle price. To reduce price premiums, a user would have to mint an asset and then sell it against Terraswap pools. However, there was no incentive for a user to short an asset since rewards from providing liquidity with bought assets were higher. In addition, minting an asset was much less capital efficient than simply buying the mAsset from Terraswap, even with the price premiums.

Mirror v2 presents a new non-tradable token called [sLP tokens](../protocol/staking-tokens-lp-and-slp.md#slp-tokens-short-tokens), which is minted from creating a short position. sLP tokens are also stakable, and generates a reward that is dynamically increasing or decreasing based on the current price premium between Terraswap and Oracle price.&#x20;

### **Pre-IPO Assets**

Assets scheduled to undergo an IPO can be whitelisted and traded on Mirror v2. Any user can specify the details of the underlying asset via governance poll creation. If the poll passes, these assets will be minted (during a fixed time window) or traded like any other mAssets before the IPO. Once the IPO happens in the underlying market, [Mirror Oracle](broken-reference) will begin reporting prices from the market, and the asset will have the same features as any other mAsset.&#x20;

**Security Audit** for the underlying Mirror v2 Contracts is done by **Cryptonics.**

{% file src="../.gitbook/assets/Mirror v2 Audit Report (1).pdf" %}

## Genesis Parameters

For a better testing environment, any time-related parameters are shortened for Mirror v2 Testnet. Please note that all time parameters are now counted in `seconds` instead of `blocks`.

### Borrow (Mint)

| Parameter                                 | Testnet (v2) | Mainnet (v2)                |
| ----------------------------------------- | ------------ | --------------------------- |
| LUNA, LUNA X Collateral Ratio Multiplier  | 1.3333334    | 1.3333334                   |
| aUST Collateral Ratio Multiplier          | 1            | 1                           |
| UST Lock-up Period                        | 600 seconds  | 1,209,600 seconds (2 weeks) |
| Protocol Fee Rate                         | 1.5%         | 1.5%                        |

### Governance&#x20;

| Parameter        | Testnet (v2) | Mainnet (v2)             |
| ---------------- | ------------ | ------------------------ |
| Proposal Deposit | 1 MIR        | 1000 MIR                 |
| Quorum           | 5%           | 18%                      |
| Voting Period    | 318 seconds  | 604,800 seconds (1 week) |
| Snapshot Period  | 100 seconds  | 86,400 seconds (1 day)   |
| Effective Delay  | 331 seconds  | 86,400 secons (1 day)    |
| Voter Weight     | 50%          | 50%                      |

### Farming (Staking)

| Parameter               | Testnet (v2) | Mainnet (v2)          |
| ----------------------- | ------------ | --------------------- |
| Premium Update Interval | 600 seconds  | 3600 seconds (1 hour) |
