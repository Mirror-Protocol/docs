# Trade

The **Market** page provides an interface for trading and borrowing of mAsset tokens. **Trades** are made against the asset's Terraswap pool and priced algorithmically by its [automated market-making algorithm](../../protocol/terraswap.md#pricing).&#x20;

## Trade

{% hint style="info" %}
The Trade interface is directly connected to [Terraswap](../../protocol/terraswap.md).
{% endhint %}

1\. Navigate to the **Market** page. Select or search for an asset to trade.

![](<../../.gitbook/assets/image (208).png>)

2\. At the top of the page, select one of **Buy or Sell.**

![](<../../.gitbook/assets/image (188).png>)

3\. Enter the amount to buy or sell, and check your slippage tolerance and simulated values. If everything is okay, click **Buy** or **Sell.**

{% hint style="danger" %}
**Slippage tolerance** is the maximum deviation from the Expected Price. Transaction will be reverted if the effective price is different from the expected price by more than slippage tolerance.&#x20;
{% endhint %}

![](<../../.gitbook/assets/image (212).png>)

4\. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

## Limit Order

Limit Order enables users to buy or sell an asset when the price reaches the Bid or Ask price. When user's order is submitted, user's offered mAsset or UST will be locked until the order is executed at Bid/Ask Price or canceled by the user.

1\. On **Buy** or **Sell** page and click on Limit Order toggle.&#x20;

![](<../../.gitbook/assets/image (210).png>)

2\. Select an asset to buy or sell.

![](<../../.gitbook/assets/image (185).png>)

3\. When entering the below information, `Order Value` will be automatically calculated:

* **Bid/Ask Price:** The desired price of an mAsset decided by the user. When the Terraswap price of the asset exceeds the Bid/Ask Price by slippage and transaction fee amount, user's order can be executed.
* **Order Amount:** Quantity of the mAsset to trade at bid/ask price.

![](<../../.gitbook/assets/image (191).png>)

4\. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

**To cancel Limit Order:**

Limit Order is visible on **My Page**. Users can select Cancel from Actions column to remove the submitted Limit Order.

_Please note that your Limit Order can be executed when Terraswap price of the mAsset **exceeds** the Limit Price due to slippage cause by size of your transaction against the liquidity pool._&#x20;
