# Terraswap

A short primer on [Terraswap](https://terraswap.io) is provided here for convenience. For more details regarding Terraswap, you can refer to its official documentation [here](https://docs.terraswap.io).

Terraswap is a [Uniswap](https://uniswap.org)-inspired automated market maker \(AMM\) protocol implemented with smart contracts on the Terra blockchain. Mirror relies on Terraswap to establish UST trading pairs for mAssets and for the MIR token. This enables a decentralized on-chain exchange for the various assets involved in Mirror Protocol.

## Mechanism

### Liquidity Pools

Terraswap creates automated markets for pairs of tokens \(or native Terra coins like UST\) called **pools** which enable users to exchange one asset for the other directly on-chain. Pools maintain balances of both assets, to which users can provide liquidity in exchange for reward-bearing LP tokens. A more detailed explanation about LP tokens and their relationship with Mirror can be found [here](terraswap.md).

### Constant Product

Terraswap pools make prices based on a **constant product invariant.**

$$
XY=k \\
$$

The product of the number of tokens on each side of the pool should remain constant across trading operations \(buying / selling\). For Mirror-related pools, TerraUSD is on one side and mAsset/MIR tokens are on the other.

### Pricing

In order to preserve the constant product invariant, Terraswap will make prices that ensure the product of resultant balances of the pool is as close as possible to product calculated prior to the trade. With $$X$$ being the current balance of the pool's source asset and and $$Y$$ being that of the target asset:

$$
X Y = k =(X + A_{\text{in}})(Y - B_{\text{out}}) \\
$$

To determine the proper value of $$B_{\text{out}}$$ given the trader's offered asset $$A_{in}$$:

$$
B_{\text{out}} = \frac{YA_{\text{in}}}{X+A_{\text{in}}}
$$

Terraswap is able to execute trades with only the current balances of the pool and the number of incoming tokens. The market price is encoded the number of pool's target tokens divided by the source asset \(also called the pool ratio\). The spread between the executed and the expected trade is:

$$
\text{spread} = \max\bigg(\frac{YA_{\text{in}}}{X+A_{\text{in}}} - \frac{YA_{\text{in}}}{X}, 0\bigg)
$$

When a pool has large balances of tokens on both sides from liquidity providers, the spread becomes smaller and helps the pool execute closer to its reported price of $$\frac{Y}{X}$$.

## LP Commission

In order to compensate liquidity providers, Terraswap charges an **LP Commission** on each trade. The fee returns to the pool to serve as a reward for LP token holders and can only be withdrawn by burning LP tokens and reclaiming a portion of the pool.

In Mirror, each liquidity pool for mAssets/MIR has a fixed **LP commission** fee of 0.3%. This fee is levied on the trader and is received as mAssets/MIR or UST, depending on the direction of the trade.

{% hint style="info" %}
Additionally, if the traded asset is a native token such as UST, the Terra network will incur a tax on the transfer \(not controlled by Mirror Protocol\).
{% endhint %}

$$
\text{received} = B_{\text{out}}(1- \text{fee}_{\text{LP}} ) - \text{tax}
$$

