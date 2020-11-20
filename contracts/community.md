# Community

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

```js
{
  "update_config": {
    "owner": "terra1...",
    "spend_limit": "10000000"
  }
}
```

{% endtab %}
{% endtabs %}

| Key            | Type      | Description |
| -------------- | --------- | ----------- |
| `owner`        | HumanAddr | TO BE ADDED |
| `mirror_token` | HumanAddr | TO BE ADDED |
| `spend_limit`  | Uint128   |             |

## HandleMsg

### `UpdateConfig`

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

```js
{
  "update_config": {
    "owner": "terra1...",
    "spend_limit": "10000000"
  }
}
```

{% endtab %}
{% endtabs %}

| Key             | Type      | Description |
| --------------- | --------- | ----------- |
| `owner`\*       | HumanAddr | TO BE ADDED |
| `spend_limit`\* | Uint128   | TO BE ADDED |

\* = optional

### `Spend`

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

```js
{
  "spend": {
    "amount": "10000000",
    "recipient": "terra1..."
  }
}
```

{% endtab %}
{% endtabs %}

| Key         | Type      | Description |
| ----------- | --------- | ----------- |
| `amount`    | Uint128   | TO BE ADDED |
| `recipient` | HumanAddr | TO BE ADDED |

## QueryMsg

### `Config`

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

{% endtab %}
{% tab title="JSON" %}

```js
{
  "config": {}
}
```

{% endtab %}
{% endtabs %}

| Key | Type | Description |
| --- | ---- | ----------- |

