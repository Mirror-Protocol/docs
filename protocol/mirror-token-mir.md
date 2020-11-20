# \*\*Mirror Token \(MIR\)

The Mirror Token \(MIR\) is Mirror Protocol's governance token. Currently, it must be staked to vote on active polls and is required as a deposit for making new governance polls. In future iterations of Mirror, it will serve further purposes for the protocol that increase its utility and value.

Users that stake MIR tokens also earn MIR rewards generated from withdrawing collateral from CDP positions within the protocol.

## **Mirror Token Supply**

There are planned to be a total of 370,580,000 MIR tokens to be distributed over 4 years. Beyond that, there will be no more new MIR tokens introduced to the supply.

### Distribution Schedule

|  | Genesis | Y1 | Y2 | Y3 | Y4 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| UNI airdrop | 9.15 | 9.15 | 9.15 | 9.15 | 9.15 |
| mAsset LP | 0 | 89.2125 | 133.81875 | 156.121875 | 167.2734375 |
| MIR LP | 0 | 20.5875 | 30.88125 | 36.028125 | 38.6015625 |
| Luna staking | 9.15 | 27.45 | 27.45 | 27.45 | 27.45 |
| Community pool | 36.6 | 36.6 | 54.9 | 91.5 | 128.1 |
| Token supply \(M\) | 54.90 | 183.00 | 256.20 | 320.25 | 370.58 |
| Annual inflation \(%\) | 0 | 233.33% | 40.00% | 25.00% | 15.71% |

#### Genesis

* UNI Airdrop:
* mAsset LP
* MIR LP
* Luna staking
* Community pool

Let T be the EOY1 supply of MIR tokens. 

* Initial airdrop \(Genesis\) 
  * 5% of T claimable by a snapshot of UNI token holders 
  * 5% of T claimable by a snapshot of LUNA stakers 
* Luna staking \(Genesis + 1 week / 1 year\) 
  * T/10 is airdropped over the course of 1 year to Luna stakers every block. Terraform Labs will not receive MIR tokens
* Liquidity mining \(Genesis + 1 week / 4 years\)
  * mAsset LP-Uniswap: Users can stake mAsset-UST-LP tokens on Uniswap and receive 24% of T, claimable daily. Each of the 13 initial pairs are weighted equally. Rewards continue by the same amount each year until the end of year 4. 
  * MIR LP-Uniswap**:** Users can stake MIR-UST-LP tokens on Uniswap and receive 6% of T, claimable daily. MIR staking offers approx. 3x rewards given to mAsset LP provision. Rewards continue by the same amount each year until the end of year 4. 
  * mAsset LP-Terraswap: Users can stake mAsset-UST-LP tokens on Terraswap and receive 24% of T, claimable daily. Each of the 13 initial pairs are weighted equally. Rewards continue by the same amount each year until the end of year 4. 
  * MIR LP-Terraswap**:** Users can stake MIR-UST-LP tokens on Terraswap and receive 6% of T, claimable daily. MIR staking offers approx. 3x rewards given to mAsset LP provision. Rewards continue by the same amount each year until the end of year 4. 

![](../.gitbook/assets/image%20%2816%29.png)

![](../.gitbook/assets/image%20%2821%29.png)

![](../.gitbook/assets/image%20%2818%29.png)

## Staking Rewards

{% hint style="info" %}
This section discusses staking rewards for MIR tokens, which come from trading fees used to buy back MIR from the market. Staking LP tokens also generates MIR rewards, which come directly from new MIR tokens created every block. Learn more about it [here](lp-token.md#from-staking).
{% endhint %}

### From Protocol Fee

MIR Token stakers receive MIR token rewards every block, which are generated from [protocol fees](mirrored-assets-massets.md#protocol-fee) from CDP withdrawals. The protocol fees are collected from CDP collateral and are sold for TerraUSD to buy MIR through Terraswap. The MIR tokens are then distributed as rewards to MIR stakers in proportion to the percentage of total stake. This process balances the generation of new MIR by creating buying pressure.

### From Poll Creation Fees

Whenever a new governance poll is created, an initial deposit of MIR tokens must be paid. This deposit is distributed to all MIR stakers proportionately.

