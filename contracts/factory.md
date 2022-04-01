# Factory

The Factory contract is Mirror Protocol's central directory and organizes information related to mAssets and the Mirror Token (MIR). It is also responsible for minting new MIR tokens each block and distributing them to the [Staking Contract](staking.md) for rewarding LP \&sLP Token stakers.

After the initial bootstrapping of Mirror Protocol contracts, the Factory is assigned to be the owner for the Mint, Oracle, Staking, and Collector contracts. The Factory is owned by the [Gov Contract.](gov.md)

## Config

| Name                   | Type      | Description                                           |
| ---------------------- | --------- | ----------------------------------------------------- |
| `mirror_token`         | HumanAddr | Contract address of Mirror Token (MIR)                |
| `mint_contract`        | HumanAddr | Contract address of [Mirror Mint](mint.md)            |
| `oracle_contract`      | HumanAddr | Contract address of [Mirror Oracle](broken-reference) |
| `terraswap_factory`    | HumanAddr | Contract address of Terraswap Factory                 |
| `staking_contract`     | HumanAddr | Contract address of [Mirror Staking](staking.md)      |
| `commission_collector` | HumanAddr | Contract address of [Mirror Collector](collector.md)  |
| `mint_per_block`       | Uint128   | Amount of new MIR tokens to mint per block            |
| `token_code_id`        | u64       | Code ID for CW20 contract for generating new mAssets  |
| `base_denom`           | String    | Native token denom for Terraswap pairs (TerraUSD)     |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub token_code_id: u64,
    pub base_denom: String,
    pub distribution_schedule: Vec<(u64, u64, Uint128)>, // [[start_time, end_time, distribution_amount], [], ...]
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "token_code_id": 8,
  "base_denom": "uusd",
  "distribution_schedule": [
    [3600, 7200, "1000000"],
    [7200, 10800, "1000000"]
  ]
}
```
{% endtab %}
{% endtabs %}

| Key                     | Type   | Description                                                                                                                                                                                                                                                                                                                          |
| ----------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `token_code_id`         | u64    | Code ID for CW20 contract for generating new mAssets                                                                                                                                                                                                                                                                                 |
| `base_denom`            | String | Native token denom for Terraswap pairs (TerraUSD)                                                                                                                                                                                                                                                                                    |
| `distribution_schedule` | Vec    | <p>Distribution schedule for the minting of new MIR tokens. Each entry consists of:</p><ul><li>start time (seconds)</li><li>end time (seconds)</li><li>distribution amount for the interval</li></ul><p>Determines the total amount of new MIR tokens minted as rewards for LP stakers over the interval [start time, end time].</p> |

## HandleMsg

### `PostInitialize`

Issued by the Factory contract's owner after bootstrapping to initialize the contract's configuration.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    PostInitialize {
        commission_collector: HumanAddr,
        mint_contract: HumanAddr,
        mirror_token: HumanAddr,
        oracle_contract: HumanAddr,
        owner: HumanAddr,
        staking_contract: HumanAddr,
        terraswap_factory: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "post_initialize": {
    "commission_collector": "terra1...",
    "mint_contract": "terra1...",
    "mirror_token": "terra1...",
    "oracle_contract": "terra1...",
    "owner": "terra1...",
    "staking_contract": "terra1...",
    "terraswap_factory": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key                    | Type      | Description                                           |
| ---------------------- | --------- | ----------------------------------------------------- |
| `commission_collector` | HumanAddr | Contract address of [Mirror Collector](collector.md)  |
| `mint_contract`        | HumanAddr | Contract address of [Mirror Mint](mint.md)            |
| `mirror_token`         | HumanAddr | Contract address of Mirror Token (MIR)                |
| `oracle_contract`      | HumanAddr | Contract address of [Mirror Oracle](broken-reference) |
| `**owner`              | HumanAddr | Address of the owner of [Mirror Factory](factory.md)  |
| `staking_contract`     | HumanAddr | Contract address of [Mirror Staking](staking.md)      |
| `terraswap_factory`    | HumanAddr | Contract address of Terraswap Factory                 |

### `UpdateConfig`

Updates the configuration for the contract. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        token_code_id: Option<u64>,
        distribution_schedule: Option<Vec<(u64, u64, Uint128)>>
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...",
    "token_code_id": 8,
    "distribution_schedule": [
      [3600, 7200, "1000000"],
      [7200, 10800, "1000000"]
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Key                         | Type                     | Description                                          |
| --------------------------- | ------------------------ | ---------------------------------------------------- |
| `owner`\*                   | HumanAddr                | Address of the owner of [Mirror Factory](factory.md) |
| `token_code_id`\*           | u64                      | Code ID for CW20 contract for generating new mAssets |
| `**distribution_schedule`\* | Vec<(u64, u64, Uint128)> | New distribution schedule                            |

\* = optional

### `UpdateWeight`

Updates the `weight` parameter of a specific token.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
 UpdateWeight {
        asset_token: HumanAddr,
        weight: u32,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "update_weight": {
        "asset_token": "terra1...",
        "weight": 8
    }
}
```
{% endtab %}
{% endtabs %}

