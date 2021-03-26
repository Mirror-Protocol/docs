# Trade

The **Trade** page provides an interface for buying and selling mAsset / MIR tokens against UST. Trades are made against the asset's Terraswap pool and are priced algorithmically by its [automated market making algorithm](../../protocol/terraswap.md#pricing). 

{% hint style="info" %}
The Trade interface is directly connected to [Terraswap](../../protocol/terraswap.md).
{% endhint %}

## Buy/Sell

1 . Navigate to the [Trade](https://terra.mirror.finance/trade) page. Select whether to **Buy** or **Sell**.

![](../../.gitbook/assets/image%20%2813%29.png)

2. Select asset to buy or sell.

![](../../.gitbook/assets/image%20%2811%29.png)

3. Enter amount to buy or sell. Then, click the button below to confirm.

![](../../.gitbook/assets/image%20%287%29.png)

{% hint style="warning" %}
The amount shown on the page is an estimate based on the observed pool ratio at time of order. You may actually receive a different amount of tokens due to changes in pool ratio between signing and broadcasting the transaction.
{% endhint %}

4. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%289%29.png)

5. All done!

![](../../.gitbook/assets/image%20%2814%29.png)

**Special cases:**

Trading transactions may not complete transaction will not be completed under following circumstances:

* If the price changes by more than 1% compared to estimated, the protocol will be prevent the transaction in order to protect the user from executing at an unexpected price.
* If the order is exceeds balance of pool, the transaction will fail. 

## Limit Order

Limit Order enables users to buy or sell an asset when the price reaches the Bid or Ask price. When user's order is submitted, user's offered mAsset or UST will be locked until the order is executed at Bid/Ask Price or canceled by the user.

1. Navigate to Trade \(**Buy** or **Sell**\) ****page and turn on Limit Order. 

![](../../.gitbook/assets/image%20%28112%29.png)

2. Select an asset to buy or sell.

![](../../.gitbook/assets/image%20%28113%29.png)

3. When entering the below information, `Order Value` will be automatically calculated:

* **Bid/Ask Price:** The desired price of an mAsset decided by the user. When the Terraswap price of the asset exceeds the Bid/Ask Price by slippage and transaction fee amount, user's order can be executed.
* **Order Amount:** Quantity of the mAsset to buy/sell at bid/ask price.

![](../../.gitbook/assets/image%20%28117%29.png)

4. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%28114%29.png)

**To cancel Limit Order:**

Limit Order is visible on **My Page**. Users can select Cancel from Actions column to remove the submitted Limit Order.

_Please note that your Limit Order can be executed when Terraswap price of the mAsset exceeds the Limit Price due to slippage cause by size of your transaction against the liquidity pool._ 

