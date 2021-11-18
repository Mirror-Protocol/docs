# Lock

The Lock contract is responsible for locking up UST returned from shorting a mAsset through [Mirror Mint](../user-guide/getting-started/mint-and-burn.md) operation.&#x20;

## InitMsg

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub mint_contract: HumanAddr,
    pub base_denom: String,
    pub lockup_period: u64,
}
```

| Key             | Type      | Description                                                              |
| --------------- | --------- | ------------------------------------------------------------------------ |
| `owner`         | HumanAddr | Owner address of Mirror Lock                                             |
| `mint_contract` | HumanAddr | Address of Mirror Mint                                                   |
| `base_denom`    | String    | Native token denomination for stablecoin (TerraUSD)                      |
| `lockup_period` | u64       | Length of time in seconds which the UST from shorting will be locked for |

## HandleMsg

### `UpdateConfig`

Updates the configuration of Lock contract. Can only be issued by the owner of the Mirror Lock.

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
        lockup_period: Option<u64>,
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
        "lockup_period": 8
    }
}
```
{% endtab %}
{% endtabs %}

| Key               | Type      | Description                                                               |
| ----------------- | --------- | ------------------------------------------------------------------------- |
| `owner`\*         | HumanAddr | Owner address of Mirror Lock                                              |
| `mint_contract`\* | HumanAddr | Address of Mirror Mint                                                    |
| `base_denom`\*    | String    | Native token denomination for stablecoin (TerraUSD)                       |
| `lockup_period`\* | u64       | Length of time in seconds which the UST from shorting witll be locked for |

\*= optional

### `LockPositionFundsHook`

Locks the UST from shorting mAsset when short CDP is successfully created on Mirror Mint.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    LockPositionFundsHook {
        position_idx: Uint128,
        receiver: HumanAddr,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "lock_position_funds_hook": {
        "position_idx": "10",
        "receiver": "terra1..."
    }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type      | Description                                     |
| -------------- | --------- | ----------------------------------------------- |
| `position_idx` | Uint128   | ID number of CDP from Mirror Mint               |
| `receiver`     | HumanAddr | Creator of CDP, who will receive unlocked funds |

### `UnlockPositionFunds`

Locked UST from`LockPositionFundsHook`is unlocked by sending this message after`lockup_period`has passed. Can only be issued by the owner of the position (`receiver`).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UnlockPositionFunds {
        position_idx: Uint128,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "unlock_position_funds": {
        "position_idx": "10"
    }
}    
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| `position_idx` | Uint128 | ID number of CDP from Mirror Mint to unlock funds from |

### `ReleasePositionFunds`

Locked funds will be released and sent to the CDP creator upon closing of the position. This message unlocks funds even when`lock_period`has not ended yet. Can only be issued by the mint contract when the position is being closed.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ReleasePositionFunds {
        position_idx: Uint128,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "release_position_funds": {
        "position_idx": "10"
    }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| `position_idx` | Uint128 | ID number of CDP from Mirror Mint to unlock funds from |

## QueryMsg

### `Config`

Returns the configuration of Mirror Lock.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr,
    pub mint_contract: HumanAddr,
    pub base_denom: String,
    pub lockup_period: u64,
}
```

| Key             | Type      | Description                                                              |
| --------------- | --------- | ------------------------------------------------------------------------ |
| `owner`         | HumanAddr | Owner address of Mirror Lock                                             |
| `mint_contract` | HumanAddr | Address of Mirror Mint                                                   |
| `base_denom`    | String    | Native token denomination for stablecoin (TerraUSD)                      |
| `lockup_period` | u64       | Length of time in seconds which the UST from shorting will be locked for |
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
        "owner": "terra1...",
        "mint_contract": "terra1...",
        "base_denom": "uusd",
        "lockup_period": 10
    }
}
```

| Key             | Type      | Description                                                     |
| --------------- | --------- | --------------------------------------------------------------- |
| `owner`         | HumanAddr | Owner address of Mirror Lock                                    |
| `mint_contract` | HumanAddr | Address of Mirror Mint                                          |
| `base_denom`    | String    | Native token denomination for stablecoin (TerraUSD)             |
| `lockup_period` | u64       | Number of blocks which the UST from shorting will be locked for |
{% endtab %}
{% endtabs %}

### `PositionLockInfo`

Returns information about locked funds of a specific CDP.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    PositionLockInfo {
        position_idx: Uint128,
    }
```

| Key            | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| `position_idx` | Uint128 | ID number of CDP from Mirror Mint to unlock funds from |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PositionLockInfoResponse {
    pub idx: Uint128,
    pub receiver: HumanAddr,
    pub locked_amount: Uint128,
    pub unlock_time: u64,
}
```

| Key             | Type      | Description                                             |
| --------------- | --------- | ------------------------------------------------------- |
| `idx`           | Uint128   | ID number of CDP from Mirror Mint to unlock funds from  |
| `receiver`      | HumanAddr | Creator of CDP, who will receive unlocked funds         |
| `locked_amount` | Uint128   | Amount of `base_denom` locked from creating a Short CDP |
| `unlock_time`   | u64       | Time when user is allowed to claim the `locked_amount`  |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "position_lock_info": {
        "position_idx": "10"
    }
}
```

| Key            | Type    | Description                                            |
| -------------- | ------- | ------------------------------------------------------ |
| `position_idx` | Uint128 | ID number of CDP from Mirror Mint to unlock funds from |

#### Response

```rust
{
    "position_lock_info_response": {
        "idx": "10",
        "receiver": "terra1...",
        "locked_funds": [
            [100, "1400"]
        ]
    }
}
```

| Key            | Type                | Description                                                                                                                                           |
| -------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `idx`          | Uint128             | ID number of CDP from Mirror Mint to unlock funds from                                                                                                |
| `receiver`     | HumanAddr           | Creator of CDP, who will receive unlocked funds                                                                                                       |
| `locked_funds` | Vec<(u64, Uint128)> | <p>Description about <br>1. Block height which the fund was locked at<br>2. Amount of <code>base_denom</code> locked<br>from creating a Short CDP</p> |
{% endtab %}
{% endtabs %}

