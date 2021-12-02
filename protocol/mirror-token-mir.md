# Mirror Token (MIR)

The **Mirror Token (MIR)** is Mirror Protocol's governance token. Currently, it must be staked to vote on active polls and is required as a deposit for making new governance polls. In future iterations of Mirror, it will serve further purposes for the protocol that increase its utility and value.

Users that stake MIR tokens also earn MIR rewards generated from withdrawing collateral from CDP positions within the protocol.

MIR is also used to incentivize users to farm yields by staking LP tokens which were minted by providing liquidity for MIR and mAssets. Yield is paid to the users from MIRs that are newly minted through annual inflation, which gradually increases the total supply of MIR until the end of 4th year.&#x20;

## **Mirror Token Supply**

There are planned to be a total of 370,575,000 MIR tokens to be distributed over 4 years. Beyond that, there will be no more new MIR tokens introduced to the supply.

### Cumulative Distribution Schedule (in millions)

|                      | Genesis   | Y1         | Y2         | Y3         | Y4          |
| -------------------- | --------- | ---------- | ---------- | ---------- | ----------- |
| UNI airdrop          | 9.15      | 9.15       | 9.15       | 9.15       | 9.15        |
| Luna staking airdrop | 9.15      | 9.15       | 9.15       | 9.15       | 9.15        |
| Luna staking reward  | 0         | 18.3       | 18.3       | 18.3       | 18.3        |
| mAsset LP staking    | 0         | 89.2125    | 133.81875  | 156.121875 | 167.2734375 |
| MIR LP staking       | 0         | 20.5875    | 30.88125   | 36.028125  | 38.6015625  |
| Community pool       | 36.6      | 36.6       | 54.9       | 91.5       | 128.1       |
| **Token supply**     | **54.90** | **183.00** | **256.20** | **320.25** | **370.575** |
| Annual inflation (%) | -         | 233.33%    | 40.00%     | 25.00%     | 15.71%      |

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MLRzugf7mxc4ryNhTuq%2Fuploads%2FdKfSHCIDtMhwXsVLNHwy%2Ffile.png?alt=media)

Total of **54.9M** tokens are available at genesis of Mirror Protocol. The distribution of these tokens will be made as below:

* **UNI Airdrop**: 16.66% (9.15M) tokens will be airdropped to UNI holders
* **LUNA staker airdrop**: 16.66% (9.15M) tokens will be airdropped to LUNA stakers.
* **Community Pool**: 66.66% (36.6M) tokens will be allocated to community pool.

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MLRzugf7mxc4ryNhTuq%2Fuploads%2FIC7UdrAp8Ne7WafFNKIk%2Ffile.png?alt=media)

Total supply of MIR tokens will increase for 4 years due to inflation, until the total token supply becomes **370.575M**.

The distribution structure at the end of year 4 will look like the below:

* **Airdrop**: The airdrop amount which was originally distributed to UNI holders and LUNA stakers will now account for 4.9% (18.3M) of the total token supply.
* **LUNA staking reward**: 4.9% (18.3M) will be distributed to LUNA stakers throughout the first year since the launching of Mirror Protocol. **** MIR will be distributed every 100,000 blocks (approximately once every week) to Luna stakers only on the first year, starting from block height 920,000. Snapshot will be taken every 100,000 blocks to determine who is eligible for the staking reward distribution.&#x20;
* **mAsset LP Staking**: 45.1% (167.27M) tokens are distributed to all mAsset and mAsset (mETH) staking pools by the end of year 4. Tokens are distributed daily to each staking pool (initially 13 pairs for each Mirror and mETH) based on their `weight` compared to other assets.
* **MIR LP Staking**: 10.4% (38.6M) tokens are evenly distributed to MIR-UST and MIR-UST (mETH) staking pools by the end of year 4. MIR-UST pair has an initial `weight` of 300, which is greater than  `weight` of mAssets (First batch of mAssets have 100, and whitelisted mAssets have 30 `weight`).&#x20;
* **Community Pool**: 34.6% (128.1M) of total MIR supply will be distributed to Community Pool by the end of year 4.&#x20;

#### **Distribution Rate (Inflation)**

Inflation rate of MIR tokens are designed to gradually decrease every year until it reaches 370.575M at the end of year 4. After the end of year 4, no more MIR tokens will be minted through inflation.

## MIR Staking Rewards

{% hint style="info" %}
This section discusses staking rewards for MIR tokens, which come from trading fees used to buy back MIR from the market. Staking LP and sLP tokens also generate MIR rewards, which come directly from new MIR tokens created every block. Learn more about it [here](staking-tokens-lp-and-slp.md#staking-rewards).
{% endhint %}

### From Protocol Fees

MIR Token stakers receive MIR token rewards every block which [protocol fees](mirrored-assets-massets.md#protocol-fee) are generated from CDP withdrawals. The protocol fees are collected from CDP collateral and are sold for TerraUSD to buy MIR through Terraswap after being sent to the [Collector ](../contracts/collector.md)contract. The MIR tokens are then distributed as rewards to MIR stakers and voters in proportion to the percentage of total stake. This process balances the generation of new MIR by creating buying pressure.

### From Poll Creation Fees

Whenever a new governance poll is created, an initial deposit of MIR tokens must be paid. If the poll does not reach the voting quorum, this deposit is distributed to all MIR stakers proportionately.

### From Voting Rewards

From protocol fee and poll creation fees from polls that have not reached voting quorum,  `voter_weight` is used to determine the amount of MIR tokens that are distributed among users who voted on on-going polls. If total MIR governance reward which includes both staking and voting rewards is $$R_{\text{total}}$$ is,

$$
R_{\text{passive}}+R_{\text{vote}}
$$

where $$R_{\text{passive}}$$is the reward for MIR stakers that did not vote, and $$R_{\text{vote}}$$is the reward for users that have voted on on-going polls. If `voter_weight` is $$W_{\text{vote}}$$, and each individual and total number of MIR voted for the $$i$$th poll are $$m_{\text{i}}$$ and $$M_{\text{i}}$$,the voting reward  $$R_{\text{vote}}$$ is,

$$
\sum_{i=1}^n \frac{m_{\text{i}}}{M_{\text{i}}} \frac{W_{\text{vote}}{R_{\text{gov}}}}{n}
$$

### APR Calculation

{% hint style="warning" %}
**Note** Since protocol fees for most mAssets are only generated during trading hours of their underlying real-world asset, the actual reward distributed over the weekends and holidays will be much lower.&#x20;
{% endhint %}

Mirror Web App displays the annual percentage rate for MIR staking reward. Due to fluctuating nature of protocol fees, MIR staking APR calculation uses the **average reward per day from the last 15 days**.

If reward for MIR staker on day $$i$$ is $$r_i$$, with total MIR staked amount is $$M$$, annualized percentage rate for MIR staking (365 days) would be,

$$
\frac{\sum_{i=1}^{15}m_i}{M}\times365
$$

where reward distribution information of the last 15 days is applied. This APR is calculated under the assumption that the users have voted on all on-going governance proposals.&#x20;

