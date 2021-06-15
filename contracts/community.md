# Community

The Community Contract holds the funds of the [Community Pool](../protocol/governance/#community-pool), which can be spent through a governance poll. 

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub mirror_token: HumanAddr,
    pub spend_limit: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner": "terra1...",
  "mirror_token": "terra1...",
  "spend_limit": "123456",
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Owner address |
| `mirror_token` | HumanAddr | Contract address of the Mirror Token |
| `spend_limit` | Uint128 | Max amount of disbursement |

## HandleMsg

### `UpdateConfig`

Can only be issued by the owner. Updates the Community Contract's configuration.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        spend_limit: Option<Uint128>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...",
    "spend_limit": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | New contract owner |
| `spend_limit`\* | Uint128 | New spending limit |

\* = optional

### `Spend`

Can only be issued by the owner. Sends the amount of MIR tokens to the designated recipient. 

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Spend {
        amount: Uint128,
        recipient: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "spend": {
    "amount": "10000000",
    "recipient": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of MIR in contract's balance to send |
| `recipient` | HumanAddr | Recipient of the funds |

## QueryMsg

### `Config`

Gets the Mirror Community contract's configuration.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {
    }
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr,
    pub mirror_token: HumanAddr,
    pub spend_limit: Uint128,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Owner address |
| `mirror_token` | HumanAddr | Contract address of the Mirror Token |
| `spend_limit` | Uint128 | Max amount of disbursement |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "config": {}
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr,
    pub mirror_token: HumanAddr,
    pub spend_limit: Uint128,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Owner address |
| `mirror_token` | HumanAddr | Contract address of the Mirror Token |
| `spend_limit` | Uint128 | Max amount of disbursement |
{% endtab %}
{% endtabs %}

