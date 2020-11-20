# Mirrored Assets \(mAssets\)

mAssets are blockchain tokens that behave like "mirror" versions of real-world assets by reflecting the exchange prices on-chain. They give traders the price exposure to real assets while enabling fractional ownership, open access and censorship resistance as any other cryptocurrency.

{% hint style="warning" %}
Unlike traditional tokens which serve to represent a real, underlying asset, mAssets are purely synthetic and only capture the price movement of the corresponding asset.
{% endhint %}

## Properties

An mAsset can be described by the following properties:

### Name & Symbol

Describes the underlying asset the mAsset is supposed to track.

### Minimum Collateral Ratio

A CDP that mints the mAsset cannot have a collateral ratio below this value, lest it be subject to liquidation through auction.

### Auction Discount Rate

For a CDP subject to liquidation, describes the discount for which its collateral can be purchased.

### Price

The current registered price as reported by its Oracle Feeder. This is mainly used for determining collateral ratio for CDP and does not affect the mAsset's trading price on Terraswap directly.

Prices are only considered valid for 60 seconds. If no new prices are published after the data has expired, Mirror will disable CDP operations like mint, burn, deposit and withdraw until the price feed resumes. This does not affect the ability to trade on the asset's Terraswap pool.

For instance, the price feed is halted when real-world markets for the asset are closed.

### Oracle Feeder

The **Oracle Feeder** is a Terra account that can change the registered on-chain price for an mAsset. They are responsible for reporting an accurate and up-to-date price, so that the mAsset's trading value is kept in sync with its reflected asset. Each mAsset has its own dedicated feeder, which can be reassigned through governance. The oracle feeders for the genesis mirrored assets are owned by [Band Protocol](https://www.bandprotocol.com), but the community can assign oracle feeders to other providers to newly whitelisted assets through governance. 

## Lifecycle

### Whitelisting

To **whitelist** an mAsset is to register it with Mirror Protocol, which involves several operations, including:

* creating the mAsset token and assigning its oracle feeder
* creating the mAsset-UST trading pair on Terraswap and its LP token
* registering the new mAsset with all relevant Mirror Contracts

Whitelisting is approved by governance and is automatically implemented if the whitelisting poll receives enough votes. Once an mAsset has been whitelisted, it will be mintable through opening a CDP and tradeable on Terraswap. In addition, LP tokens for the corresponding Terraswap pool will begin to earn MIR inflation rewards when staked.

### Deprecation & Migration

In situations where the tracked asset undergoes a corporate event such as a stock split, merger, bankruptcy, etc. and becomes difficult to reflect properly due to inconsistencies, an mAsset can be **deprecated**, or discontinued, with the following migration procedure initiated by the oracle feeder:

1. New replacement mAsset token, Terraswap pair, and LP tokens contracts are created, and the present values of properties of mAsset will be transferred over
2. The oracle feeder sets the "end price" for the mAsset to the latest valid price
3. The mAsset's min. collateral ratio is set to 100%

At this stage:

* CDPs may no longer mint new tokens of the mAsset
* Liquidation auctions are disabled for the mAsset
* Burns will take effect at the fixed "end price" for withdrawing collateral deposits
* LP tokens for the mAsset will stop counting for staking rewards

Deprecation will not directly affect the functionality of the mAsset's Terraswap pool and users will still be able to make trades against it, although price is likely to be very unstable. Users are urged to burn the mAsset to recover their collateral if they have an open position, and are free to open a new CDP / engage in liquidity provision for the new, replacement mAsset. The old mAsset will be retired and marked as "deprecated" on front-end interfaces.

## Collateralized Debt Position

New tokens for a listed mAsset can be minted by creating a **collateralized debt position** \(CDP\) with either TerraUSD \(UST\) or other mAsset tokens as collateral. The CDP is essentially a short position against the price movement of the reflected asset, -- i.e. if the stock price of AAPL rises, minters of mAAPL would be pressured to deposit more collateral to maintain the same collateral ratio.

### Collateral Ratio

The **collateral ratio** \(C-ratio\) is simply the ratio of the value of a CDP's locked collateral to the value of its current minted tokens.

The CDP is required to always maintain a C-ratio above the mAsset's minimum, otherwise the protocol will initiate a margin call to liquidate collateral in an attempt to restore the position's C-ratio. The protocol is able to determine whether a position is underneath the required threshold by re-denominating all mAsset values into UST via their oracle-reported prices.

Let the mAsset's minimum C-ratio be $$r_{\text{min}}$$. Given a CDP's current quantities of collateral and minted mAssets $$Q_c,Q_m$$ and their current prices $$P_c, P_m$$, the effective collateral ratio at any time $$t$$ is:

