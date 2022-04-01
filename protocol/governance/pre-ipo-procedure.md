# Pre-IPO Procedure

{% hint style="info" %}
Pre-IPO asset whitelisting discussion can be started at the [Mirror Protocol Forum](https://forum.mirror.finance).
{% endhint %}

To enable minting of Pre-IPO asset, two separate governance polls must be passed. Pre-IPO asset whitelisting process consists of the steps below:&#x20;

1. The user should create **Suggest Pre-IPO Asset** poll. The poll should provide required details and supporting information for the voters to be able to make an informed decision. An example would be to utilize the external link to provide further information about the proposal in the [Mirror Protocol Forum](https://forum.mirror.finance). Information included could potentially include an [official registration statement](https://www.sec.gov/Archives/edgar/data/1679788/000162828021003168/coinbaseglobalincs-1.htm) of the IPO asset.&#x20;
2. Once the poll above is completed and passed, a subsequent **Register Pre-IPO Parameters** should be created with oracle provider (from set of whitelisted oracle providers), pre-IPO asset parameters and post-IPO asset parameters which will be effective immediately after the IPO event.&#x20;
3. If the second poll passes, Pre-IPO asset will be listed after `effective_delay`, and the asset will enter `mint_period`. During `mint_period`, Pre-IPO asset will be mintable at the submitted price, using `mint_collateral_ratio`, tradable and providable to liquidity pools.&#x20;
4. When the underlying asset is publicly listed, Mirror Oracle will start updating the price of mAsset to mimic the real-world asset's price. This mAsset will now be treated like any other mAsset, allowing trading, minting and liquidity provision.&#x20;
