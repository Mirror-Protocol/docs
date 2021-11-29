# Collateral Oracle

Collateral Oracle contract manages a directory of whitelisted collateral assets, providing the necessary interfaces to register and revoke assets. Mint contract will fetch prices from collateral oracle to determine the C-ratio of each CDP. \
\
The Collateral Oracle  fetches prices from different sources on the Terra ecosystem,  acting as a proxy for Mint Contract.

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub mint_contract: HumanAddr,
    pub base_denom: String,
    pub mirror_oracle: HumanAddr,
    pub anchor_oracle: HumanAddr,
    pub band_oracle: HumanAddr,
}
```
{% endtab %}
{% endtabs %}

| Key             | Type      | Description                                                     |
| --------------- | --------- | --------------------------------------------------------------- |
| `owner`         | HumanAddr | Address of owner                                                |
| `mint_contract` | HumanAddr | Address of Mirror Mint contract                                 |
| `base_denom`    | String    | Asset in which prices will be denominated in (default TerraUSD) |
| `mirror_oracle` | HumanAddr | Address of MIR token oracle feeder                              |
| `anchor_oracle` | HumanAddr | Address of ANC token oracle feeder                              |
| `band_oracle`   | HumanAddr | Address of Band Protocol oracle feeder                          |

## HandleMsg

### `UpdateConfig`

This function can be only issued by the active owner of the Collateral Oracle contract. Changes the configuration of collateral oracle contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        mint_contract: Option<HumanAddr>,
        base_denom: Option<String>,
        mirror_oracle: Option<HumanAddr>,
        anchor_oracle: Option<HumanAddr>,
        band_oracle: Option<HumanAddr>,
    },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "update_config": {
        "owner": "terra1...",
        "mint_contract": "terra1...",
        "base_denom": "uusd",
        "mirror_oracle": "terra1...",
        "anchor_oracle": "terra1...",
        "band_oracle": "terra1..."
    }
}
```
{% endtab %}
{% endtabs %}

| Key               | Type      | Description                                                     |
| ----------------- | --------- | --------------------------------------------------------------- |
| `owner`\*         | HumanAddr | Address of owner                                                |
| `mint_contract`\* | HumanAddr | Address of Mirror Mint contract                                 |
| `base_denom`\*    | String    | Asset in which prices will be denominated in (default TerraUSD) |
| `mirror_oracle`\* | HumanAddr | Address of MIR token oracle feeder                              |
| `anchor_oracle`\* | HumanAddr | Address of ANC token oracle feeder                              |
| `band_oracle`\*   | HumanAddr | Address of Band Protocol oracle feeder                          |

\*= optional

### `RegisterCollateralAsset`

Registers a new type of collateral to be used on Mirror Mint contract for the creation of new CDP. Can only be issued by the owner of Collateral Oracle.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterCollateralAsset {
        asset: AssetInfo,
        price_source: SourceType,
        multiplier: Decimal,
    },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
     "register_collateral_asset": {
         "asset": {
             "token": {
                 "contract_addr": "terra1..."
             }
         },
         "price_source": {
             "terra_oracle": {
                 "terra_oracle_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
             },
             "band_oracle": {
                 "band_oracle_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
             },
             "fixed_price": {
                 "price": "123.123456"
             },
             "terraswap": {
                 "terraswap_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9",
                 "intermediate_denom": "uluna"
             },
             "anchor_market": {
                 "anchor_market_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
             },
             "native": {
                 "native_denom": "uusd"
             }
         }
         "multiplier": "0.05"   
    }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type       | Description                                                                  |
| -------------- | ---------- | ---------------------------------------------------------------------------- |
| `asset`        | AssetInfo  | Asset to be registered                                                       |
| `price_source` | SourceType | Base64-encoded string of JSON of Receive Hook                                |
| `multiplier`   | Decimal    | Multiplied to `min_collateral_ratio` when this asset is chosen as collateral |

