# Mirrored Assets (mAssets)

mAssets are blockchain tokens that behave like "mirror" versions of real-world assets by reflecting the exchange prices on-chain. They give traders the price exposure to real assets while enabling fractional ownership, open access and censorship resistance as any other cryptocurrency.

{% hint style="info" %}
Unlike traditional tokens which serve to represent a real, underlying asset, mAssets are purely synthetic and only capture the price movement of the corresponding asset.
{% endhint %}

## Properties

An mAsset can be described by the following properties:

### Name & Symbol

Describes the underlying asset the mAsset is supposed to track.

### Minimum Collateral Ratio & Multiplier

A CDP that mints the mAsset cannot have a collateral ratio below **minimum collateral ratio** times the **multiplier** parameter, lest it be subject to liquidation through auction. A multiplier is a parameter assigned to each asset type that could be used as collateral to open a CDP and is multiplied to the minimum collateral ratio of the minted mAsset to determine the final minimum collateral ratio for the given position.

### Auction Discount Rate

For a CDP subject to liquidation, describes the discount for which its collateral can be purchased by paying the minted amount for the position. Auction discount applied during the liquidation process is the lower value between _Minimum collateral ratio - 1, or `auction_discount` parameter._

### Price

The current registered price as reported by its Oracle Feeder. This is mainly used for determining collateral ratio for CDP and does not affect the mAsset's trading price on Terraswap directly.

Prices are only considered valid for 60 seconds. If no new prices are published after the data has expired, Mirror will disable CDP operations like mint, burn, deposit and withdraw until the price feed resumes.

