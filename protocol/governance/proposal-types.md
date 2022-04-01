# Proposal Types

Governance proposals on the Mirror Protocol can come in a variety of formats. There are four main categories of governance proposals: **Asset Listing, Reward Distribution, Parameter Modification, and Suggestion / Others.**

![](<../../.gitbook/assets/image (12).png>)

## Asset Listing

### 1. Suggest Asset Whitelisting

{% hint style="info" %}
The full process of getting a new asset to be fully operational on the Mirror Protocol is described [here](whitelist-procedure.md).
{% endhint %}

The function of this proposal is to start a poll to whitelist a new asset. Given that this is a simple text proposal, there are a few fields that need to be filled. The `Asset Name`, `Ticker`, `Listed Exchange`, and `Reason for listing` are required. The length of the description field is limited to 1024 bytes. It is suggested that additional information be added on a separate web page and linked to using the `Information Link` field.

![](<../../.gitbook/assets/image (138).png>)

Information that would be necessary for a voter to make informed decisions should be clearly stated in addition to reasons for listing this asset and the asset name and ticker. For example, simply stating:

`Google should be listed because it is a popular stock`

has numerous problems. First, the specific exchange that the price feed should be taken from is not clearly stated (many companies are dual-listed). Second, Google actually has two stocks that are publicly traded: `GOOGL` and `GOOG` (the former has voting rights whereas the latter does not). In addition, the official name registered with NASDAQ is not `Google` but rather `Alphabet Inc.` Thus it is extremely important to make it as clear as possible to others about the asset that you intend to whitelist.

| Field              | Description                                  | Type     |
| ------------------ | -------------------------------------------- | -------- |
| Asset Name         | Name of the asset to be whitelisted          | Required |
| Ticker             | Ticker of asset to be whitelisted            | Required |
| Listed Exchange    | Exchange that the underlying asset trades on | Required |
| Reason for listing | Short description of whitelist reason        | Required |
| Information Link   | External URL for further information         | Optional |
| Suggested Oracle   | Oracle provider or address                   | Optional |

### 2. Register Whitelist Parameters

![](<../../.gitbook/assets/image (10).png>)

If a proposal to whitelist is approved, another poll must be passed to set the parameters of the newly listed asset. The required fields include `Title`, `Description`, `Asset Name`, `Symbol`, `Oracle Provider`****, `Auction Discount`, and the `Minimum Collateral Ratio`. Again, it is advised to include reference to the passed whitelist poll as well as reasoning for choosing the given oracle address. Given that the Mirror Protocol is in its early stages, it is advisable to keep the parameters equal to existing parameters for already listed assets (Auction Discount at 20%, and Minimum Collateral Ratio at 150%).

Oracle Provider can only be chosen among the ones that are listed on Oracle Hub contract. Originally, mAsset prices are provided by Band Protocol feeder, but other sources can be whitelisted to Oracle Hub contract through Mirror Governance.&#x20;

| Field                    | Description                                                                                                                          | Type     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| Title                    | Title of the poll                                                                                                                    | Required |
| Description              | Short description of the poll                                                                                                        | Required |
| Information Link         | External URL for further information                                                                                                 | Optional |
| Asset Name               | Name of the asset to be whitelisted                                                                                                  | Required |
| Symbol                   | Ticker to be used on Mirror Protocol without the `m` prefix (ex. AAPL instead of mAAPL)                                              | Required |
| Reference Poll ID        | Poll ID of referenced poll (ex. whitelisting poll)                                                                                   | Optional |
| Oracle Provider          | Droplist of available oracle provider for a given `symbol` . The droplist will appear once the `symbol` field is entered correctly.  | Required |
| Auction Discount         | Discount ratio applied during CDP liquidation auction                                                                                | Required |
| Minimum Collateral Ratio | Minimum collateral ratio applied when opening a mint position                                                                        | Required |

### 3. Suggest Pre-IPO Asset

The function of this proposal is to start a poll to whitelist an asset which is scheduled to go through an initial public offering (or IPO), but is not publicly listed yet. Similar to the "Suggest asset whitelisting" proposal, this is also a text proposal. Fields that need to be filled are `Asset Name`, `Ticker`, `Listed Exchange`, and `Reason for listing`. It is suggested that additional information on the IPO or supporting information to be added using the `Information Link` field.

| Field              | Description                                           | Type     |
| ------------------ | ----------------------------------------------------- | -------- |
| Title              | Name of the asset to be whitelisted                   | Required |
| Reason for listing | Short description of whitelist reason                 | Required |
| Asset Name         | Name of the asset to be whitelisted                   | Required |
| Symbol             | Ticker of the asset to be whitelisted                 | Required |
| Listed Exchange    | Exchange which the underlying asset will be traded on | Required |
| Information Link   | External URL for further information                  | Optional |
| Suggested Oracle   | Oracle provider or address                            | Optional |

