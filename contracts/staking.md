# Staking

The Staking Contract contains the logic for LP Token staking and reward distribution. Staking rewards for LP stakers come from the new MIR tokens generated at each block by the [Factory Contract](factory.md) and are split between all combined staking pools. The new MIR tokens are distributed in proportion to size of staked LP tokens multiplied by the weight of that asset's staking pool.

## Config

| Name | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of owner of staking contract |
| `mirror_token` | HumanAddr | Contract address of the Mirror Token \(MIR\) |

## InitMsg

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub mirror_token: HumanAddr,
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of staking contract owner |
| `mirror_token` | HumanAddr | Contract address of the Mirror Token \(MIR\) |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Mint contract is the recipient. Allows the token transfer to execute a [Receive Hook](staking.md#receive-hooks) as a subsequent action within the same transaction.

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
| `amount` | Uint128 | Token amount received |
| `sender` | HumanAddr | Sender of token transaction |
| `msg`\* | Binary | Base64-encoded JSON of [Receive Hook](staking.md#receive-hooks) message |

\* = optional

### `UpdateConfig`

Updates the Staking contract's configuration. Can only be issued by the staking contract's owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct enum HandleMsg {
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
| `owner`\* | HumanAddr | Address of new owner of staking contract |

\* = optional

### `RegisterAsset`

Registers a new staking pool for an asset token and associates the LP token with the staking pool.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterAsset {
        asset_token: HumanAddr,
        staking_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_asset": {
    "asset_token": "terra1...",
    "staking_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of mAsset/MIR token \(staking pool identifier\) |
| `staking_token` | HumanAddr | Contract address of asset's corresponding LP Token |

### `Unbond`

Users can issue the unbond message at any time to remove their staked LP tokens from a staking position.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Unbond {
        amount: Uint128,
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "unbond": {
    "amount": "10000000",
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of LP tokens to unbond |
| `asset_token` | HumanAddr | Contract address of mAsset/MIR token \(staking pool identifier\) |

### `Withdraw`

Withdraws a user's rewards for a specific staking position.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Withdraw {
        asset_token: Option<HumanAddr>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token`\* | HumanAddr | Contract address of asset token \(staking pool identifier\). If empty, withdraws all rewards from all staking pools involved. |

\* = optional

## Receive Hooks

### `Bond`

{% hint style="danger" %}
**WARNING**

If you send LP Tokens to the Staking contract without issuing this hook, they will not be staked and will **BE LOST FOREVER.**
{% endhint %}

Can be issued when the user sends LP Tokens to the Staking contract. The LP token must be recognized by the staking pool of the specified asset token.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Bond {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "bond": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token \(staking pool identifier\). |

### `DepositReward`

`[INTERNAL]`

Can be issued when the user sends MIR tokens to the Staking contract, which will be used as rewards for the specified asset's staking pool. Used by [Factory Contract](factory.md) to deposit newly minted MIR tokens.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    DepositReward {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit_reward": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token \(staking pool identifier\) |

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


### `PoolInfo`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    PoolInfo {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "pool_info": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Asset token to query \(staking pool identifier\) |

### `RewardInfo`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    RewardInfo {
        asset_token: Option<HumanAddr>,
        staker: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "reward_info": {
    "asset_token": "terra1...",
    "staker": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token`\* | HumanAddr | Asset token to query. If empty, returns info for all staking pools. |
| `staker` | HumanAddr | Address of staker to query |

\* = optional

