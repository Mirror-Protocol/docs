# Oracle

The Oracle Contract exposes an interface for accessing the latest reported price for mAssets. Price quotes are kept up-to-date by oracle feeders that are tasked with periodically fetching exchange rates from reputable sources and reporting them to the Oracle contract.

Prices are only considered valid for 60 seconds. If no new prices are published after the data has expired, Mirror will disable CDP operations \(mint, burn, deposit, withdraw\) until the price feed resumes.

## InitMsg

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub base_asset: String,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of the owner who can register new assets |
| `base_asset` | String | Asset in which prices will be denominated \(default TerraUSD\) |

## HandleMsg

### `UpdateConfig`

This function can only be issued by the active owner of the Oracle contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | Address of new owner |

\* = optional

### `RegisterAsset`

Registers a new asset with the oracle, enabling a price feed for the asset. The feeder account responsible for reporting the price is assigned at this step. Can also be used to update an existing asset.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterAsset {
        asset_token: HumanAddr,
        feeder: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_asset": {
    "asset_token": "terra1...",
    "feeder": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token |
| `feeder` | HumanAddr | Address of Oracle Feeder for the asset |

### `FeedPrice`

Publish a price for one or multiple assets. Caller should be the designated oracle feeder for each of the assets for which price information is provided.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    FeedPrice {
        prices: Vec<(HumanAddr, Decimal)>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "feed_price": {
    "prices": [
      ["terra1...", "123.456789"],
      ["terra1...", "123.456789"]
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `prices` | Vec&lt;\(HumanAddr, Decimal\)&gt; | Price information for assets |

## QueryMsg

### `Config`

Get the Mirror Oracle contract configuration.

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
    pub base_asset: String,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Owner address |
| `base_asset` | String/`'uusd'` | Asset in which prices will be denominated \(default TerraUSD\) |
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
        "base_asset": "uusd"
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Owner address |
| `base_asset` | String/`'uusd'` | Asset in which prices will be denominated \(default TerraUSD\) |
{% endtab %}
{% endtabs %}

### `Feeder`

Get asset token details, such as designated oracle feeder.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Feeder {
        asset_token: HumanAddr,
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token to query |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct FeederResponse {
    pub asset_token: HumanAddr,
    pub feeder: HumanAddr,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token to query |
| `feeder` | HumanAddr | Terra address of price feeder |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "asset": {
    "asset_token": "terra1..."
  }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token to query |

#### Response

```rust
{
    "feeder_response": {
        "asset_token": "terra1...",
        "feeder": "terra1..."
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token to query |
| `feeder` | HumanAddr | Terra address of price feeder |
{% endtab %}
{% endtabs %}

### `Price`

Get price information for the specified mAsset.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Price {
        base_asset: String,
        quote_asset: String
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `base_asset` | HumanAddr | Asset for which to get price |
| `quote_asset` | HumanAddr / `'uusd'` | Asset in which price will be denominated |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PriceResponse {
    pub rate: Decimal,
    pub last_updated_base: u64,
    pub last_updated_quote: u64,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `rate` | HumanAddr | Asset for which to get price |
| `last_updated_base` | u64 | Block height which the `base_asset` price has been updated at |
| `last_updated_quote` | u64 | Block height which the `quote_asset` price has been updated at |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "price": {
    "base_asset": "terra1...",
    "quote_asset": "uusd"
  }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `base_asset` | HumanAddr | Asset for which to get price |
| `quote_asset` | HumanAddr / `'uusd'` | Asset in which price will be denominated |

#### Response

```rust
{
    "price_response": {
        "rate": "123.456789",
        "last_updated_base": 10
        "last_updated_quote": 10
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `rate` | HumanAddr | Asset for which to get price |
| `last_updated_base` | u64 | Block height which the `base_asset` price has been updated at |
| `last_updated_quote` | u64 | Block height which the `quote_asset` price has been updated at |
{% endtab %}
{% endtabs %}

### `Prices`

Get price information for all registered mAssets.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Prices {
        start_after: Option<HumanAddr>,
        limit: Option<u32>
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr | Contract address to start query from |
| `limit` | u32 | Max number of results to report |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PricesResponseElem {
    pub asset_token: HumanAddr,
    pub price: Decimal,
    pub last_updated_time: u64,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address to start query from |
| `price` | Decimal | Current price of the `asset_token`  |
| `last_updated_time` | u64 | Block height which the`asset_token`price has been updated at |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "prices": {
    "start_after": "terra1...",
    "limit": 8
  }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr | Contract address to start query from |
| `limit` | u32 | Max number of results to report |

#### Response

```rust
{
    "prices_response": {
        "asset_token": "terra1...",
        "price": "123.456789",
        "last_updated_time": 10,
    }
    ...
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address to start query from |
| `price` | Decimal | Current price of the `asset_token`  |
| `last_updated_time` | u64 | Block height which the`asset_token`price has been updated at |
{% endtab %}
{% endtabs %}

