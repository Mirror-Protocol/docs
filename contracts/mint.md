# Mint

The Mint Contract implements the logic for [Collateralized Debt Positions ](../protocol/mirrored-assets-massets.md#collateralized-debt-position)(CDPs), through which users can mint or short new mAsset tokens against their deposited collateral (UST or mAssets).&#x20;

Current prices of collateral and minted mAssets are read from the [Collateral Oracle](collateral-oracle.md) and [Oracle Contract](broken-reference) to determine the C-ratio of each CDP. Depending on which the type of asset used as the collateral, the minimum collateral ratio of each CDP may change. Collateral Oracle is responsible for feeding prices and collateral ratio `multiplier` of each collateral asset type.&#x20;

The Mint Contract also contains the logic for liquidating CDPs with C-ratios below the minimum for their minted mAsset through auction.

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub oracle: HumanAddr,
    pub collector: HumanAddr,
    pub collateral_oracle: HumanAddr,
    pub staking: HumanAddr,
    pub terraswap_factory: HumanAddr,
    pub lock: HumanAddr,
    pub base_denom: String,
    pub token_code_id: u64,
    pub protocol_fee_rate: Decimal,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner": "terra1...",
  "oracle": "terra1...",
  "collector": "terra1...",
  "collateral_oracle": "terra1...",
  "staking": "terra1...",
  "terraswap_factory": "terra1...",
  "lock": "terra1...",
  "base_denom": "uusd",
  "token_code_id": 8,
  "protocol_fee_rate": "0.123
}
```
{% endtab %}
{% endtabs %}

| Key                 | Type      | Description                                                          |
| ------------------- | --------- | -------------------------------------------------------------------- |
| `owner`             | HumanAddr | Owner of contract                                                    |
| `oracle`            | HumanAddr | Contract address of [Mirror Oracle](broken-reference)                |
| `collector`         | HumanAddr | Contract address of [Mirror Collector](collector.md)                 |
| `collateral_oracle` | HumanAddr | Contract address of [Mirror Collateral Oracle](collateral-oracle.md) |
| `staking`           | HumanAddr | Contract address of [Mirror Staking](staking.md)                     |
| `terraswap_factory` | HumanAddr | Contract address of Terraswap Factory                                |
| `lock`              | HumanAddr | Contract address of [Mirror Lock](lock.md)                           |
| `base_denom`        | String    | Native token denomination for stablecoin (TerraUSD)                  |
| `token_code_id`     | u64       | Code ID for Terraswap CW20 Token                                     |
| `protocol_fee_rate` | Decimal   | Protocol fee                                                         |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Mint contract is the recipient. Allows the token transfer to execute a [Receive Hook](mint.md#receive-hooks) as a subsequent action within the same transaction.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Receive {
        amount: Uint128,
        sender: HumanAddr,
        msg: Option<Binary>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "receive": {
    "amount": "10000000",
    "sender": "terra1...",
    "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
  }
}
```
{% endtab %}
{% endtabs %}

