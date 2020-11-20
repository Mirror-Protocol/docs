# \*\*Mirror Token \(MIR\)

The Mirror Token \(MIR\) is Mirror Protocol's governance token. Currently, it must be staked to vote on active polls and is required as a deposit for making new governance polls. In future iterations of Mirror, it will serve further purposes for the protocol that increase its utility and value.

Users that stake MIR tokens also earn MIR rewards generated from trading fees within the protocol.

## **\*\*Mirror Token Supply**

### Initial Distribution

&lt;TO BE ADDED&gt;

### Inflation

The Mirror Protocol mints new MIR tokens every block according to the `mint_per_block` inflation rate, adjustable through governance. These MIR tokens are distributed to LP stakers, as discussed [here](lp-token.md#from-staking).

## Staking Rewards

{% hint style="info" %}
This section discusses staking rewards for MIR tokens, which come from trading fees used to buy back MIR from the market. Staking LP tokens also generates MIR rewards, which come directly from new MIR tokens created every block. Learn more about it [here](lp-token.md#from-staking).
{% endhint %}

### From Withdrawing Collateral

All tradable mAssets on Mirror Protocol are minted by opening a CDP with UST or mAsset collateral. When the user withdraws the collateral to close or adjust the collateral ratio for the position, a **Withdraw Fee** occurs. Withdraw Fee that is collected in different denominations are sold for UST through Terraswap in order to purchase MIR from the MIR-UST market. The purhcased MIR are then distributed as rewards to MIR stakers in proportion to the percentage of total stake. This process balances the generation of new MIR by creating a buying pressure.

### From Poll Creation Fees

Whenever a new governance poll is created, an initial deposit of MIR tokens must be paid. This deposit is distributed to all MIR stakers proportionately.