| Name          | Type      | Description                                                                    |
| ------------- | --------- | ------------------------------------------------------------------------------ |
| `asset_token` | HumanAddr | Contract address of mAsset token                                               |
| `weight`      | u32       | Ratio of MIR token reward received by this asset in comparison to other tokens |

### `Whitelist`

Introduces a new mAsset to the protocol and creates markets on Terraswap. This process will:

* Instantiate the mAsset contract as a new Terraswap CW20 token
* Register the mAsset with Oracle Hub and [Mirror Mint](mint.md)
* Create a new Terraswap Pair for the new mAsset against TerraUSD
* Instantiate the LP Token contract associated with the pool as a new Terraswap CW20 token
* Register the LP token with the [Mirror Staking](staking.md) contract

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Whitelist {
        name: String,
        oracle_proxy: HumanAddr,
        params: Params,
        symbol: String,
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct Params {
    pub auction_discount: Decimal,
    pub min_collateral_ratio: Decimal,
    pub weight: Option<u32>,
    pub mint_period: Option<u64>,
    pub min_collateral_ratio_after_ipo: Option<Decimal>,
    pub pre_ipo_price: Option<Decimal>,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "whitelist": {
    "name": "Mirrored Apple Derivative",
    "oracle_proxy": "terra1...",
    "params": {
      "auction_discount": "0.2",
      "min_collateral_ratio": "1.5",
    },
    "symbol": "mAAPL"
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type      | Description                                                              |
| -------------- | --------- | ------------------------------------------------------------------------ |
| `name`         | String    | Name of new asset to be whitelisted                                      |
| `oracle_proxy` | HumanAddr | Address of the oracle proxy contract that provides prices for this asset |
| `params`       | Params    | mAsset parameters to be registered                                       |
| `symbol`       | String    | mAsset symbol (ex: `AAPL`)                                               |

#### mAsset Params

| Key                                | Type    | Description                                                                    |
| ---------------------------------- | ------- | ------------------------------------------------------------------------------ |
| `auction_discount`                 | Decimal | Liquidation discount for purchasing CDP's collateral                           |
| `min_collateral_ratio`             | Decimal | Minimum C-ratio for CDPs that mint the mAsset                                  |
| `weight`\*                         | u32     | Ratio of MIR token reward received by this asset in comparison to other tokens |
| `mint_period`\*                    | u64     | Time period after asset creation in which minting is enabled (Seconds)         |
| `min_collateral_ratio_after_ipo`\* | Decimal | `min_collateral_ratio` to be applied to this asset after IPO                   |
| `pre_ipo_price`\*                  | Decimal | Fixed price used to mint Pre-IPO asset during `mint_period`                    |

\*= optional (Only added for [Pre-IPO asset whitelisting](../protocol/mirrored-assets-massets.md#pre-ipo))

### `TokenCreationHook`

`(INTERNAL)`

Called after mAsset token contract is created in the [Whitelist](factory.md#whitelist) process. \*\*Why this is necessary

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    TokenCreationHook {
        oracle_feeder: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "token_creation_hook": {
    "oracle_feeder": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key             | Type      | Description                         |
| --------------- | --------- | ----------------------------------- |
| `oracle_feeder` | HumanAddr | Address of Oracle Feeder for mAsset |

### `TerraswapCreationHook`

`(INTERNAL)`

Called after mAsset token contract is created in the [Whitelist](factory.md#whitelist) process.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct enum HandleMsg {
    TerraswapCreationHook {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "terraswap_creation_hook": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key           | Type      | Description                      |
| ------------- | --------- | -------------------------------- |
| `asset_token` | HumanAddr | Contract address of mAsset token |

### `PassCommand`

Calls the contract specified with the message to execute. Used for invoking functions on other Mirror Contracts since Mirror Factory is defined to be the owner. To be controlled through governance.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    PassCommand {
        contract_addr: HumanAddr,
        msg: Binary,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "pass_command": {
    "contract_addr": "terra1...",
    "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
  }
}
```
{% endtab %}
{% endtabs %}

| Key             | Type      | Description                          |
| --------------- | --------- | ------------------------------------ |
| `contract_addr` | HumanAddr | Contract address of contract to call |
| `msg`           | Binary    | Base64-encoded JSON of ExecuteMsg    |

### `Distribute`

Mints the appropriate amount of new MIR tokens as reward for LP stakers by sends the newly minted tokens to the Mirror Staking contract to be distributed to its stakers. Can be called by anyone at any time to trigger block reward distribution for LP stakers.

The contract keeps track of the last height at which `Distribute` was called for a specific asset, and uses it to calculate the amount of new assets to mint for the blocks occurred in the interval between.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Distribute {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribute": {}
}
```
{% endtab %}
{% endtabs %}

### `RevokeAsset`

`(Feeder Operation)`\
Used when delisting occurs for a specific mAsset. &#x20;

* mAsset is unregistered from MIR reward pool
* Fixes the oracle price at `end_price` for mint contract operation

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RevokeAsset {
        asset_token: HumanAddr,
        end_price: Decimal,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "revoke_asset": {
        "asset_token": "terra1...",
        "end_price": "123.456789"
    }
}
```
{% endtab %}
{% endtabs %}

| Name          | Type      | Description                                     |
| ------------- | --------- | ----------------------------------------------- |
| `asset_token` | HumanAddr | Contract address of mAsset token                |
| `end_price`   | Decimal   | <p>Final price to freeze revoked mAsset<br></p> |

### `MigrateAsset`

Can be issued by the oracle feeder of an mAsset to trigger the [mAsset migration procedure](../protocol/mirrored-assets-massets.md#deprecation-and-migration).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    MigrateAsset {
        name: String,
        symbol: String,
        oracle_proxy: String,
        from_token: String,
    },
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "migrate_asset": {
    "name": "Apple Inc.",
    "symbol": "AAPL",
    "oracle_proxy": "terra1...",
    "from_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type   | Description                                                                                                    |
| -------------- | ------ | -------------------------------------------------------------------------------------------------------------- |
| `name`         | String | Name of the new asset post-migration                                                                           |
| `symbol`       | String | Symbol for the new asset post-migration                                                                        |
| `oracle_proxy` | String | Address of the oracle proxy contract that will provide prices for this asset after asset migration takes place |
| `from_token`   | String | Contract address of mAsset to migrate                                                                          |

## QueryMsg

### `Config`

Get the Mirror Factory configuration.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {}
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr,
    pub mirror_token: HumanAddr,
    pub mint_contract: HumanAddr,
    pub staking_contract: HumanAddr,
    pub commission_collector: HumanAddr,
    pub oracle_contract: HumanAddr,
    pub terraswap_factory: HumanAddr,
    pub token_code_id: u64,
    pub base_denom: String,
    pub genesis_time: u64,
    pub distribution_schedule: Vec<(u64, u64, Uint128)>,
}
```

| Key                     | Type                     | Description                                                                                                                                                                                                                                                                                                   |
| ----------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `owner`                 | HumanAddr                | Contract address of mAsset token                                                                                                                                                                                                                                                                              |
| `mirror_token`          | HumanAddr                | Final price to freeze revoked mAsset                                                                                                                                                                                                                                                                          |
| `mint_contract`         | HumanAddr                | Contract address of Mirror Mint                                                                                                                                                                                                                                                                               |
| `staking_contract`      | HumanAddr                | Contract address of Mirror Oracle                                                                                                                                                                                                                                                                             |
| `commission_collector`  | HumanAddr                | Contract address of Mirror Collector                                                                                                                                                                                                                                                                          |
| `oracle_contract`       | HumanAddr                | Contract address of Mirror Oracle                                                                                                                                                                                                                                                                             |
| `terraswap_factory`     | HumanAddr                | Contract address of Terraswap Factory                                                                                                                                                                                                                                                                         |
| `token_code_id`         | u64                      | Code ID for CW20 contract for generating new mAssets                                                                                                                                                                                                                                                          |
| `base_denom`            | String                   | Native token denom for Terraswaap pairs (TerraUSD)                                                                                                                                                                                                                                                            |
| `genesis_time`          | u64                      | Block height which the Factory contract was instantiated                                                                                                                                                                                                                                                      |
| `distribution_schedule` | Vec<(u64, u64, Uint128)> | <p>Distribution schedule for the minting of new MIR tokens. Each entry consists of:<br>- start time (seconds)<br>- end time (seconds)<br>distribution amount for the interval<br>Determines the total amount of new MIR tokens minted as rewards for LP stakers over the interval [start time, end time].</p> |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "config": {}
}
```

#### Response

```javascript
{
    "config_response": {
        "owner": "terra1..."
        "mirror_token": "terra1...",
        "mint_contract": "terra1...",
        "staking_contract": "terra1...",
        "commission_collector": "terra1...",
        "oracle_contract": "terra1...",
        "terraswap_factory": "terra1...",
        "token_code_id": 8,
        "base_denom": "uusd",
        "genesis_time": 1000000,
        "distribution_schedule": [
            [3600, 7200, "1000000"],
            [7200, 10800, "1000000"]
        ]
    }
}  
```

| Key                     | Type                     | Description                                                                                                                                                                                                                                                                                               |
| ----------------------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `owner`                 | HumanAddr                | Contract address of mAsset token                                                                                                                                                                                                                                                                          |
| `mirror_token`          | HumanAddr                | Final price to freeze revoked mAsset                                                                                                                                                                                                                                                                      |
| `mint_contract`         | HumanAddr                | Contract address of Mirror Mint                                                                                                                                                                                                                                                                           |
| `staking_contract`      | HumanAddr                | Contract address of Mirror Oracle                                                                                                                                                                                                                                                                         |
| `commission_collector`  | HumanAddr                | Contract address of Mirror Collector                                                                                                                                                                                                                                                                      |
| `oracle_contract`       | HumanAddr                | Contract address of Mirror Oracle                                                                                                                                                                                                                                                                         |
| `terraswap_factory`     | HumanAddr                | Contract address of Terraswap Factory                                                                                                                                                                                                                                                                     |
| `token_code_id`         | u64                      | Code ID for CW20 contract for generating new mAssets                                                                                                                                                                                                                                                      |
| `base_denom`            | String                   | Native token denom for Terraswaap pairs (TerraUSD)                                                                                                                                                                                                                                                        |
| `genesis_time`          | u64                      | Block height which the Factory contract was instantiated                                                                                                                                                                                                                                                  |
| `distribution_schedule` | Vec<(u64, u64, Uint128)> | <p>Distribution schedule for the minting of new MIR tokens. Each entry consists of:<br>start time (seconds)<br>end time (seconds)<br>distribution amount for the interval<br>Determines the total amount of new MIR tokens minted as rewards for LP stakers over the interval [start time, end time].</p> |
{% endtab %}
{% endtabs %}

### `DistributionInfo`

Get the distribution schedules for MIR token.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    DistributionInfo {}
}
```

| Key           | Type      | Description                     |
| ------------- | --------- | ------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset token |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct DistributionInfoResponse {
    pub weights: Vec<(HumanAddr, u32)>,
    pub last_distributed: u64,
}
```

| Key                | Type                  | Description                                                                                                           |
| ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `weights`          | Vec<(HumanAddr, u32)> | <p>List of reward weight assigned to each mAsset. Each entry consists of:<br>- Token contract address<br>- weight</p> |
| `last_distributed` | u64                   | Block height of the most recent reward distribution                                                                   |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribution_info": {}
}
```

| Key           | Type      | Description                     |
| ------------- | --------- | ------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset token |

#### Response

```javascript
{
    "distribution_info_response": {
        "weights": [
            ["terra1...", 8],
            ["terra1...", 8]
        ],
        "last_distributed": 3600
    }
}
```

| Key                | Type                  | Description                                                                                                           |
| ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `weights`          | Vec<(HumanAddr, u32)> | <p>List of reward weight assigned to each mAsset. Each entry consists of:<br>- Token contract address<br>- weight</p> |
| `last_distributed` | u64                   | Block height of the most recent reward distribution                                                                   |
{% endtab %}
{% endtabs %}
