# Modify Mint Parameters

The mechanics of a mint position on the Mirror Protocol is determined by two parameters,  `Auction Discount` and `Minimum Collateral Ratio`. At the genesis of the Mirror Protocol, these two parameters have been uniformly set to 20% and 150% respectively for all listed mAssets.&#x20;

However, the structure of financial markets of the underlying assets are not static, so it is not unreasonable to expect that the Mirror Protocol should also be able to adapt to these changes. To further account for the fact that mirrored assets come in a variety of classes, the two mint parameters can be voted separately for each individual asset.

The community is able to change the two parameters of an existing mAsset through a [**Modify Mint Parameters**](proposal-types.md#4-modify-mint-parameters) **** poll.