| Key      | Type      | Description                                                            |
| -------- | --------- | ---------------------------------------------------------------------- |
| `amount` | Uint128   | Amount of tokens received                                              |
| `sender` | HumanAddr | Sender of the token transfer                                           |
| `msg`\*  | Binary    | Base64-encoded string of JSON of [Receive Hook](mint.md#receive-hooks) |

\* = optional

### `UpdateConfig`

Updates the configuration of the Mint contract. Can only be issued by the owner of the Mint contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        oracle: Option<HumanAddr>,
        collector: Option<HumanAddr>,
        collateral_oracle: Option<HumanAddr>,
        terraswap_factory: Option<HumanAddr>,
        lock: Option<HumanAddr>,
        token_code_id: Option<u64>,
        protocol_fee_rate: Option<Decimal>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...",
    "oracle": "terra1...",
    "collector": "terra1...",
    "collateral_oracle": "terra1...",
    "staking": "terra1...",
    "terraswap_factory": "terra1...",
    "lock": "terra1...",
    "token_code_id": 8,
    "protocol_fee_rate": "0.123",
  }
}
```
{% endtab %}
{% endtabs %}

| Key                   | Type      | Description                           |
| --------------------- | --------- | ------------------------------------- |
| `owner`\*             | HumanAddr | New owner                             |
| `oracle`\*            | u64       | New oracle contract address           |
| `collector`\*         | HumanAddr | New collector contract address        |
| `terraswap_factory`\* | HumanAddr | Contract address of Terraswap Factory |
| `lock`\*              | HumanAddr | Contract address of Mirror Lock       |
| `token_code_id`\*     | u64       | New token code ID                     |
| `protocol_fee_rate`\* | Decimal   | New protocol fee rate                 |

\* = optional

### `UpdateAsset`

Updates mAsset's minting and liquidation parameters. Can only be issued by the owner of the Mint contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateAsset {
        asset_info: AssetInfo,
        auction_discount: Option<Decimal>,
        min_collateral_ratio: Option<Decimal>,
        ipo_params: Option<IPOParams>
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct IPOParams {
    pub mint_end: u64,
    pub pre_ipo_price: Decimal,
    pub min_collateral_ratio_after_ipo: Decimal,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_asset": {
    "asset_info": {
      "token": {
        "contract_addr": "terra1..."
      }
    },
    "auction_discount": "123.456789",
    "min_collateral_ratio": "123.456789",
    "ipo_params": {
      "mint_end": "10000000",
      "pre_ipo_price": "7.77",
      "min_collateral_ratio_after_ipo": "1.5"
    }
  }
}
```
{% endtab %}
{% endtabs %}

| Key                      | Type      | Description                             |
| ------------------------ | --------- | --------------------------------------- |
| `asset_info`             | AssetInfo | Asset to be updated                     |
| `auction_discount`\*     | Decimal   | New auction discount rate               |
| `min_collateral_ratio`\* | Decimal   | New minimum collateralization ratio     |
| `ipo_params`\*           | IPOParams | Parameters to be used for Pre-IPO asset |

\* = optional

#### IPOParams

| Key                              | Type    | Description                                                                          |
| -------------------------------- | ------- | ------------------------------------------------------------------------------------ |
| `mint_end`                       | u64     | Time which `mint_period` ends                                                        |
| `pre_ipo_price`                  | Decimal | Fixed price to be used to mint during `mint_period`                                  |
| `min_collateral_ratio_after_ipo` | Decimal | Minimum collateralization ratio to be used after IPO is triggered (Used for pre-IPO) |

### `RegisterAsset`

Registers a new mAsset to be mintable.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {    
    RegisterAsset {
        asset_token: HumanAddr,
        auction_discount: Decimal,
        min_collateral_ratio: Decimal,
        ipo_params: Option<IPOParams>
    },
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "register_asset": {
        "asset_token": "terra1...",
        "auction_discount": "0.2",
        "min_collateral_ratio": "1.5",
        "ipo_params": {
            "mint_end": "10000000",
            "pre_ipo_price": "7.77",
            "min_collateral_ratio_after_ipo": "1.5"
    }
}
```
{% endtab %}
{% endtabs %}

| Key                    | Type      | Description                                 |
| ---------------------- | --------- | ------------------------------------------- |
| `asset_token`          | HumanAddr | Contract address of mAsset to be registered |
| `auction_discount`     | Decimal   | Auction discount rate                       |
| `min_collateral_ratio` | Decimal   | Minimum collateralization ratio             |
| `ipo_params`\*         | IPOParams | Parameters to be used for Pre-IPO asset     |

\*= optional

### `TriggerIPO`

Asset feeder is allowed to trigger IPO event on pre-IPO asset to have the following characters:

* Oracle feeder now feeds real-time price of the underlying asset
* The asset becomes mintable again, even after the `mint_period` has ended

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    TriggerIPO {
        asset_token: HumanAddr, 
    },
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "trigger_ipo": {
        "asset_token": "terra1..."
    }
}
```
{% endtab %}
{% endtabs %}

| Key           | Type      | Description                   |
| ------------- | --------- | ----------------------------- |
| `asset_token` | HumanAddr | Contract address of the token |

### `RegisterMigration`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterMigration {
        asset_token: HumanAddr,
        end_price: Decimal,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_migration": {
    "asset_token": "terra1...",
    "end_price": "123.456789"
  }
}
```
{% endtab %}
{% endtabs %}

| Key           | Type      | Description                              |
| ------------- | --------- | ---------------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset to be migrated |
| `end_price`   | Decimal   | Final price to freeze old mAsset         |

### `OpenPosition`

{% hint style="info" %}
Used for creating a new CDP with TerraUSD collateral. For creating a CDP using mAsset collateral, you need to use the [Receive Hook variant](mint.md#openposition-1).
{% endhint %}

Opens a new CDP with an initial deposit of collateral. The user specifies the target minted mAsset for the CDP, and sets the desired initial collateralization ratio, which must be greater or equal than the minimum for the mAsset. The initial amount of minted mAsset tokens are determined by:

```rust
let mint_amount = collateral.amount
    * collateral_price
    * reverse_decimal(asset_price)
    * reverse_decimal(collateral_ratio);