#### `SourceType`

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum SourceType {
    TerraOracle {
        terra_oracle_query: Binary,
    },
    BandOracle {
        band_oracle_query: Binary,
    },
    FixedPrice {
        price: Decimal,
    },
    Terraswap {
        terraswap_query: Binary,
        intermediate_denom: Option<String>,
    },
    AnchorMarket {
        anchor_market_query: Binary,
    },
    Native {
        native_denom: String,
    },
} 
```

| Key                    | Type    | Description                                                                                    |
| ---------------------- | ------- | ---------------------------------------------------------------------------------------------- |
| `terra_oracle_query`   | Binary  | Queries information of Terra's oracle                                                          |
| `band_oracle_query`    | Binary  | Queries information of Band Protocol oracle                                                    |
| `price`                | Decimal | Fixed price to be used for the collateral type (aUST = 1 UST)                                  |
| `terraswap_query`      | Binary  | Queries information of Terraswap Pair                                                          |
| `intermediate_denom`\* | String  | Used to calculate UST denominated price of an asset when the asset does not have UST pair pool |
| `anchor_market_query`  | Binary  | Query to fetch information for Anchor Protocol's asset information (ANC)                       |
| `native_denom`         | String  | String denomination of the Terra native asset (uusd)                                           |

### `RevokeCollateralAsset`

Removes registered collateral so that it is no longer used as collateral for Mirror Mint. Can only be issued by the owner of Collateral Oracle.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RevokeCollateralAsset {
        asset: AssetInfo,
    },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "revoke_collateral_asset": {
        "asset": {
            "token": {
                "contract_addr": "terra1..."
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

| Key     | Type      | Description         |
| ------- | --------- | ------------------- |
| `asset` | AssetInfo | Asset to be revoked |

### `UpdateCollateralPriceSource`

Updates the price data source for a specific collateral asset. Can only be issued by the owner of Collateral Oracle.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateCollateralPriceSource {
        asset: AssetInfo,
        price_source: SourceType,
    },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "update_collateral_query": {
        "asset": {
            "token": {
                "contract_addr": "terra1..."
            }
        },
         "price_source": {
             "terra_oracle": {
                 "terra_oracle_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
             },
             "band_oracle": {
                 "band_oracle_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
             },
             "fixed_price": {
                 "price": "123.123456"
             },
             "terraswap": {
                 "terraswap_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9",
                 "intermediate_denom": "uluna"
             },
             "anchor_market": {
                 "anchor_market_query": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
             },
             "native": {
                 "native_denom": "uusd"
             }
         }
    }
}   
```
{% endtab %}
{% endtabs %}

| Key            | Type       | Description                                             |
| -------------- | ---------- | ------------------------------------------------------- |
| `asset`        | AssetInfo  | Asset to update query information                       |
| `price_source` | SourceType | Message detailing where to query asset information from |

### `UpdateCollateralMultiplier`

Updates the multiplier parameter of a specific collateral asset registered in Mirror contract. Can only be issued by the owner of Collateral Oracle.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]    
pub enum HandleMsg {
    UpdateCollateralPremium {
        asset: AssetInfo,
        multiplier: Decimal,
    },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "update_collateral_premium": {
        "asset": {
            "token": {
                "contract_addr": "terra1..."
            }
        },
        "multiplier": "2.00"
    }
}
```
{% endtab %}
{% endtabs %}

| Key          | Type      | Description                                                                                           |
| ------------ | --------- | ----------------------------------------------------------------------------------------------------- |
| `asset`      | AssetInfo | Asset to change collateral premium                                                                    |
| `multiplier` | Decimal   | Collateral ratio multiplied to `min_collateral_ratio` of the CDP being created by choosing this asset |

## QueryMsg

### `Config`

Queries the configuration of Collateral Oracle.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
    pub enum QueryMsg {
        Config {},
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr,
    pub mint_contract: HumanAddr,
    pub base_denom: String,
    pub mirror_oracle: HumanAddr,
    pub anchor_oracle: HumanAddr, 
    pub band_oracle: HumanAddr,
}
```

| Key             | Type      | Description                                                     |
| --------------- | --------- | --------------------------------------------------------------- |
| `owner`         | HumanAddr | Address of owner                                                |
| `mint_contract` | HumanAddr | Address of Mirror Mint contract                                 |
| `base_denom`    | String    | Asset in which prices will be denominated in (default TerraUSD) |
| `mirror_oracle` | HumanAddr | Address of MIR token oracle feeder                              |
| `anchor_oracle` | HumanAddr | Address of ANC token oracle feeder                              |
| `band_oracle`   | HumanAddr | Address of Band Protocol oracle feeder                          |
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
        "mint_contract": "terra1...",
        "base_denom": "uusd",
        "mirror_oracle": "terra1...",
        "anchor_oracle": "terra1...",
        "band_oracle": "terra1..."
    }
}
```

| Key             | Type      | Description                                                     |
| --------------- | --------- | --------------------------------------------------------------- |
| `owner`         | HumanAddr | Address of owner                                                |
| `mint_contract` | HumanAddr | Address of Mirror Mint contract                                 |
| `base_denom`    | String    | Asset in which prices will be denominated in (default TerraUSD) |
| `mirror_oracle` | HumanAddr | Address of MIR token oracle feeder                              |
| `anchor_oracle` | HumanAddr | Address of ANC token oracle feeder                              |
| `band_oracle`   | HumanAddr | Address of Band Protocol oracle feeder                          |
{% endtab %}
{% endtabs %}

### `CollateralPrice`

Returns the UST price of the selected collateral asset.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    CollateralPrice {
        asset: String,
    },
}
```

