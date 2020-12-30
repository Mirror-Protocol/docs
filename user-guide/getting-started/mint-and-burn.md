# Mint

The **Mint** page allows you to mint new tokens of an mAsset by opening a [CDP](../../protocol/mirrored-assets-massets.md#collateralized-debt-position). After creating a position, you can manage it on the [My page](https://terra.mirror.finance/my).

When the user withdraws collateral either to manage position's collateral ratio or to close the position, a [Protocol Fee](../../protocol/mirrored-assets-massets.md#protocol-fee) of 1.5% is charged to the amount being withdrawn.

Please note that all asset values are calculated based on the reported price from the [oracle feeder ](../../protocol/mirrored-assets-massets.md#oracle-feeder)instead of the Terraswap price.

## Mint \(Open Position\)

User may mint new mAsset that is whitelisted on Mirror Protocol, by providing other mAssets or UST as collateral and open a new CDP. The value of user's collateral against the minted asset must be higher than the [minimum collateral ratio](../../protocol/mirrored-assets-massets.md#minimum-collateral-ratio) in order to have the transaction successfully executed.

1. Navigate to the [**Mint**](https://terra.mirror.finance/mint) page

![](../../.gitbook/assets/image%20%2863%29.png)

2. Select an asset to use as collateral. You can use UST or another mAsset.

![](../../.gitbook/assets/image%20%2835%29.png)

3. Select the mAsset to mint

![](../../.gitbook/assets/image%20%2846%29.png)

4. Set the collateral ratio

![](../../.gitbook/assets/image%20%2836%29.png)

5. Click `OPEN`. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%2855%29.png)

6. All Done!

![](../../.gitbook/assets/image%20%2827%29.png)

Now, the minted position becomes visible on My Page. User may choose the same asset pairs to mint more, but due to changed oracle price of both the collateral and the minted asset, and a different collateral ratio, a completely new CDP will be created instead of combining the values with the previously minted position. These two positions will be identifiable by a position ID, which is displayed as a number.

## Close Position

Any user with an open mint position can always choose to close the position, by “burning” the same type and amount of Mirrored Asset corresponding to an open position. When the minted assets are burned, users can reclaim all of the collaterals that were locked in the position.

Please note that user cannot close the position if the user holds minted asset of any amount lower than the amount minted to open a position. User must first obtain the same or a greater amount of minted assets by buying them on “Trade” page or have it sent from a different wallet.

1. Navigate to [**My** **Page**](https://terra.mirror.finance/my)

![](../../.gitbook/assets/image%20%2815%29.png)

2. In the Mint section, press the "`...`" under actions

![](../../.gitbook/assets/image%20%2824%29.png)

3. Select `Close Position` and confirm details

![](../../.gitbook/assets/image%20%2848%29.png)

4. Click `CLOSE`. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%2833%29.png)

## Deposit / Withdrawal

Users are warned when their CDP is close to liquidation. Minted positions become vulnerable to liquidation when the collateral ratio falls below the `min_collateral_ratio`. Vice versa, minted positions can be over-collateralized when the value of collateral to minted assets increase by a large margin. To avoid liquidation or over-collateralization, user can deposit or withdraw locked collateral from the CDP.

1. Navigate to [**My** **Page**](https://terra.mirror.finance/my)\*\*\*\*

2. In the Mint section, press the "`...`"under actions

![](../../.gitbook/assets/image%20%2828%29.png)

3. Select "`Deposit` / `Withdraw`"

![](../../.gitbook/assets/image%20%2859%29.png)

4. Enter either amount to deposit/withdraw or collateral ratio.

![](../../.gitbook/assets/image%20%2856%29.png)

5. Click action button to confirm. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%2860%29.png)