For instance, the price feed is halted when real-world markets for the asset are closed. The market hour used to track the price of mAssets is based on [Nasdaq trading hours](https://www.nasdaq.com/stock-market-trading-hours-for-nasdaq). This does not affect the ability to trade on the asset's Terraswap pool.

### Oracle Feeder

The **Oracle Feeder** is a Terra account that can change the registered on-chain price for an mAsset, whitelisted collateral, and staking reward distribution between LP and sLP based on current price premium of mAssets. They are responsible for reporting an accurate and up-to-date price, so that the mAsset's trading value is kept in sync with its reflected asset. Each mAsset has its own dedicated feeder, which can be reassigned through governance. The oracle feeders for the genesis mirrored assets are owned by [Band Protocol](https://www.bandprotocol.com), but the community can (re-)assign oracle feeders to other providers to newly whitelisted assets through governance.

## Lifecycle

### Whitelisting

To **whitelist** an mAsset is to register it with Mirror Protocol, which involves several operations, including:

* creating the mAsset token and assigning its oracle feeder
* creating the mAsset-UST trading pair on Terraswap and its LP token
* registering the new mAsset with all relevant Mirror Contracts

Whitelisting is approved by [governance](governance/whitelist-procedure.md) and is automatically implemented if the whitelisting poll receives enough votes. Once an mAsset has been whitelisted, it will be mintable through opening a CDP and tradeable on Terraswap. In addition, LP tokens for the corresponding Terraswap pool will begin to earn MIR inflation rewards when staked.

### Delisting & Migration

In situations where the tracked asset undergoes a corporate event such as a stock split, merger, bankruptcy, etc. and becomes difficult to reflect properly due to inconsistencies, an mAsset can be **delisted**, or discontinued, with the following migration procedure initiated by the oracle feeder:

1. New replacement mAsset token, Terraswap pair, and LP tokens contracts are created, and the present values of properties of mAsset will be transferred over
2. The oracle feeder sets the `end_price` for the mAsset to the latest valid price
3. The mAsset's min. collateral ratio is set to 100%

At this stage:

* CDPs may no longer mint new tokens of the mAsset
* Liquidation auctions are disabled for the mAsset
* Burns will take effect at the fixed `end_price` for withdrawing collateral from any existing mint position.&#x20;
* LP & sLP tokens for the mAsset will stop counting for staking rewards

Delisting will not directly affect the functionality of the mAsset's Terraswap pool and users will still be able to make trades against it, although price is likely to be very unstable (and the [Web App](../user-guide/getting-started/) will not provide front-end interface for trading of delisted mAsset). Users are urged to burn the mAsset to recover collateral from any open positions on Mirror Protocol, including their own.

Since anyone can burn against any open position, CDP holders may end up having no or less amount of "borrowed assets" within their position, but they will still be able to withdraw the remaining amount of collateral by only burning the remaining amount of delisted mAsset. Opening a new CDP / engaging in liquidity provision can be done with the new, replacement mAsset.

The old mAsset will be retired and marked as `delisted` only allowing burn, close CDP, withdraw collateral and liquidity, and unstake LP transactions on front-end interfaces.

### Pre-IPO

When an IPO schedule and price quote has been announced, a new mAsset can be whitelisted as a **pre-IPO** mAsset to be minted and traded on Mirror Protocol upon passing of a [governance poll](governance/pre-ipo-procedure.md). Whitelisting of pre-IPO mAsset is different from a regular whitelisting (explained above) operation in the following manner:

* Creating the pre-IPO token and assigning a fixed price to be fed during the mint period
* Assigning a mint period that allows minting of pre-IPO mAssets within its duration
* Assigning a collateral ratio to be only effective during the mint period
* When mint period ends, pre-IPO assets will no longer be mintable, until the IPO event

When IPO occurs,

* [Oracle](broken-reference) triggers the IPO event on Mirror Protocol
* The asset becomes mintable again at the oracle price of the underlying asset
* A new collateral ratio, defined by the Pre-IPO poll proposer is assigned

## Collateralized Debt Position

New tokens for a listed mAsset can be minted by creating a **collateralized debt position** (CDP) with either TerraUSD (UST), mAsset or whitelisted collateral tokens as collateral. Also, mAssets can be directly shorted upon opening a CDP to mint sLP tokens. The CDP is essentially a short position against the price movement of the reflected asset, -- i.e. if the stock price of AAPL rises, minters of mAAPL would be pressured to deposit more collateral to maintain the same collateral ratio.

### Collateral

Mirror Protocol accepts the following types of tokens as collateral:

* UST
* All mAssets
* Other collateral: LUNA, LUNA X, aUST

Each type of asset listed above has a different `multiplier` ($$\zeta$$) which is multiplied to the minimum collateral ratio of each minted mAsset.

| Asset          | Multiplier |
| -------------- | ---------- |
| MIR (Delisted) | 1.3333334  |
| ANC (Delisted) | 1.3333334  |
| LUNA           | 1.3333334  |
| aUST           | 1          |
| LUNA X         | 1.3333334  |

For example, if LUNA, which has a collateral premium of 1.333334, is used as collateral to mint mAAPL, which has a minimum collateral ratio of 150%, then the minimum tolerated collateral ratio for this position will be $$1.3333334\times150\% \approx 200\%$$. Any price change of either LUNA or mAAPL which causes collateral ratio drop to below 200% will lead to liquidation auction.

### Collateral Ratio

The **collateral ratio** (C-ratio) is simply the ratio of the value of a CDP's locked collateral to the value of its current minted tokens.

The CDP is required to always maintain a C-ratio above the position's minimum, otherwise the protocol will initiate a margin call to liquidate collateral. The protocol is able to determine whether a position is underneath the required threshold by re-denominating all mAsset values into UST via their oracle-reported prices.

Let the mAsset's minimum C-ratio and collateral's multiplier be each $$r_{\text{min}}$$and $$\zeta$$. Given a CDP's current quantities of collateral and minted mAssets $$Q_c,Q_m$$ and their current prices $$P_c, P_m$$, the effective collateral ratio at any time $$t$$ is:

$$
r_t = \frac{P_cQ_c}{P_mQ_m}
$$

A CDP should strive to always maintain $$r_{t} \ge \zeta r_{\text{min}}$$, otherwise the collateral will be subject to [liquidation](mirrored-assets-massets.md#liquidation-and-auction).

#### Opening a new position

Users are allowed to set the initial C-ratio for their CDPs as long as it meets or exceeds the mandated minimum value for each position. The selection of the initial C-ratio $$r_{0}$$ along with the choice of collateral is used to determine how many tokens are minted during the creation of a CDP.

$$
Q_{m} = \frac{P_{c}Q_{c}}{r_{0}P_{m}}
$$

#### Depositing / withdrawing collateral to position

With an existing CDP, the user can deposit additional collateral $$Q_c'$$ to raise its effective C-ratio. The total amount of potential mintable mAsset tokens is then increased by the marginal value $$Q_m'$$.

$$Q_c'$$ can be negative, which is equivalent to withdrawing collateral. The user can only withdraw up to however much is needed to maintain the mAsset's effective C-ratio above the $$\zeta r_{\text{min}}$$. The user will receive $$Q_c' - \text{fee}_{\text{protocol}}$$ upon withdrawal due to the [protocol fee](mirrored-assets-massets.md#protocol-fee).

$$
Q_m + Q_{m}' = \frac{P_{c}(Q_{c}+Q_{c}')}{\zeta r_\text{min}P_{m}}
$$