### 4. Register Pre-IPO Parameters

![](<../../.gitbook/assets/image (1).png>)

After the proposal to [suggest Pre-IPO asset](proposal-types.md#2-suggest-pre-ipo-asset) is approved, an additional poll to register the asset to Mirror Protocol must be submitted and executed.

| Field                                     | Description                                                                                                                          | Type     |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| Title                                     | Title of the poll                                                                                                                    | Required |
| Description                               | Short description of whitelist reason                                                                                                | Required |
| Information Link                          | External URL for further information                                                                                                 | Optional |
| Asset Name                                | Name of the asset to be whitelisted                                                                                                  | Required |
| Symbol                                    | Ticker of the asset to be whitelisted (without the `m` prefix)                                                                       | Required |
| Reference Poll ID                         | Poll ID of the referenced poll                                                                                                       | Optional |
| Listed Exchange                           | Exchange which the underlying asset will be traded on                                                                                | Required |
| Oracle Provider                           | Droplist of available oracle provider for a given `symbol` . The droplist will appear once the `symbol` field is entered correctly.  | Required |
| Auction Discount (after IPO)              | Discount ratio applied during CDP liquidation auction for Post-IPO asset                                                             | Required |
| Min collateral ratio (after IPO)          | Minimum collateral ratio to be used for mint operations for Post-IPO asset                                                           | Required |
| Min Collateral Ratio (during mint period) | Collateral ratio to be applied during `mint_period`                                                                                  | Required |
| Mint Period                               | Number of seconds which mint operation will be allowed for Pre-IPO asset                                                             | Required |
| Pre-IPO Price                             | Fixed price decided by official IPO registration statement, which will be used for Pre-IPO asset                                     | Required |

### 5. Delist Asset

![](<../../.gitbook/assets/image (173).png>)

Specific mAsset on Mirror Protocol is delisted upon approval and execution of this poll. More information about delisting can be found [here](../mirrored-assets-massets.md#delisting-and-migration).

| Field            | Description                           | Type     |
| ---------------- | ------------------------------------- | -------- |
| Title            | Name of the asset to be whitelisted   | Required |
| Description      | Short description of whitelist reason | Required |
| Information Link | External URL for further information  | Optional |
| Asset Name       | Droplist of mAsset on Mirror Protocol | Required |

## Reward Distribution

This category allows modification of two types of MIR distribution: 1) mAsset LP staking reward; 2) Governance voting reward.

### 1. Modify mAsset Reward Distribution

![](<../../.gitbook/assets/image (203).png>)

