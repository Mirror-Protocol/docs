# Borrow

User can **Borrow** newly minted mAssets by providing any collateral accepted in Mirror Protocol and open a [CDP](../../protocol/mirrored-assets-massets.md#collateralized-debt-position). After creating a position, user can manage it on My Page.

When the user withdraws collateral either to manage position's collateral ratio or to close the position, a[ Protocol Fee](../../protocol/mirrored-assets-massets.md#protocol-fee) of 1.5% is charged to the collateral being withdrawn.

Please note that all asset values are calculated based on the reported price from the [oracle feeder ](../../protocol/mirrored-assets-massets.md#oracle-feeder)instead of the Terraswap price.

## Borrow (Open Position)

User may mint new mAsset that is whitelisted on Mirror Protocol, by providing other mAssets , UST or [other tokens](../../protocol/mirrored-assets-massets.md#collateral) as collateral and open a new CDP. The value of user's collateral against the minted asset must be higher than the [minimum collateral ratio](../../protocol/mirrored-assets-massets.md#minimum-collateral-ratio) in order to have the transaction successfully executed.

1\. Navigate to **Borrow (Mint)** page and select an asset to borrow.&#x20;

2\. Select an asset to use as [collateral](../../protocol/mirrored-assets-massets.md#collateral).

![](<../../.gitbook/assets/image (170).png>)

3\. Select an asset to borrow (mint).

![](<../../.gitbook/assets/image (165).png>)

4\. Enter value in `Collateral` and set the `Collateral ratio`.&#x20;

![](<../../.gitbook/assets/image (214).png>)

5\. Click `OPEN`. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

6\. All Done!

![](<../../.gitbook/assets/image (189).png>)

Now, the borrow position becomes visible on My Page. User may choose the same asset pairs to mint more, but due to changed oracle price of both the collateral and the minted asset, and a different collateral ratio, a completely new CDP will be created instead of combining the values with the previously minted position. These two positions will be identifiable by a position ID, which is displayed as a number.

## Close Position

Any user with an open borrow or [short position ](pool.md#short)can always choose to close the position, by “burning” the same type and amount of Mirrored Asset corresponding to an open position. When the minted assets are burned, users can reclaim all of the collaterals that were locked in the position.

Please note that user cannot close the position if the user holds borrowed asset of any amount lower than the amount minted to open a position. User must first obtain the same or a greater amount of minted assets by buying them on the [Market](trade.md) page or have them sent from a different wallet.

1\. Navigate to **My Page** by clicking on the wallet icon.

![](<../../.gitbook/assets/image (168).png>)

2\. In the **Borrowing** section, press the "`Manage`" under actions

![](<../../.gitbook/assets/image (145).png>)

3\. Select `Close Position` and confirm details

{% hint style="warning" %}
If you do not hold an amount greater than or equal to `Burn Amount` displayed on this page, you must first navigate to **Market** page to buy the corresponding amount.&#x20;
{% endhint %}

![](<../../.gitbook/assets/image (167).png>)

4\. Click `CLOSE`. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

## Manage Position

Users are warned when their CDP is close to liquidation. Borrow / short positions become vulnerable to liquidation when the collateral ratio falls below the `min_collateral_ratio`. Vice versa, borrow / short positions can be over-collateralized when the value of collateral to minted assets increases by a large margin. To avoid liquidation or over-collateralization, the user can borrow more or burn assets, deposit or withdraw collateral or change both amounts of borrowed and collateral.&#x20;

1\. Navigate to [**My** **Page**](https://terra.mirror.finance/my)****

2\. In the **Borrowings** section, press the "`Manage`"under actions

![](<../../.gitbook/assets/image (145).png>)

3\. Select "`Edit` "

![](<../../.gitbook/assets/image (219).png>)

4\. Users may do the following actions on the page by entering a value greater or less than the default value in:

* `Collateral` field to **deposit** or **withdraw** collateral from the position
* `Borrowed` field to **borrow more** or **burn** mAssets from the position

or change both fields to manage the position in a more flexible manner. Changing `Collateral Ratio` will only change the value in `Collateral` field.

![](<../../.gitbook/assets/image (218).png>)

5\. Click the activated button to confirm. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

6\. All done!

![](<../../.gitbook/assets/image (217).png>)