| Key     | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| `asset` | String | Name of the collateral asset to query prices |

#### Response

Returns the UST price of the selected collateral asset.

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct CollateralPriceResponse {
    pub asset: String,
    pub rate: Decimal,
    pub last_updated: u64
    pub multiplier: Decimal,
    pub is_revoked: bool,
}
```

| Key                  | Type    | Description                                                                                      |
| -------------------- | ------- | ------------------------------------------------------------------------------------------------ |
| `asset`              | String  | Name of the collateral asset to query prices                                                     |
| `rate`               | Decimal | Current price of the collateral                                                                  |
| `collateral_premium` | Decimal | Collateral ratio added to `min_collateral_ratio` of the CDP being created by choosing this asset |
| `is_revoked`         | bool    | Check if the collateral is registered or revoked                                                 |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "collateral_price": {
        "asset": "uluna"
    }
}
```

| Key     | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| `asset` | String | Name of the collateral asset to query prices |

#### Response

Returns the UST price of the selected collateral asset.

```rust
{
    "collateral_price_response": {
        "asset": "uluna",
        "rate": "123.456789",
        "last_updated": 8
        "multiplier": "2.0",
        "is_revoked": false
    }
}
```

| Key            | Type    | Description                                                                                           |
| -------------- | ------- | ----------------------------------------------------------------------------------------------------- |
| `asset`        | String  | Name of the collateral asset to query prices                                                          |
| `rate`         | Decimal | Current price of the collateral                                                                       |
| `last_updated` | Decimal | Time when the asset price was last updated                                                            |
| `multiplier`   | Decimal | Collateral ratio multiplied to `min_collateral_ratio` of the CDP being created by choosing this asset |
| `is_revoked`   | bool    | Check if the collateral is registered or revoked                                                      |
{% endtab %}
{% endtabs %}

### `CollateralAssetInfo`

Returns the parameter of selected collateral.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    CollateralAssetInfo {
        asset: String,
    },
}
```

| Key     | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| `asset` | String | Name of the collateral asset to query prices |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct CollateralInfoResponse {
    pub asset: String,
    pub multiplier: Decimal,
    pub source_type: String,
    pub is_revoked: bool,
}
```

| Key                  | Type      | Description                                                                                                                                       |
| -------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `asset`              | String    | Name of the collateral asset to query prices                                                                                                      |
| `collateral_premium` | Decimal   | Collateral ratio added to `min_collateral_ratio` of the CDP being created by choosing this asset                                                  |
| `source_type`        | WasmQuery | Queries the public API of another contract at a known address (with known ABI) return value is whatever the contract returns (caller should know) |
| `is_revoked`         | bool      | Check if the collateral is registered or revoked                                                                                                  |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "collateral_asset_info": {
        "asset": "uluna"
    }
}
```

| Key     | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| `asset` | String | Name of the collateral asset to query prices |

#### Response

```rust
{
    "collateral_asset_info_response": {
        "asset": "uluna",
        "multiplier": "2.0",
        "source_type": "WasmQuery",
        "is_revoked": false
    }
}
```

| Key           | Type    | Description                                                                                                                                       |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `asset`       | String  | Name of the collateral asset to query prices                                                                                                      |
| `multiplier`  | Decimal | Collateral ratio multiplied to `min_collateral_ratio` of the CDP being created by choosing this asset                                             |
| `source_type` | String  | Queries the public API of another contract at a known address (with known ABI) return value is whatever the contract returns (caller should know) |
| `is_revoked`  | bool    | Check if the collateral is registered or revoked                                                                                                  |
{% endtab %}
{% endtabs %}

### `CollateralAssetInfos`

Returns parameters for multiple collateral assets.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    CollateralAssetInfos {},
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct CollateralInfosResponse {
    pub collaterals: Vec<CollateralInfoResponse>,
}
```

| Key           | Type                         | Description                       |
| ------------- | ---------------------------- | --------------------------------- |
| `collaterals` | Vec\<CollateralInfoResponse> | Array of `CollateralInfoResponse` |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "collateral_asset_infos": {}
}
```

#### Response

```rust
{
    "collateral_infos_respose": {
        "collaterals": [
            [
                "asset": "uluna",
                "collateral_premium": "2.0",
                "query_request": "WasmQuery",
                "is_revoked": false
            ]
        ...
    }
}
```

| Key           | Type                         | Description                       |
| ------------- | ---------------------------- | --------------------------------- |
| `collaterals` | Vec\<CollateralInfoResponse> | Array of `CollateralInfoResponse` |
{% endtab %}
{% endtabs %}

