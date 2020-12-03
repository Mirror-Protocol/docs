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

_The amount shown on the page is an estimate based on the observed pool ratio at time of order. You may actually receive a different amount of tokens due to changes in pool ratio between signing and broadcasting the transaction._

4. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%289%29.png)

5. All done!

![](../../.gitbook/assets/image%20%2814%29.png)

**Special cases:**

Trading transactions may not complete transaction will not be completed under following circumstances:

* If the price changes by more than 1% compared to estimated, the protocol will be prevent the transaction in order to protect the user from executing at an unexpected price.
* If the order is exceeds balance of pool, the transaction will fail. 

