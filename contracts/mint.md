# Mint

The Mint Contract implements the logic for [Collateralized Debt Positions ](../protocol/mirrored-assets-massets.md#collateralized-debt-position)\(CDPs\), through which users can mint new mAsset tokens against their deposited collateral \(UST or mAssets\). Current prices of collateral and minted mAssets are read from the [Oracle Contract](oracle.md) determine the C-ratio of each CDP. The Mint Contract also contains the logic for liquidating CDPs with C-ratios below the minimum for their minted mAsset through auction.

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub oracle: HumanAddr,
    pub base_asset_info: AssetInfo,
    pub token_code_id: u64,
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
  "owner": "terra1...",
  "oracle": "terra1...",
  "base_asset_info": {
    "asset": {
      "native_token": {
        "denom": "uusd"
      }
    }
  },
  "token_code_id": 8,
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Account that is permitted to change contract config |
| `oracle` | HumanAddr | Contract address of [Mirror Oracle](oracle.md) |
| `base_asset_info` | AssetInfo | Asset used for denomination \(TerraUSD\) |
| `token_code_id` | u64 | Code ID for asset token contract |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of tokens received |
| `sender` | HumanAddr | Sender of the token transfer |
| `msg`\* | Binary | Base64-encoded string of JSON of [Receive Hook](mint.md#receive-hooks) |

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
        token_code_id: Option<u64>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...",
    "token_code_id": 8
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | Address of new owner for mint contract |
| `token_code_id`\* | u64 | Code ID of new token contract |

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
    }
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
    "min_collateral_ratio": "123.456789"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_info` | AssetInfo | Asset to be updated |
| `auction_discount`\* | Decimal | New auction discount rate |
| `min_collateral_ratio`\* | Decimal | New minimum collateralization ratio |

\* = optional

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
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_asset": {
    "asset_token": "terra1...",
    "auction_discount": "123.456789",
    "min_collateral_ratio": "123.456789"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Asset to be registered |
| `auction_discount` | Decimal | Auction discount rate |
| `min_collateral_ratio` | Decimal | Minimum collateralization ratio |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | TO BE ADDED |
| `end_price` | Decimal | TO BE ADDED |

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
        asset_info: AssetInfo,
        collateral: Asset,
        collateral_ratio: Decimal,
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
    "asset_info": {
      "token": {
        "contract_addr": "terra1..."
      }
    },
    "collateral": {
      "info": {
        "token": {
          "contract_address": "terra1..."
        }
      },
      "amount": "1000000"
    },
    "collateral_ratio": "123.456789"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_info` | AssetInfo | Asset to be minted by CDP |
| `collateral` | Asset | Initial collateral deposit for the CDP |
| `collateral_ratio` | Decimal | Initial desired collateralization ratio |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral` | Asset | Collateral amount to be deposited |
| `position_idx` | Uint128 | Index of position |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral` | Asset | Collateral to withdraw |
| `position_idx` | Uint128 | Index of position |

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
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "mint": {
    "asset": {
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

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset` | Asset | mAssets to be minted |
| `position_idx` | Uint128 | Index of position |

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
    "collateral_ratio": "123.456789"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_info` | AssetInfo | mAsset to be minted by CDP |
| `collateral_ratio` | Decimal | Initial collateralization ratio to use |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `position_idx` | Uint128 | Index of position |

### `Burn`

Issued when a user sends mAsset tokens to the Mint contract.

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `position_idx` | Uint128 | Index of position |

### `Auction`

Issued when a user sends mAsset tokens to the Mint contract.

Purchases the collateral of a CDP subject to liquidation \(whose C-ratio has fallen under its minted mAsset's minimum\). The buyer cannot pay more than the CDP's current minted mAsset balance.

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

| Key | Type | Description |
| :--- | :--- | :--- |
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
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "config": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |


### `AssetConfig`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    AssetConfig {
        asset_info: AssetInfo,
    }
}
```
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
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_info` | AssetInfo | Asset to query |

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
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "position": {
    "position_idx": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `position_idx` | Uint128 | Index of position to query |

### `Positions`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Positions {
        limit: Option<integer>,
        owner_addr: HumanAddr,
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

| Key | Type | Description |
| :--- | :--- | :--- |
| `limit`\* | integer | Upper bound of number of entries to query |
| `owner_addr` | HumanAddr | Owner of positions |
| `start_after`\* | Uint128 | Position index to start at |

\* = optional