```

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {   
    OpenPosition {
        collateral: Asset,
        asset_info: AssetInfo,
        collateral_ratio: Decimal,
        short_params: Option<ShortParams>,
    }
}

pub struct Asset {
    pub info: AssetInfo,
    pub amount: Uint128,
}

#[serde(rename_all = "snake_case")]
pub enum AssetInfo {
    Token { contract_addr: HumanAddr },
    NativeToken { denom: String },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "open_position": {
        "collateral": {
            "info": {
                "token": {
                    "contract_addr": "terra1..."
                    }
                },
                "amount": "1000000"
            },
        "asset_info": {
            "token": {
                "contract_addr": "terra1..."
                }
            },
        "collateral_ratio": "1.5",
        "short_params": {
            "belief_price": "123.456789",
            "max_spread": "0.1"
        }
    }
}
```
{% endtab %}
{% endtabs %}

| Key                | Type        | Description                                                                                        |
| ------------------ | ----------- | -------------------------------------------------------------------------------------------------- |
| `asset_info`       | AssetInfo   | Asset to be minted by CDP                                                                          |
| `collateral`       | Asset       | Initial collateral deposit for the CDP                                                             |
| `collateral_ratio` | Decimal     | Initial desired collateralization ratio                                                            |
| `short_params`\*   | ShortParams | Terraswap Price and spread limit to immediately short tokens after CDP creation (used for "Short") |

\*= optional

#### `ShortParams`