Through this poll, the ratio which newly minted MIR tokens are distributed among LP token stakers, called `weight` parameter can be changed. To learn how `weight` parameter determines the amount of MIR distributed to each mAsset pool refer to [this document](../mirror-token-mir.md#mir-staking-rewards).

| Field                                  | Description                                                                 | Type     |
| -------------------------------------- | --------------------------------------------------------------------------- | -------- |
| Title                                  | Name of the asset to be whitelisted                                         | Required |
| Reasons for modifying weight parameter | Short description of whitelist reason                                       | Required |
| Information Link                       | External URL for further information                                        | Optional |
| Asset                                  | Droplist of mAsset on Mirror Protocol, which to change weight parameter for | Required |
| Weight                                 | Weight parameter to be applied for LP staking reward distribution           | Required |

## Parameter Modification

This poll category allows modification of mAsset, governance contract, collateral and premium tolerance parameters in Mirror Protocol.

### 1. Modify mint parameters

Through this poll, the existing Mirror Protocol parameters for a specific individual mAsset can be modified. The possible parameters to be updated consist of either the `Auction Discount` and the `Minimum Collateral Ratio`.

![](<../../.gitbook/assets/image (150).png>)

| Field                                 | Description                                                   | Type     |
| ------------------------------------- | ------------------------------------------------------------- | -------- |
| Title                                 | Title of the poll                                             | Required |
| Reasons for modifying mint parameters | Short description of the poll                                 | Required |
| Information Link                      | External URL for further information                          | Optional |
| Asset                                 | Name of the asset to modify parameters                        | Required |
| Auction Discount                      | Discount ratio applied during CDP liquidation auction         | Required |
| Minimum Collateral Ratio              | Minimum collateral ratio applied when opening a mint position | Required |

### 2. Modify governance parameters

Similar to modifying mint parameters, governance parameters such as the `Effective Delay`, and `Voter Weight` can be modified.

![](<../../.gitbook/assets/image (25).png>)

| Fields                                    | Description                                                                         | Type     |
| ----------------------------------------- | ----------------------------------------------------------------------------------- | -------- |
| Title                                     | Title of the poll                                                                   | Required |
| Reason for modifying governance parameter | Short description of the poll                                                       | Required |
| Information Link                          | External URL for further information                                                | Optional |
| Effective Delay                           | Length of delay before protocol integration for a passed poll (in units of seconds) | Optional |
| Voter Weight                              | Ratio of rewards to be distributed to governance voting participants                | Optional |

### 3. Modify poll parameters

This poll allows configuration modifications for `default_poll` types such as `quorum`, `threshold` , `voting_period` and `proposal_deposit`.&#x20;

{% hint style="warning" %}
Due to sensitivity in contract migration polls, both migration and key authorization poll configurations cannot be modified through Mirror Web App UI.&#x20;
{% endhint %}

![](<../../.gitbook/assets/image (28).png>)

| Field            | Description                                                      | Type     |
| ---------------- | ---------------------------------------------------------------- | -------- |
| Title            | Title of the poll                                                | Required |
| Description      | Short description of the poll                                    | Required |
| Information Link | External URL for further information                             | Optional |
| Quorum           | Minimum quorum required for accepting a poll (in percentage)     | Optional |
| Threshold        | Minimum percentage of `YES` votes to pass a poll (in percentage) | Optional |
| Voting Period    | Length of poll (in units of seconds)                             | Optional |
| Proposal Deposit | Required deposit to start a poll (in units of MIR)               | Optional |

### 4. Modify collateral parameters

![](<../../.gitbook/assets/image (192).png>)

Through this poll, existing collateral's `multiplier` or `oracle address` can be modified.

| Field                                      | Description                                                                    | Type     |
| ------------------------------------------ | ------------------------------------------------------------------------------ | -------- |
| Title                                      | Title of the poll                                                              | Required |
| Reasons for modifying collateral parameter | Short description of the poll                                                  | Required |
| Information Link                           | External URL for further information                                           | Optional |
| Collateral (Asset)                         | Droplist of collateral types on Mirror Protocol, which to modify parameter for | Required |
| Multiplier                                 | Number to be `min_collateral_ratio` of the asset being minted                  | Required |
| Collateral Oracle Feeder                   | Address of the collateral oracle used for this asset type                      | Required |

## Price Oracle

This category allows modification on price oracle settings and parameters.&#x20;

### 1. Update Priority

This poll is to propose the priority settings of an mAsset’s price feed. For instance, if there are more than one price sources supporting a specific mAsset (i.e. Band Protocol and Chainlink), the proposer can create a poll to prefer one price source over another.

{% hint style="warning" %}
This poll can only be submitted when there is more than one price source for a chosen mAsset.&#x20;
{% endhint %}

![](<../../.gitbook/assets/image (2).png>)

| Field            | Description                                                                                                        | Type     |
| ---------------- | ------------------------------------------------------------------------------------------------------------------ | -------- |
| Title            | Title of the poll                                                                                                  | Required |
| Description      | Short description of priority change reason                                                                        | Required |
| Information Link | Optional URL for further information                                                                               | Optional |
| Asset            | Droplist of mAssets on Mirror Protocol, which to change price priority for                                         | Required |
| Priority         | Drag & Drop field to put one price source over another. Sources are loaded only after the Asset field is selected. | Required |

### 2. Remove Price

When there is a price source that is invalid or unnecessary (as decided by community consensus), the proposer of this poll can suggest removing a price source for an asset.

![](<../../.gitbook/assets/image (24).png>)

| Field            | Description                                                                                                                           | Type     |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Title            | Title of the poll                                                                                                                     | Required |
| Description      | Short description of price removal                                                                                                    | Required |
| Information Link | Optional URL for further information                                                                                                  | Optional |
| Asset            | Droplist of mAssets on Mirror Protoco, which to change price priority for                                                             | Required |
| Priority         | Droplist of prices currently being provided fro the chosen mAsset. The field may not be a droplist if there is only one price source. | Required |

## Suggestions / Others

### 1. Spend Community Pool

Proposals relating to spending the accrued MIR tokens in the community pool can be made under this section. The proposal should ideally specify the reasons for the distribution and identify the recipient and amount to be given.

![](<../../.gitbook/assets/image (213).png>)

| Fields                             | Description                          | Type     |
| ---------------------------------- | ------------------------------------ | -------- |
| Title                              | Title of the poll                    | Required |
| Reason for community pool spending | Short description of the poll        | Required |
| Information Link                   | External URL for further information | Optional |
| Recipient                          | Grant recipient address              | Required |
| Amount                             | Grant amount (in units of MIR)       | Required |

### 2. Text Poll

The function of the text proposal is to allow proposals that do not fit the defined categories. Given that it is a basic text proposal, simple fields for the `Title`, `Description`, and an `Information Link` for additional information.

| Field            | Description                          | Type     |
| ---------------- | ------------------------------------ | -------- |
| Title            | Title of the poll                    | Required |
| Description      | Short description of the poll        | Required |
| Information Link | External URL for further information | Optional |
