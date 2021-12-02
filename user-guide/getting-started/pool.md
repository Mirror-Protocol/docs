# Farm

The **Farm** page displays operations related to liquidity provision, short position creation and earning [LP and sLP token](../../protocol/staking-tokens-lp-and-slp.md) staking rewards.&#x20;

In order to create a short position, you need to lock collateral assets to Mirror Protocol, and mint new mAssets. Unlike Borrow positions, newly minted assets are immediately sold at Terraswap price for UST. UST returned for shorting mAssets will be locked for a defined `lock_period`. To close your short position, you must first hold shorted amount of mAssets. These mAssets will be burned to return your locked collateral.

## Farm Main

![](<../../.gitbook/assets/image (187).png>)

The main page of **Farm** provides a list of available staking pools and information corresponding to each pool.&#x20;

### **Ticker**&#x20;

Name and symbol of the mAsset associated with the staking pool

### **Long**

Link to LP Staking (Long Farm) page where users can provide liquidity for this asset and start earning MIR reward. The annual percentage rate for Long is calculated by,

$$\text{Long APR=}\text{(Annual MIR Long Reward} \times \text{MIR Price)}/\text{(Liquidity Value)}$$

where $$\text{Annual MIR Long Reward}$$ is the amount distributed to the corresponding mAsset's  long staking pool per year, $$\text{MIR Price}$$ is the Terraswap price of MIR token denominated in UST and $$\text{Liquidity Value}$$ is the UST value of total liquidity provided in this mAsset-UST liquidity pool.&#x20;

### **Short**

Link to short position creation (Short Farm) page where users can create a short CDP and start earning MIR reward. The annual percentage rate for Short is calculated by,

$$\text{Short APR = (Annual MIR Short Reward}\times\text{MIR Price)}/\text{(Shorted Token Amount}\times\text{Price)}$$

where $$\text{Annual MIR Short Reward}$$ is the amount distributed to the corresponding mAsset's short staking pool per year, $$\text{MIR Price}$$ is the Terraswap Price of MIR token denominated in UST, $$\text{Short Token Amount}$$ is the number of mAssets sold from all short position creation, and $$\text{Price}$$ is the Terraswap price of this mAsset token denominated in UST.&#x20;

### **Terraswap Price**

Current Terraswap pool ratio of the corresponding asset.&#x20;

### **Premium**

Difference ratio between Terraswap and Oracle price of the associated mAsset. The premium is positively correlated with Short reward. Read more about Short reward [here](../../protocol/staking-tokens-lp-and-slp.md#staking-rewards).

## Long Farm (LP)

In order to provide liquidity for Mirror assets, you need to supply an equal value of the asset and UST. Minted LP tokens will be immediately staked to start earning MIR rewards. By unstaking LP tokens, your liquidity provided will be reclaimable from the Terraswap pool. For more information about Terraswap pools, [click here](../../protocol/terraswap.md).

{% hint style="info" %}
The Pool interface is directly connected to [Terraswap](https://terraswap.io).
{% endhint %}

### Stake LP

1\. Navigate to the **Farm** page and select **Long Farm** for the asset to provide liquidity for.

![](<../../.gitbook/assets/image (166).png>)

2\. Enter the amount of selected asset. The App will calculate the amount of UST you will need to deposit as well.

![](<../../.gitbook/assets/image (200).png>)

3\. Press **`LONG`**. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

4\. The app will automatically stake the LP tokens minted from liquidity provision.&#x20;

![](<../../.gitbook/assets/image (142).png>)

### Unstake LP

1\. Navigate to **My Page** and select `Unstake` by the **Farming** position to unstake from.

![](<../../.gitbook/assets/image (146).png>)

2\. Enter the amount of LP to unstake and click `Unstake` You may click on your available balance to enter the `MAX` amount. The app will automatically calculate the expected returned amount from this transaction.&#x20;

![](<../../.gitbook/assets/image (133).png>)

3\. Press **`Unstake`**. Station Extension will prompt you to sign the transaction. Confirm details presented and enter password to sign.&#x20;

## Short Farm

In order to create a short position, you need to lock collateral assets to Mirror Protocol, and mint new mAssets. Unlike a regular borrow position, newly minted assets are immediately sold at Terraswap price for UST. UST returned for shorting mAssets will be locked for a defined `lock_period`.&#x20;

### Stake sLP

1 . Navigate to **Farm** page, and select **Short Farm** for the asset to create a short position for.

![](<../../.gitbook/assets/image (140).png>)

2\. Select an asset to provide as collateral and enter the amount.

![](<../../.gitbook/assets/image (161).png>)

3\. Set the collateral ratio. The app will automatically calculate the expected short amount.&#x20;

![](<../../.gitbook/assets/image (209).png>)

4\. Press **`Short`**. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

5\. All done! The app will automatically stake the sLP minted from this transaction.&#x20;

![](<../../.gitbook/assets/image (180).png>)

### Unstake sLP

{% hint style="warning" %}
To close your short position, you must first hold the shorted amount of mAssets. These mAssets will be burned to return your locked collateral.
{% endhint %}

1 . Navigate to **My Page**, and click on `Manage` button on **Borrowing** by the short position to close.

![](<../../.gitbook/assets/image (136).png>)

2\. Check the simulated values before executing the transaction.

![](<../../.gitbook/assets/image (184).png>)

3\. Press **`Close`**. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

### Claiming Locked UST

After a short position creation, UST from mAssets sold against the Terraswap pool is locked for a defined period called [Lock Period](../../protocol/staking-tokens-lp-and-slp.md#lock-period). When `lock_period` (initially 15 days) is over, users may claim their unlocked UST throug the steps below:&#x20;

1 . Navigate to **My Page** and go to **Farming** section

![](<../../.gitbook/assets/image (197).png>)

2\. Below **Short Farming** section, a list of locked UST per each short position is displayed. Click `Claim UST` to retrieve unlocked UST

![](<../../.gitbook/assets/image (201).png>)

3\. Check the amount to be retreived and press `Claim`&#x20;

![](<../../.gitbook/assets/image (157).png>)
