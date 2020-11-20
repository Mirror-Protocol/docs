# Collector

The Collector accumulates fee rewards generated from closing CDPs within the protocol, and converts them into UST in order to purchase MIR from the MIR-UST Terraswap pool. The MIR is then sent to the [Gov Contract](gov.md) to supply trading fee rewards for MIR stakers.

## InitMsg

{% tabs %}
{% tab title="Rust" %}

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub distribution_contract: HumanAddr,
    pub terraswap_factory: HumanAddr,
    pub mirror_token: HumanAddr,
    pub base_denom: String,
}
```

{% endtab %}

{% tab title="JSON" %}

```javascript
{
  "distribution_contract": "terra1...",
  "terraswap_factory": "terra1...",
  "mirror_token": "terra1...",
  "base_denom": "uusd" // native Terra token
}
```

{% endtab %}
{% endtabs %}

| Key                     | Type      | Description                                     |
| :---------------------- | :-------- | :---------------------------------------------- |
| `distribution_contract` | HumanAddr | Contract address of [Mirror Governance](gov.md) |
| `terraswap_factory`     | HumanAddr | Contract address of Terraswap Factory           |
| `mirror_token`          | HumanAddr | Contract address of Mirror Token \(MIR\)        |
| `base_denom`            | String    | Base denomination \(native Terra token denom\)  |

## HandleMsg

### `**Convert`

Converts the contract's balance of the specified asset token into `base_denom` through its Terraswap pool. However, if the given asset token is the Mirror Token \(MIR\), converts the contract's entire balance of `base_denom` into MIR.

{% tabs %}
{% tab title="Rust" %}

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Convert {
        asset_token: HumanAddr,
    }
}
```

{% endtab %}

{% tab title="JSON" %}

```javascript
{
  "convert": {
    "asset_token": "terra1..."
  }
}
```

{% endtab %}
{% endtabs %}

| Key           | Type      | Description                          |
| :------------ | :-------- | :----------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset to convert |

### `Send`

Sends to entire balance of collector's Mirror Tokens \(MIR\) to `distribution_contract`

{% tabs %}
{% tab title="Rust" %}

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Send {}
}
```

{% endtab %}

{% tab title="JSON" %}

```javascript
{
  "send": {}
}
```

{% endtab %}
{% endtabs %}

## QueryMsg

### `**Config`

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
| :-- | :--- | :---------- |

