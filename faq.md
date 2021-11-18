# FAQ

#### 1. What are Mirror synthetic assets?

The aim of mAssets is to mimic the price trends of real-world exchange-traded underlying assets and give investors access to not only home markets but also foreign markets as well. While mAAPL tries to closely represent the movements of AAPL stock, users are not afforded any rights of the underlying asset and tracking errors may arise due to the imbalances in trading volume in the underlying markets and the Terraswap markets.

#### 2. How are mAssets actually traded?

mAssets are traded through interacting with liquidity pools on Terraswap. For more information about the mechanism of the Terraswap, please see [here](https://terraswap.io).

#### 3. Do I have to go through the KYC process?

Mirror aims to be decentralized in all aspects including whitelisting, governance, minting, and trading. As a result, as long as you have UST balance, you are able to perform all functions available on both the Mirror protocol as well as Mirror protocol-owned Terraswap pools without any need to go through a KYC process.

#### 4. How are corporate actions and dividends handled?

Corporate actions are handled through an asset migration process discussed [here](protocol/mirrored-assets-massets.md#deprecation-and-migration). Given that Mirrored assets do not confer any rights of the underlying asset, Mirrored assets do not give dividends.

#### 5. What are the trading commissions composed of?

There is a fixed fee called the LP commission is 0.30% which serves as a reward for liquidity providers for Mirror-related pools on Terraswap. More detailed information can be found [here](protocol/terraswap.md).

#### 6. What does it mean to mint an mAsset?

All mAssets that are purchased or sold on Mirror were, at one point, minted. Minting is the process of providing collateral to issue a “synthetic” mAsset.

Price oracles play an important role in the minting process and are used for two key functions: First, they help determine the amount of collateral required for minting an mAsset. Second, they help determine whether sufficient collateral is backing existing mAssets.

In the below example (Figure 1), assume that a minter provided $150 worth of stablecoin to issue an mAsset worth $90 and that the minimum collateral ratio (MCR) is 150%. If at time T=2, the asset’s value increases to $101, then the collateral ratio would be 149% ($101/$150) and would fall below the MCR.

When this happens, the Mirror protocol will seize a portion of the collateral and initiate an auction for anyone willing to sell the mAsset in exchange. To incentivize this liquidation, the Mirror protocol allows anyone to purchase this seized collateral at a discount until the collateral ratio reaches the MCR again. In the example, using the collateral supplied at T=0, users will be able to send mAsset tokens in exchange for discounted collateral until the collateral ratio reaches 150% again at T=2.01. If, for instance, the asset price increases again at T=3, then the process repeats itself until the collateral ratio reaches 150%.

![Minting Example](https://github.com/wengzilla/docs/raw/master/images/faq\_minting\_example.png)

_Figure 1: When the minted asset’s price rises and the collateral ratio falls below the minimum collateral ratio, the protocol will sell collateral to buy back shares of the minted asset to burn. (_[_Link to calculations_](https://docs.google.com/spreadsheets/d/1RUlBliHX-AnigSieF4jC15xhG\_gGSnfTNz7g4mkHV7w/edit#gid=0)_)_

#### 7. What hours can I trade and mint mAssets?

mAsset liquidity is provided directly through the Terraswap liquidity pools, and as such, they can be traded irrespective of market hours.

Unlike trading, the oracle feeder is used to price the value of mAsset for minting. The oracle feeder stops operating when real-world market hours are closed, so minting transactions on Mirror Protocol will fail.

#### 8. How do mAssets keep their peg to real assets?

mAssets are soft pegged to the oracle price, which means that the Mirror protocol does not directly rely on price oracles to determine the trading prices of mAssets. Instead, Mirror relies on a combination of the minting liquidation process, arbitrageurs, and governance changes to keep mAsset prices close to oracle prices.

**Minting Liquidation**

As the price of an asset XXX rises on the NASDAQ, minted mXXX may fall below the minimum collateral ratio (MCR) and trigger a liquidation event. When that happens, the Mirror protocol will automatically sell collateral to buy mXXX until the collateral ratio reaches the MCR again. This buying pressure created for mXXX will drive prices higher and will help the price of mXXX converge with the price on the NASDAQ.

**Arbitrageurs**

If the price of XXX on the NASDAQ were $1000, but the price of mXXX on Mirror were $900, an arbitrageur would buy the asset with the assumption that in the near future, with enough buying pressure, the mXXX price would eventually converge to the NASDAQ price of $1000. At that point, the arbitrageur would then sell mXXX at $1000, taking a profit of $100 per share. Similarly, if the price of XXX on the NASDAQ were $1000, but the price of mXXX on Mirror were $1100, an arbitrageur would provide collateral, mint the mXXX asset, and sell it at $1100 with the assumption that in the near future, with enough selling pressure, the mXXX price would eventually converge to the NASDAQ price of $1000. At that point, the arbitrageur would then repurchase mXXX at $1000 and then burn it to regain their collateral, taking a profit of $100 per share.

**Governance**

Without the trust that mAssets should be pegged to oracle prices, mAsset prices could theoretically diverge from oracle prices. Unlike minting liquidation which can create buying pressure to drive prices upwards, the Mirror protocol can only drive downward pressure to the minimum collateral price of an mAsset. For instance, if the MCR of an asset is 150% and the oracle price of XXX is $100, then the price of an mXXX could, in theory, reach $150. However, once the price of mXXX is greater than $150, then arbitrageurs can simply mint mXXX with $150 worth of collateral, sell the mXXX for $160, and forego the collateral. Therefore, the theoretical maximum price of mXXX would be $150. If, in practice, mAsset prices did drift significantly higher than oracle prices, governance could be used to solve this by creating incentives to mint and sell assets. For instance, the MCR could be lowered in order to reign in mAsset prices or negative selling fees could be used to incentivize users to mint assets and sell them in the market. Changing governance, however, would require a proposal and the collective agreement of MIR stakers.

#### 9. What are the benefits of providing liquidity?

Providing liquidity for the Mirror Protocol is equivalent to locking up your liquidity in Terraswap. By doing so, you ensure that there is a sufficient supply of assets to be traded at any point in time. As compensation for providing liquidity, you will receive LP tokens which accrue trading commission charged by the protocol. In addition, staking these LP tokens provides inflationary rewards in the form of MIR tokens. To learn more about the specifics, see [here](broken-reference).

#### 10. Is there any risk to providing liquidity to the Terraswap pools?

You are able to remove your liquidity provided at any point in time. While there is no risk of losing any of your liquidity in most circumstances, in the case of large changes in price of the tokens provided, the return on providing liquidity may be less than the absolute price variation (known as _impermanent loss_). This is the [same risk](https://uniswap.org/docs/v2/advanced-topics/understanding-returns/) faced by liquidity providers on Uniswap.

#### 11. What can we do with Mirror tokens?

MIR tokens have a variety of functions on the Mirror Protocol. The first and foremost functionality of the token allows holders to participate in [governance](protocol/governance/) on the protocol. In addition, by providing liquidity to the MIR token pool, MIR LP tokens issued can be further staked in order to receive CDP withdrawal fees in the form of MIR tokens. Learn more [here](protocol/mirror-token-mir.md).

#### 12. What is the MIR Token (MIR) distribution and vesting schedule?

See [here](protocol/mirror-token-mir.md#cumulative-distribution-schedule-in-millions).

#### 13. Have the Mirror Protocol smart contracts been audited?

Yes, you can find the audits reports [here](security.md#audits).



_Special thanks to _[_wengzilla_](https://github.com/wengzilla)_ for edits and improvements to the FAQ._