If optional `short_params` is added, mint contract will immediately sell the minted mAssets and`sLP` tokens will be minted and sent to the user. The UST obtained from the operation will be added to the user's lock position in [lock contract](lock.md).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ShortParams {
    pub belief_price: Option<Decimal>,
    pub max_spread: Option<Decimal>,
}
```
{% endtab %}
{% endtabs %}

| Key              | Type    | Description                                                                  |
| ---------------- | ------- | ---------------------------------------------------------------------------- |
| `belief_price`\* | Decimal | Price submitted to the Terraswap pool                                        |
| `max_spread`\*   | Decimal | Maximum slippage accepted during swap transaction against the Terraswap Pool |

\*=optional

### `Deposit`

{% hint style="info" %}
Used for depositing TerraUSD collateral. For depositing mAsset collateral to a CDP, you need to use the [Receive Hook variant](mint.md#deposit-1).
{% endhint %}

Deposits additional collateral to an existing CDP to raise its C-ratio.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Deposit {
        collateral: Asset,
        position_idx: Uint128,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit": {
    "collateral": {
      "info": {
        "native_token": {
          "denom": "uusd",
        }
      },
      "amount": "1000000"
    },
    "position_idx": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description                       |
| -------------- | ------- | --------------------------------- |
| `collateral`   | Asset   | Collateral amount to be deposited |
| `position_idx` | Uint128 | Index of position                 |

### `Withdraw`

Withdraws collateral from the CDP. Cannot withdraw more than an amount that would drop the CDP's C-ratio below the minted mAsset's mandated minimum.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Withdraw {
        collateral: Asset,
        position_idx: Uint128,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw": {
    "collateral": {
      "info": {
        "token": {
          "contract_address": "terra1..."
        }
      },
      "amount": "1000000"
    },
    "position_idx": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `collateral`   | Asset   | Collateral to withdraw |
| `position_idx` | Uint128 | Index of position      |

### `Mint`

Mints new mAssets against an existing CDP. Cannot mint more than what would bring the CDP's C-ratio below its minted mAsset's mandated minimum.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Mint {
        asset: Asset,
        position_idx: Uint128,
        short_params: Option<ShortParams>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "mint": {
    "position_idx": "10000000",
    "asset": {
      "info": {
        "token": {
          "contract_address": "terra1..."
        }
      },
      "amount": "1000000"
    },
    "short_params": {
      "belief_price": "123.456789",
      "max_spread": "0.1"
      }
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type        | Description                                                                                                                      |
| -------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `position_idx` | Uint128     | Index of position                                                                                                                |
| `asset`        | Asset       | mAssets to be minted                                                                                                             |
| `short_params` | ShortParams | Terraswap Price and maximum slippage tolerance to be applied for selling minted token after position creation (used for "Short") |

## Receive Hooks

### `OpenPosition`

Issued when a user sends mAsset tokens to the Mint contract.

Uses the sent amount to create a new CDP.

{% hint style="info" %}
Used for creating a new CDP with mAsset collateral. For creating a CDP using TerraUSD collateral, you need to use the [HandleMsg variant.](mint.md#openposition)
{% endhint %}

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    OpenPosition {
        asset_info: AssetInfo,
        collateral_ratio: Decimal,
        short_params: Option<ShortParams>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "open_position": {
    "asset_info": {
      "token": {
        "contract_addr": "terra1..."
      }
    },
    "collateral_ratio": "123.456789",
    "short_params": {
      "belief_price": "123.456789",
      "max_spread": "0.1"
      }
  }
}
```
{% endtab %}
{% endtabs %}

| Key                | Type        | Description                                                                                        |
| ------------------ | ----------- | -------------------------------------------------------------------------------------------------- |
| `asset_info`       | AssetInfo   | mAsset to be minted by CDP                                                                         |
| `collateral_ratio` | Decimal     | Initial collateralization ratio to use                                                             |
| `short_params`     | ShortParams | Terraswap Price and spread limit to immediately short tokens after CDP creation (used for "Short") |

### `Deposit`

Issued when a user sends mAsset tokens to the Mint contract.

Deposits the amount as collateral to an open CDP.

