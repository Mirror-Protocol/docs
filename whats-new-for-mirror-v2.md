# What's new for Mirror v2?

## What’s new for Mirror v2? 

_The below changes are based on_ [_Mirror Forum post_](https://forum.mirror.finance/t/mirror-v2-updates/1077)_._

Coming into Mirror Protocol v2, there are new feature additions which supplement existing mechanisms from v1 so that all classes of users are sufficiently incentivized for their given contributions within the protocol.

### **Governance participation incentives**

Governance plays a crucial role in the decision-making and success of the Mirror Protocol, but users were not sufficiently incentivized to actively participate. As a result, the quorums were often not reached as users were disincentivized by the fact that their staked MIR tokens were locked until the end of the poll. This was further exacerbated by the fact that MIR deposits were mandatory to create new polls, so poll creators had a relatively high chance to lose their MIR due to lack of poll participation.  
  
In Mirror Protocol v2, active voters will be eligible for additional voting rewards in addition to existing governance staking rewards. An `ABSTAIN` vote option is also added for users who want to participate actively in governance but feel that they do not adequately understand the proposal. In addition, a snapshot of the quorum will be saved once it reaches a `Snapshot Period` to ensure that additional MIR staked in the governance contract does not affect the minimum amount of MIR needed to reach quorum.  
  
These changes will drive governance participation upwards by reducing risks related to poll creators’ deposits and incentivizing active participation. 

### **New Collaterals**

A highly requested feature addition by the community was to add MIR to the list of accepted collaterals for mint positions. 

Including MIR, [new collateral types](protocol/mirrored-assets-massets.md#collateral) from Terra ecosystem have been added in Mirror v2. All collaterals will be given a new governance-decided parameter called the `multiplier` which is multiplied to the `min_collateral_ratio` of minted mAsset. Stable assets such as UST or aUST will have `multiplier`=1, and volatile collaterals including LUNA, MIR and ANC will be initially set to 1.3333334.

### **Short Incentives**

One of the largest issues in Mirror Protocol v1 was the persisting price premium between the Terraswap and Oracle price. To reduce price premiums, a user would have to mint an asset and then sell it against Terraswap pools. However, there was no incentive for a user to short an asset since rewards from providing liquidity with bought assets were higher. In addition, minting an asset was much less capital efficient than simply buying the mAsset from Terraswap, even with the price premiums.

Mirror v2 presents a new non-tradable token called [sLP tokens](protocol/staking-tokens-lp-and-slp.md#slp-tokens-short-tokens), which is minted from creating a short position. sLP tokens are also stakable, and generates a reward that is dynamically increasing or decreasing based on the current price premium between Terraswap and Oracle price. 

### **Pre-IPO Assets**

Assets scheduled to undergo an IPO can be whitelisted and traded on Mirror v2. Any user can specify the details of the underlying asset via governance poll creation. If the poll passes, these assets will be minted \(during a fixed time window\) or traded like any other mAssets before the IPO. Once the IPO happens in the underlying market, [Mirror Oracle](contracts/oracle.md) will begin reporting prices from the market, and the asset will have the same features as any other mAsset. 

**Security Audit** for the underlying Mirror v2 Contracts is done by **Cryptonics** \(TBD\).

## Genesis Parameters

For a better testing environment, any time-related parameters are shortened for Mirror v2 Testnet. Please note that all time parameters are now counted in `seconds` instead of `blocks`.

Mainnet parameters will be announced at the official launch of Mirror v2. 

### Borrow \(Mint\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Testnet (v2)</th>
      <th style="text-align:left">Mainnet (v1)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Collateral Ratio Multiplier</p>
        <p>(Luna, MIR, ANC)</p>
      </td>
      <td style="text-align:left">1.3333334</td>
      <td style="text-align:left">Not available</td>
    </tr>
    <tr>
      <td style="text-align:left">aUST Collateral Ratio Multiplier</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">Not available</td>
    </tr>
    <tr>
      <td style="text-align:left">UST Lock-up Period</td>
      <td style="text-align:left">600 seconds</td>
      <td style="text-align:left">Not available</td>
    </tr>
    <tr>
      <td style="text-align:left">Protocol Fee Rate</td>
      <td style="text-align:left">1%</td>
      <td style="text-align:left">1.5%</td>
    </tr>
  </tbody>
</table>

### Governance 

| Parameter | Testnet \(v2\) | Mainnet \(v1\) |
| :--- | :--- | :--- |
| Proposal Deposit | 1 MIR | 100 MIR |
| Quorum | 5% | 10% |
| Voting Period | 318 seconds | 100,000 Blocks |
| Snapshot Period | 100 seconds | Not available |
| Effective Delay | 331 seconds | 13,000 Blocks |
| Voter Weight | 50% | Not available |

### Farming \(Staking\)

| Parameter | Testnet \(v2\) | Mainnet \(v1\) |
| :--- | :--- | :--- |
| Premium Update Interval | 600 seconds | Not available |