$$
r_t = \frac{P_cQ_c}{P_mQ_m}
$$

A CDP should strive to always maintain $$r_{t} \ge r_{\text{min}}$$, otherwise the collateral will be subject to [liquidation](mirrored-assets-massets.md#liquidation-and-auction).

#### Opening a new position

Users are allowed to set the initial C-ratio for their CDPs as long as it meets or exceeds the mandated minimum value for each mAsset. The selection of the initial C-ratio $$r_{0}$$ along with the choice of collateral is used to determine how many tokens are minted during the creation of a CDP.

$$
Q_{m} = \frac{P_{c}Q_{c}}{r_{0}P_{m}}
$$

#### Depositing / withdrawing collateral to position

With an existing CDP, the user can deposit additional collateral $$Q_c'$$ to raise its effective C-ratio. The total amount of potential mintable mAsset tokens is then increased by the marginal value $$Q_m'$$.

$$Q_c'$$ can be negative, which is equivalent to withdrawing collateral. The user can only withdraw up to however much is needed to maintain the mAsset's effective C-ratio above the mAsset's minimum. The user will receive $$Q_c' - \text{fee}_{\text{protocol}}$$ upon withdrawal due to the [protocol fee](mirrored-assets-massets.md#protocol-fee).

$$
Q_m + Q_{m}' = \frac{P_{c}(Q_{c}+Q_{c}')}{r_\text{min}P_{m}}
$$

#### Minting / Burning mAssets

In addition to depositing and withdrawal collateral, the user can also mint and burn mAssets against the CDP to adjust the value of their CDP's effective C-ratio.

Let the quantity of newly minted tokens be $$Q_m'$$\(negative if burned\). The minimum collateral required to keep the CDP position above the mAsset's min. collateral ratio is:

$$
Q_c \ge \frac{r_\text{min} P_m (Q_m+Q_m')}{P_c}
$$

Anything above that amount can be withdrawn from the CDP.

#### Closing a position

If a user wishes to collect all their collateral from their CDP, they must close their position by returning the outstanding balance of minted mAssets, which the protocol will burn. The user will be then be able to withdraw their locked collateral.

### Protocol Fee

The Mirror **protocol fee** is charged whenever a withdrawal from a CDP is made \(including closing the position\). This fee is then converted into MIR through Terraswap and distributed to MIR token stakers as a staking reward.

### Margin Call & Auction

Maintaining a lower C-ratio allows you to mint more mAsset tokens for less collateral, but obviously does not come without its risks. A CDP can be **margin called** when it falls below the min. collateral ratio. At this stage, if the owner does not quickly act and deposit more collateral or burn mAssets to deleverage their position, other users may purchase their CDP's collateral at a discount.

The protocol will try to raise the CDP's C-ratio by burning mAssets it recovers from liquidating its collateral. Let $$a$$ be the amount of the mAssets paid \(up to the amount minted by the CDP\) and $$d$$ be the mAsset's **auction discount rate.** The buyer can expect to receive:

$$
\min\bigg(\frac{a}{1-d}\times\frac{P_m}{P_c},Q_c\bigg)
$$

The remainder of the collateral not sold is returned to the CDP's owner.

The auction process continues until either the CDP's C-ratio is restored to a level above the mAsset's minimum collateral ratio OR the quantity of minted mAssets is completely burned, which closes the position. Because this provides almost risk-free profit, participants are incentivized to liquidate the entire margin-called position when possible to maximize their profits.

#### Example

To illustrate, let mXXX be priced at 1 UST, and mYYY at 2 UST. Assume that both assets have a min. collateral ratio of 150% and an auction discount rate of 20%. A user has opened a CDP to mint 100 mXXX at the C-ratio of 150%, depositing 75 mYYY as collateral. 

If the CDP is being liquidated with the auction participant paying 100 mXXX to totally liquidate the position, they should receive:

$$
\min\bigg(\frac{100\text{mXXX}}{ 1-.2}\times\frac{\text{1UST/mXXX}}{2\text{UST/mYYY}},75\text{mYYY}\bigg)
$$

Working over the math, they should receive 62.5 mYYY tokens, totalling 125 UST, making a 25% profit. The CDP owner would receive the remaining 12.5 mYYY.

Note that the owner would still retain his 100 mXXX, meaning along with the 12.5 mYYY they would keep around 125 UST of value out of their initial 150 UST deposit.

To avoid liquidation, users should aim for C-ratio that factors in the known price dynamics of the reflected asset. A safety buffer of at least 50% above the mAsset's minimum is usually recommended. Users with open positions should actively monitor price activity that threaten the safety of their CDP, and respond accordingly either by burning mAssets \(or closing the position altogether\), or depositing more collateral to reduce the possibility of liquidation.

