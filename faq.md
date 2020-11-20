# FAQ

## General

### 1. What are Mirror synthetic assets?

The aim of mAssets is to mimic the price trends of real-world exchange-traded underlying assets and give investors access to not only home markets but also foreign markets as well. While mAAPL tries to closely represent the movements of AAPL stock, users are not afforded any rights of the underlying asset and tracking errors may arise due to the imbalances in trading volume in the underlying markets and the Terraswap markets.

### 2. How are mAssets actually traded?

mAssets are traded through interacting with liquidity pools on Terraswap. For more information about the mechanism of the Terraswap, please see [here](https://terraswap.io).

### 3. Do I have to go through the KYC process?

Mirror aims to be decentralized in all aspects including whitelisting, governance, minting, and trading. As a result, as long as you have UST balance, you are able to perform all functions available on both the Mirror protocol as well as Mirror protocol-owned Terraswap pools without any need to go through a KYC process.

### 4. How are corporate actions and dividends handled?

Corporate actions are handled through an asset migration process discussed [here](protocol/mirrored-assets-massets.md#deprecation-and-migration). Given that Mirrored assets do not confer any rights of the underlying asset, Mirrored assets do not give dividends.

### 5. What are the trading commissions composed of?

There is a fixed fee called the LP commission is 0.30% which serves as a reward for liquidity providers for Mirror-related pools on Terraswap. More detailed information can be found [here](protocol/terraswap.md#trading-fees).

### 6. What hours can I trade and mint mAssets?

mAsset liquidity is provided directly through the Terraswap liquidity pools, and as such, they can be traded irrespective of market hours.

Unlike trading, the oracle feeder is used to price the value of mAsset for minting. The oracle feeder stops operating when real-world market hours are closed, so minting transactions on Mirror Protocol will fail.

### 7. How do mAssets keep their peg to real assets?

The Mirror Protocol's mechanism and incentive designs creates several forces in play:

* mAssets must be covered by more collateral than their value, which limits minting
* collateral can be liquidated when positions go under min. collateral ratio
* liquidity providers ensure mAsset can be swapped against UST at market price
* arbitrage with external market makers maintain the peg

### 8. What are the benefits of providing liquidity?

Providing liquidity for the Mirror Protocol is equivalent to locking up your liquidity in Terraswap. By doing so, you ensure that there is a sufficient supply of assets to be traded at any point in time. As compensation for providing liquidity, you will receive LP tokens which accrue trading commission charged by the protocol. In addition, staking these LP tokens provides inflationary rewards in the form of MIR tokens. To learn more about the specifics, see [here](protocol/lp-token.md#lp-commission-rewards).

### 9. Is there any risk to providing liquidity to the Terraswap pools?

You are able to remove your liquidity provided at any point in time. While there is no risk of losing any of your liquidity in most circumstances, in the case of large changes in price of the tokens provided, the return on providing liquidity may be less than the absolute price variation \(known as _impermanent loss_\). This is the [same risk](https://uniswap.org/docs/v2/advanced-topics/understanding-returns/) faced by liquidity providers on Uniswap.

### 10. What can we do with Mirror tokens?

MIR tokens have a variety of functions on the Mirror Protocol. The first and foremost functionality of the token allows holders to participate in [governance](protocol/governance.md#mirror-token) on the protocol. In addition, by providing liquidity to the MIR token pool, MIR LP tokens issued can be further staked in order to receive CDP withdrawal fees in the form of MIR tokens. Learn more [here](protocol/mirror-token-mir.md).

### 11. What is the MIR Token \(MIR\) distribution and vesting schedule?

See [here](protocol/mirror-token-mir.md#mirror-token-supply).

### 12. Have the Mirror Protocol smart contracts been audited?

Yes, you can find the audits reports [here](security.md#audits).