{% hint style="info" %}
Used for depositing mAsset collateral. For depositing TerraUSD collateral to a CDP, you need to use the [HandleMsg variant](mint.md#deposit).
{% endhint %}

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Deposit {
        position_idx: Uint128,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit": {
    "position_idx": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description       |
| -------------- | ------- | ----------------- |
| `position_idx` | Uint128 | Index of position |

### `Burn`

Issued when a user sends mAsset tokens to the Mint contract.&#x20;

Burns the sent tokens against a CDP and reduces the C-ratio. If all outstanding minted mAsset tokens are burned, the position is closed and the collateral is returned.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Burn {
        position_idx: Uint128,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "burn": {
    "position_idx": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description       |
| -------------- | ------- | ----------------- |
| `position_idx` | Uint128 | Index of position |

### `Auction`

Issued when a user sends mAsset tokens to the Mint contract.

Purchases the collateral of a CDP subject to liquidation (whose C-ratio has fallen under its minted mAsset's minimum). The buyer cannot pay more than the CDP's current minted mAsset balance.

The discounted price for the collateral is calculated as follows:

```rust
let discounted_price = price * (1 - config.auction_discount);
```

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Auction {
        position_idx: Uint128,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "auction": {
    "position_idx": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description       |
| -------------- | ------- | ----------------- |
| `position_idx` | Uint128 | Index of position |

## QueryMsg

### `Config`

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
    pub oracle: HumanAddr,
    pub collector: HumanAddr,
    pub collateral_oracle: HumanAddr,
    pub staking: HumanAddr,
    pub terraswap_factory: HumanAddr,
    pub lock: HumanAddr,
    pub base_denom: String,
    pub token_code_id: u64,
    pub protocol_fee_rate: Decimal,
}
```

| Key                 | Type      | Description                                           |
| ------------------- | --------- | ----------------------------------------------------- |
| `owner`             | HumanAddr | Owner of contract                                     |
| `oracle`            | HumanAddr | Contract address of [Mirror Oracle](broken-reference) |
| `collector`         | HumanAddr | Contract address of [Mirror Collector](collector.md)  |
| `collateral_oracle` | HumanAddr | Contract address of Mirror Collateral Oracle          |
| `staking`           | HumanAddr | Contract address of Mirror Staking                    |
| `terraswap_factory` | HumanAddr | Contract address of Terraswap Factory                 |
| `lock`              | HumanAddr | Contract address of Mirror Lock                       |
| `base_denom`        | String    | Native token denomination for stablecoin (TerraUSD)   |
| `token_code_id`     | u64       | Code ID for Terraswap CW20 Token                      |
| `protocol_fee_rate` | Decimal   | Protocol fee                                          |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "config": {}
}
```

#### Response

```rust
{
    "config_response": {
        "owner": "terra1...",
        "oracle": "terra1...",
        "collector": "terra1...",
        "collateral_oracle": "terra1...",
        "staking": "terra1...",
        "terraswap_factory": "terra1...",
        "lock": "terra1...",
        "base_denom": "uusd",
        "token_code_id": 8,
        "protocol_fee_rate": "123.456789"
    }
}
```

| Key                 | Type      | Description                                           |
| ------------------- | --------- | ----------------------------------------------------- |
| `owner`             | HumanAddr | Owner of contract                                     |
| `oracle`            | HumanAddr | Contract address of [Mirror Oracle](broken-reference) |
| `collector`         | HumanAddr | Contract address of [Mirror Collector](collector.md)  |
| `collateral_oracle` | HumanAddr | Contract address of Mirror Collateral Oracle          |
| `staking`           | HumanAddr | Contract address of Mirror Staking                    |
| `terraswap_factory` | HumanAddr | Contract address of Terraswap Factory                 |
| `lock`              | HumanAddr | Contract address of Mirror Lock                       |
| `base_denom`        | String    | Native token denomination for stablecoin (TerraUSD)   |
| `token_code_id`     | u64       | Code ID for Terraswap CW20 Token                      |
| `protocol_fee_rate` | Decimal   | Protocol fee                                          |
{% endtab %}
{% endtabs %}

### `AssetConfig`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    AssetConfig {
        asset_token: HumanAddr,
    }
}
```

| Key           | Type      | Description                        |
| ------------- | --------- | ---------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset to query |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct AssetConfigResponse {
    pub token: HumanAddr,
    pub auction_discount: Decimal,
    pub min_collateral_ratio: Decimal,
    pub end_price: Option<Decimal>,
    pub ipo_params: Option<IPOParams>,
}
```

| Key                    | Type      | Description                                                              |
| ---------------------- | --------- | ------------------------------------------------------------------------ |
| `token`                | HumanAddr | Contract address of asset to query                                       |
| `auction_discount`     | Decimal   | Discount rate applied for liquidation auction                            |
| `min_collateral_ratio` | Decimal   | Lowest collateral ratio to mint this mAsset                              |
| `end_price`\*          | Decimal   | Fixed oracle price of mAsset when migration / Pre-IPO / Delisting occurs |
| `ipo_params`\*         | u64       | Parameters to be used for Pre-IPO assets                                 |

\*= optional
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "asset_config": {
    "asset_info": {
      "token": {
        "contract_addr": "terra1..."
      }
    }
  }
}
```

| Key           | Type      | Description                        |
| ------------- | --------- | ---------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset to query |

#### Response

```rust
{
    "asset_config_response": {
        "token": "terra1...",
        "auction_discount": "0.2",
        "min_collateral_ratio": "1.5",
        "end_price": "123.456789",
        "ipo_params": {
            "mint_end": "10000000",
            "pre_ipo_price": "7.77",
            "min_collateral_ratio_after_ipo": "1.5"
        }    
    }
}
```

| Key                    | Type      | Description                                                              |
| ---------------------- | --------- | ------------------------------------------------------------------------ |
| `token`                | HumanAddr | Contract address of asset to query                                       |
| `auction_discount`     | Decimal   | Discount rate applied for liquidation auction                            |
| `min_collateral_ratio` | Decimal   | Lowest collateral ratio to mint this mAsset                              |
| `end_price`\*          | Decimal   | Fixed oracle price of mAsset when migration / Pre-IPO / Delisting occurs |
| `ipo_params`\*         | u64       | Parameters to be used for Pre-IPO assets                                 |

\*= optional
{% endtab %}
{% endtabs %}

### `Position`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Position {
        position_idx: Uint128,
    }
}
```

| Key            | Type    | Description                |
| -------------- | ------- | -------------------------- |
| `position_idx` | Uint128 | Index of position to query |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PositionResponse {
    pub idx: Uint128,
    pub owner: HumanAddr,
    pub collateral: Asset,
    pub asset: Asset,
    pub is_short: bool,
}
```

| Name         | Type      | Description                                |
| ------------ | --------- | ------------------------------------------ |
| `idx`        | Uint128   | Index of CDP                               |
| `owner`      | HumanAddr | Address of CDP owner                       |
| `collateral` | Asset     | Asset used as collateral                   |
| `asset`      | Asset     | Asset minted by CDP                        |
| `is_short`   | bool      | Determines if CDP is short position or not |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "position": {
    "position_idx": "10000000"
  }
}
```

| Key            | Type    | Description                |
| -------------- | ------- | -------------------------- |
| `position_idx` | Uint128 | Index of position to query |

#### Response

```rust
{
    "position_response": {
        "idx": "100",
        "owner": "terra1...",
        "collateral": {
            "info": {
                "token": {
                    "contract_addr": "terra1..."
                }
            },
            "amount": "1000000"
        },
        "asset": {
            "info": {
                "token": {
                    "contract_addr": "terra1..."
                }
            },
            "amount": "1000000"
        },
        "is_short": true
    }
}
```

| Name         | Type      | Description                                |
| ------------ | --------- | ------------------------------------------ |
| `idx`        | Uint128   | Index of CDP                               |
| `owner`      | HumanAddr | Address of CDP owner                       |
| `collateral` | Asset     | Asset used as collateral                   |
| `asset`      | Asset     | Asset minted by CDP                        |
| `is_short`   | bool      | Determines if CDP is short position or not |
{% endtab %}
{% endtabs %}

### `NextPositionIdx`

Returns the most recent position ID +1.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    NextPositionIdx {}
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug, Default)]
pub struct NextPositionIdxResponse {
    pub next_position_idx: Uint128,
}
```

| Key                 | Type    | Description                              |
| ------------------- | ------- | ---------------------------------------- |
| `next_position_idx` | Uint128 | Index of the next position to be created |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "next_position_idx": {}
}
```

#### Response

```rust
{
    "next_position_idex_response": {
        "next_position_idx": "1000000"
    }
}
```

| Key                 | Type    | Description                              |
| ------------------- | ------- | ---------------------------------------- |
| `next_position_idx` | Uint128 | Index of the next position to be created |
{% endtab %}
{% endtabs %}

### `Positions`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Positions {
        limit: Option<u32>,
        owner_addr: Option<HumanAddr>,
        asset_token: Option<HumanAddr>,
        start_after: Option<Uint128>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "positions": {
    "limit": 8,
    "owner_addr": "terra1...",
    "start_after": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key             | Type      | Description                               |
| --------------- | --------- | ----------------------------------------- |
| `limit`\*       | u32       | Upper bound of number of entries to query |
| `owner_addr*`   | HumanAddr | Owner of positions                        |
| `asset_token`\* | HumanAddr | Contract address of asset token           |
| `start_after`\* | Uint128   | Position index to start at                |

\* = optional