#### Minting / Burning mAssets

In addition to depositing and withdrawal collateral, the user can also mint and burn mAssets against the CDP to adjust the value of their CDP's effective C-ratio.

Let the quantity of newly minted tokens be $$Q_m'$$(negative if burned). The minimum collateral required to keep the CDP position above the mAsset's min. collateral ratio is:

$$
Q_c \ge \frac{\zeta r_\text{min} P_m (Q_m+Q_m')}{P_c}
$$

Anything above that amount can be withdrawn from the CDP.

#### Closing a position

If a user wishes to collect all their collateral from their CDP, they must close their position by returning the outstanding balance of minted mAssets, which the protocol will burn. A user must first hold the corresponding amount of mAsset to close CDP (any sLP tokens minted are automatically burned when mAssets are returned). The user will then be able to withdraw their locked collateral minus the protocol fee.

### Protocol Fee

1.5% Mirror **protocol fee** is charged whenever a withdrawal from a CDP is made (including position closure and liquidation auction). The protocol fee is calculated based on the value of the mAsset at the burning of minted asset. This fee is then sent to the [Collector](../contracts/collector.md) contract, converted into MIR through Terraswap and distributed to MIR token stakers as a staking reward.

### Margin Call & Auction

Maintaining a lower C-ratio allows you to mint more mAsset tokens for less collateral, but obviously does not come without its risks. A CDP can be **margin called** when it falls below the min. collateral ratio. At this stage, if the owner does not quickly act and deposit more collateral or burn mAssets to deleverage their position, other users may purchase their CDP's collateral at a discount.

The protocol will try to raise the CDP's C-ratio by burning mAssets it recovers from liquidating its collateral. Let $$a$$ be the amount of the mAssets paid (up to the amount minted by the CDP) and $$d$$ be the mAsset's **auction discount rate.** The buyer can expect to receive:

$$
\min\bigg(\frac{a}{1-d}\times\frac{P_m}{P_c},Q_c\bigg)
$$

The remainder of the collateral not sold is returned to the CDP's owner.

The auction process continues until either the CDP's C-ratio is restored to a level above the mAsset's minimum collateral ratio OR the quantity of minted mAssets is completely burned, which closes the position. Because this provides almost risk-free profit, participants are incentivized to liquidate the entire margin-called position when possible to maximize their profits.

#### Example

To illustrate, let mXXX be priced at 1 UST, and mYYY at 2 UST. Assume that both assets have a min. collateral ratio of 150% and an auction discount rate of 20%. A user has opened a CDP to mint 100 mXXX at the C-ratio of 150%, depositing 75 mYYY as collateral.

If the CDP is being liquidated with the auction participant paying 100 mXXX to totally liquidate the position, one should receive:

$$
\min\bigg(\frac{100\text{mXXX}}{ 1-0.2}\times\frac{\text{1UST/mXXX}}{2\text{UST/mYYY}},75\text{mYYY}\bigg)
$$

Working over the math, one should receive 62.5 mYYY tokens, totaling 125 UST, making a 25% profit. The CDP owner would receive the remaining 12.5 mYYY.

Note that the owner would still retain his 100 mXXX, meaning along with the 12.5 mYYY they would keep around 125 UST of value out of their initial 150 UST deposit.

To avoid liquidation, users should aim for a C-ratio that factors in the known price dynamics of the reflected asset. A safety buffer of at least 50% above the mAsset's minimum is usually recommended. Users with open positions should actively monitor price activity that threaten the safety of their CDP and respond accordingly either by burning mAssets (or closing the position altogether), or deposit more collateral to reduce the possibility of liquidation.
