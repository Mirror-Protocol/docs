---
description: 'TODO: Band Protocol information'
---

# Oracle

The Oracle Contract exposes an interface for accessing the latest reported price for mAssets. Price quotes are kept up-to-date by oracle feeders that are tasked with periodically fetching exchange rates from reputable sources and reporting them to the Oracle contract.

Prices are only considered valid for 60 seconds. If no new prices are published after the data has expired, Mirror will disable CDP operations \(mint, burn, deposit, withdraw\) until the price feed resumes.

## InitMsg

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub base_asset_info: AssetInfo,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of the owner who can register new assets |
| `base_asset_info` | AssetInfo | Asset in which prices will be denominated \(default TerraUSD\) |

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

Registers a new asset with the oracle, enabling a price feed for the asset. The feeder account responsible for reporting the price is assigned at this step.

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


### `Asset`

Get asset token details, such as designated oracle feeder.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Asset {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "asset": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token to query |

### `Price`

Get price information for the specified mAsset.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Price {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "price": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token to query |

### `Prices`

Get price information for all registered mAssets.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Prices {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "prices": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |


