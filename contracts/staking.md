# Staking

The Staking Contract contains the logic for LP Token staking and reward distribution. Staking rewards for LP stakers come from the new MIR tokens generated at each block by the [Factory Contract](factory.md) and are split between all combined staking pools. The new MIR tokens are distributed in proportion to size of staked LP tokens multiplied by the weight of that asset's staking pool.

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub mirror_token: HumanAddr,
    pub mint_contract: HumanAddr,
    pub oracle_contract: HumanAddr,
    pub terraswap_factory: HumanAddr,
    pub base_denom: String,
    pub premium_min_update_interval: u64,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "owner": "terra1...",
    "mirror_token": "terra1...",
    "mint_contract": "terra1...",
    "oracle_contract": "terra1...",
    "terraswap_factory": "terra1...",
    "base_denom": "uusd",
    "premium_min_update_interval": 8
}
```
{% endtab %}
{% endtabs %}

| Key                           | Type      | Description                                                                                          |
| ----------------------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| `owner`                       | HumanAddr | Owner address of Staking contract                                                                    |
| `mirror_token`                | HumanAddr | Contract address of the Mirror Token (MIR)                                                           |
| `mint_contract`               | HumanAddr | Contract address of Mirror Mint                                                                      |
| `oracle_contract`             | HumanAddr | Contract address of Mirror Oracle                                                                    |
| `terraswap_factory`           | HumanAddr | Contract address of Terraswap Factory                                                                |
| `base_denom`                  | String    | Native token denom for Terraswap pairs (TerraUSD)                                                    |
| `premium_min_update_interval` | u64       | Interval of time denominated in number of blocks which the collateral premium information is updated |

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

| Key      | Type      | Description                                                             |
| -------- | --------- | ----------------------------------------------------------------------- |
| `amount` | Uint128   | Token amount received                                                   |
| `sender` | HumanAddr | Sender of token transaction                                             |
| `msg`\*  | Binary    | Base64-encoded JSON of [Receive Hook](staking.md#receive-hooks) message |

\* = optional

### `UpdateConfig`

Updates the Staking contract's configuration. Can only be issued by the staking contract's owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        premium_min_update_interval: Option<u64>,
    }

```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "update_config": {
        "owner": "terra1...",
        "premium_min_update_interval": 8
    }
}
```
{% endtab %}
{% endtabs %}

| Name                            | Type      | Description                                                                                          |
| ------------------------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| `owner`\*                       | HumanAddr | Owner address of Staking contract                                                                    |
| `premium_min_update_interval`\* | u64       | Interval of time denominated in number of blocks which the collateral premium information is updated |

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

| Key             | Type      | Description                                                    |
| --------------- | --------- | -------------------------------------------------------------- |
| `asset_token`   | HumanAddr | Contract address of mAsset/MIR token (staking pool identifier) |
| `staking_token` | HumanAddr | Contract address of asset's corresponding LP Token             |

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

| Key           | Type      | Description                                                    |
| ------------- | --------- | -------------------------------------------------------------- |
| `amount`      | Uint128   | Amount of LP tokens to unbond                                  |
| `asset_token` | HumanAddr | Contract address of mAsset/MIR token (staking pool identifier) |

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

| Key             | Type      | Description                                                                                                                 |
| --------------- | --------- | --------------------------------------------------------------------------------------------------------------------------- |
| `asset_token`\* | HumanAddr | Contract address of asset token (staking pool identifier). If empty, withdraws all rewards from all staking pools involved. |

\* = optional

### `AutoStake`

When providing liquidity in Mirror Protocol, asset pair is first sent to Staking contract, and it acts as a relay. When defined assets are sent to the contract, the contract provides liquidity to receive LP tokens and starts `AutoStakeHook`.

{% hint style="warning" %}
Note: Executor of the transaction should first increase allowance to spend CW20 tokens
{% endhint %}

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    AutoStake {
        assets: [Asset; 2],
        slippage_tolerance: Option<Decimal>,
    }    
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
"auto_stake": {
  "assets": [
    "info": {
      "native_token": {
        "denom": "uusd",
      }
    },
    "amount": "1000000",
    "info": {
      "token": {
        "contract_address": "terra1..."
      }
    },
    "amount": "1000000",
        ],
        "slippage_tolerance": "1.234567"
    }
}
```
{% endtab %}
{% endtabs %}

| Key                    | Type           | Description                                                 |
| ---------------------- | -------------- | ----------------------------------------------------------- |
| `assets`\*             | Array\[Assets] | Information of assets that are provided into liquidity pool |
| `slippage_tolerance`\* | Decimal        | Maximum price slippage allowed to execute this transaction  |

\*= optional

### `AutoStakeHook`

`[INTERNAL]`

Hook to stake the minted LP tokens from providing liquidity.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    AutoStakeHook {
        asset_token: HumanAddr,
        staking_token: HumanAddr,
        staker_addr: HumanAddr,
        prev_staking_token_amount: Uint128,
    }    
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "auto_stake_hook": {
        "asset_token": "terra1...",
        "staking_token": "terra1...",
        "staker_addr": "terra1...",
        "prev_staking_token_amount: "1000000"
    },
}
```
{% endtab %}
{% endtabs %}

| Key                         | Type      | Description                                              |
| --------------------------- | --------- | -------------------------------------------------------- |
| `assets_token`              | HumanAddr | Contract address of the asset token                      |
| `staking_token`             | HumanAddr | Contract address of the LP token to be staked            |
| `staker_addr`               | HumanAddr | Address of the staker                                    |
| `prev_staking_token_amount` | Uint128   | LP staking balance of the staker before this transaction |

### `AdjustPremium`

`[Permission-less operation]`\
Changes the price premium rate for a specified mAsset, can be done by anyone. Message can be sent again after a defined `premium_min_update_interval`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    AdjustPremium {
        asset_tokens: Vec<HumanAddr>,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "adjust_premium": {
        "asset_tokens": [
            "terra1...", "terra1..."
        ]
    }
}
```
{% endtab %}
{% endtabs %}

| Key            | Type            | Description                                                        |
| -------------- | --------------- | ------------------------------------------------------------------ |
| `asset_tokens` | Vec\<HumanAddr> | Array of token contract addresses to update new price premium rate |

### **`IncreaseShortToken`**

`[Mint contract operation]`\
Increases the total staked supply of `sLP` tokens for a specific staker.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    IncreaseShortToken {
        asset_token: HumanAddr,
        staker_addr: HumanAddr,
        amount: Uint128,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "increase_short_token": {
        "asset_token": "terra1...",
        "staker_addr": "terra1...",
        "amount": "1000000"
    }
}
```
{% endtab %}
{% endtabs %}

| Key           | Type      | Description                            |
| ------------- | --------- | -------------------------------------- |
| `asset_token` | HumanAddr | Token contract address of `sLP`        |
| `staker_addr` | HumanAddr | Address of the `sLP` staker            |
| `amount`      | Uint128   | Amount of `sLP` supply to be increased |

### `DecreaseShortToken`

`[Mint contract operations]`\
Decreases the total staked supply of `sLP` tokens for a specific staker.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    DecreaseShortToken {
        asset_token: HumanAddr,
        staker_addr: HumanAddr,
        amount: Uint128,
    }
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "decrease_short_token": {
        "asset_token": "terra1...",
        "staker_addr": "terra1...", 
        "amount": "1000000"
    }
}
```
{% endtab %}
{% endtabs %}

| Key           | Type      | Description                                                  |
| ------------- | --------- | ------------------------------------------------------------ |
| `asset_token` | HumanAddr | Token contract address of `sLP`                              |
| `staker_addr` | HumanAddr | Address of the `sLP` staker                                  |
| `amount`      | Uint128   | <p>Amount of <code>sLP</code> supply to be decreased<br></p> |

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

| Key           | Type      | Description                                                |
| ------------- | --------- | ---------------------------------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset token (staking pool identifier). |

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

| Key           | Type      | Description                                               |
| ------------- | --------- | --------------------------------------------------------- |
| `asset_token` | HumanAddr | Contract address of asset token (staking pool identifier) |

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
    pub mirror_token: HumanAddr,
    pub mint_contract: HumanAddr,
    pub oracle_contract: HumanAddr,
    pub terraswap_factory: HumanAddr,
    pub base_denom: String,
    pub premium_min_update_interval: u64,
}
```

| Key                           | Type      | Description                                                                                          |
| ----------------------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| `owner`                       | HumanAddr | Owner address of Staking contract                                                                    |
| `mirror_token`                | HumanAddr | Contract address of the Mirror Token (MIR)                                                           |
| `mint_contract`               | HumanAddr | Contract address of Mirror Mint                                                                      |
| `oracle_contract`             | HumanAddr | Contract address of Mirror Oracle                                                                    |
| `terraswap_factory`           | HumanAddr | Contract address of Terraswap Factory                                                                |
| `base_denom`                  | String    | Native token denom for Terraswap pairs (TerraUSD)                                                    |
| `premium_min_update_interval` | u64       | Interval of time denominated in number of blocks which the collateral premium information is updated |
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
        "mirror_token": "terra1...",
        "mint_contract": "terra1...",
        "oracle_contract": "terra1...",
        "terraswap_factory": "terra1...",
        "base_denom": "uusd",
        "premium_min_update_interval": 8
    }
}
```

| Name                          | Type      | Description                                                                                          |
| ----------------------------- | --------- | ---------------------------------------------------------------------------------------------------- |
| `owner`                       | HumanAddr | Owner address of Staking contract                                                                    |
| `mirror_token`                | HumanAddr | Contract address of the Mirror Token (MIR)                                                           |
| `mint_contract`               | HumanAddr | Contract address of Mirror Mint                                                                      |
| `oracle_contract`             | HumanAddr | Contract address of Mirror Oracle                                                                    |
| `terraswap_factory`           | HumanAddr | Contract address of Terraswap Factory                                                                |
| `base_denom`                  | String    | Native token denom for Terraswap pairs (TerraUSD)                                                    |
| `premium_min_update_interval` | u64       | Interval of time denominated in number of blocks which the collateral premium information is updated |
{% endtab %}
{% endtabs %}

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

| Key           | Type      | Description                                    |
| ------------- | --------- | ---------------------------------------------- |
| `asset_token` | HumanAddr | Asset token to query (staking pool identifier) |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PoolInfoResponse {
    pub asset_token: HumanAddr,
    pub staking_token: HumanAddr,
    pub total_bond_amount: Uint128,
    pub total_short_amount: Uint128,
    pub reward_index: Decimal,
    pub short_reward_index: Decimal,
    pub pending_reward: Uint128,
    pub short_pending_reward: Uint128,
    pub premium_rate: Decimal,
    pub short_reward_weight: Decimal,
    pub premium_updated_time: u64,
}
```

| Key                    | Type      | Description                                                                       |
| ---------------------- | --------- | --------------------------------------------------------------------------------- |
| `asset_token`          | HumanAddr | Contract address of mAsset token                                                  |
| `staking_token`        | HumanAddr | Contract address of `LP` or `sLP` token                                           |
| `total_bond_amount`    | Uint128   | Amount of total LP staked                                                         |
| `total_short_amount`   | Uint128   | Amount of total sLP staked                                                        |
| `reward_index`         | Decimal   | Amount of reward distributed per 1 LP token                                       |
| `short_reward_index`   | Decimal   | Amount of reward distributed per 1 sLP token                                      |
| `pending_reward`       | Uint128   | Amount of unclaimed rewards by LP stakers                                         |
| `short_pending_reward` | Uint128   | Amount of unclaimed rewards by sLP stakers                                        |
| `premium_rate`         | Decimal   | Current price premium of the asset pool                                           |
| `short_reward_weight`  | Decimal   | Current reward weight for sLP staking                                             |
| `premium_updated_time` | u64       | Block height which the price premium data for this asset is most recently updated |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "pool_info": {
    "asset_token": "terra1..."
Key
```

| Type          | Description |                                                |
| ------------- | ----------- | ---------------------------------------------- |
| `asset_token` | HumanAddr   | Asset token to query (staking pool identifier) |

#### Response

```javascript
{
    "pool_info_response": {
        "asset_token": "terra1...",
        "staking_token": "terra1...",
        "total_bond_amount": "1000000",
        "total_short_amount": "1000000",
        "reward_index": "123.456789",
        "short_reward_index": "123.456789",
        "pending_reward": "123.456789",
        "short_pending_reward": "123.456789",
        "premium_rate": "0.4",
        "short_reward_weight": "0.4",
        "premium_updated_time": 8
    }
}
```

| Key                    | Type      | Description                                                                       |
| ---------------------- | --------- | --------------------------------------------------------------------------------- |
| `asset_token`          | HumanAddr | Contract address of mAsset token                                                  |
| `staking_token`        | HumanAddr | Contract address of `LP` or `sLP` token                                           |
| `total_bond_amount`    | Uint128   | Amount of total LP staked                                                         |
| `total_short_amount`   | Uint128   | Amount of total sLP staked                                                        |
| `reward_index`         | Decimal   | Amount of reward distributed per 1 LP token                                       |
| `short_reward_index`   | Decimal   | Amount of reward distributed per 1 sLP token                                      |
| `pending_reward`       | Uint128   | Amount of unclaimed rewards by LP stakers                                         |
| `short_pending_reward` | Uint128   | Amount of unclaimed rewards by sLP stakers                                        |
| `premium_rate`         | Decimal   | Current price premium of the asset pool                                           |
| `short_reward_weight`  | Decimal   | Current reward weight for sLP staking                                             |
| `premium_updated_time` | u64       | Block height which the price premium data for this asset is most recently updated |
{% endtab %}
{% endtabs %}

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

| Key             | Type      | Description                                                         |
| --------------- | --------- | ------------------------------------------------------------------- |
| `asset_token`\* | HumanAddr | Asset token to query. If empty, returns info for all staking pools. |
| `staker`        | HumanAddr | Address of staker to query                                          |

\*= optional

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct RewardInfoResponseItem {
    pub asset_token: HumanAddr,
    pub bond_amount: Uint128,
    pub pending_reward: Uint128,
    pub is_short: bool,
}
```

| Key              | Type      | Description                              |
| ---------------- | --------- | ---------------------------------------- |
| `asset_token`    | HumanAddr | Asset token being queried                |
| `bond_amount`    | Uint128   | Amount of LP or sLP tokens staked        |
| `pending_reward` | Uint128   | Amount of reward that are not claimed    |
| `is_short`       | bool      | Identifies of the pool os LP or sLP pool |
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

| Key             | Type      | Description                                                         |
| --------------- | --------- | ------------------------------------------------------------------- |
| `asset_token`\* | HumanAddr | Asset token to query. If empty, returns info for all staking pools. |
| `staker`        | HumanAddr | Address of staker to query                                          |

\*= optional

#### Response

```javascript
{
    "reward_info_response_item": {
        "asset_token": "terra1...",
        "bond_amount": "1000000",
        "pending_reward": "1000000",
        "is_short": true
    }
}
```

| Key              | Type      | Description                              |
| ---------------- | --------- | ---------------------------------------- |
| `asset_token`    | HumanAddr | Asset token being queried                |
| `bond_amount`    | Uint128   | Amount of LP or sLP tokens staked        |
| `pending_reward` | Uint128   | Amount of reward that are not claimed    |
| `is_short`       | bool      | Identifies of the pool os LP or sLP pool |
{% endtab %}
{% endtabs %}
