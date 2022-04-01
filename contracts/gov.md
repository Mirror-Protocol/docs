# Gov

The Gov Contract contains logic for holding polls and Mirror Token (MIR) staking, and allows the Mirror Protocol to be governed by its users in a decentralized manner. After the initial bootstrapping of Mirror Protocol contracts, the Gov Contract is assigned to be the owner of itself and [Mirror Factory.](factory.md)

New proposals for change are submitted as polls, and are voted on by MIR stakers through the [voting procedure](../protocol/governance/). Polls can contain messages that can be executed directly without changing the Mirror Protocol code.

The Gov Contract keeps a balance of MIR tokens, which it uses to reward stakers with funds it receives from trading fees sent by the [Mirror Collector](collector.md) and user deposits from creating new governance polls. This balance is separate from the [Community Pool](../protocol/governance/#community-pool), which is held by the [Community](community.md) contract (owned by the Gov contract).

## Config

| Key                | Type      | Description                                                                                                                  |
| ------------------ | --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `mirror_token`     | HumanAddr | Contract address of Mirror Token (MIR)                                                                                       |
| `quorum`           | Decimal   | Minimum percentage of participation required for a poll to pass                                                              |
| `threshold`        | Decimal   | Minimum percentage of `yes` votes required for a poll to pass                                                                |
| `voting_period`    | u64       | Number of seconds during which votes can be cast                                                                             |
| `proposal_deposit` | Uint128   | MIR deposit required for a new poll to be submitted                                                                          |
| `effective_delay`  | u64       | Number of seconds after a poll passes to apply changes                                                                       |
| `voter_weight`     | Decimal   | Ratio of protocol fee which will be distributed among the governance poll voters                                             |
| `snapshot_period`  | u64       | Minimum number of blocks before the end of voting period which snapshot could be taken to lock the current quorum for a poll |
| `admin_manager`    | String    | Address of the admin manager contract                                                                                        |
| `poll_gas_limit`   | u64       | Maximum amount of gas which a poll can consume during its execution                                                          |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InstantiateMsg {
    pub mirror_token: String,
    pub effective_delay: u64,
    pub default_poll_config: PollConfig,
    pub migration_poll_config: PollConfig,
    pub auth_admin_poll_config: PollConfig,
    pub voter_weight: Decimal,
    pub snapshot_period: u64,
    pub admin_manager: String,
    pub poll_gas_limit: u64,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct PollConfig {
    pub proposal_deposit: Uint128,
    pub voting_period: u64,
    pub quorum: Decimal,
    pub threshold: Decimal,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "mirror_token": "terra1...",
    "effective_delay": 8
    "default_poll_config": {
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8
        "proposal_deposit": "100000000"
        },
    "migration_poll_config": {
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8
        "proposal_deposit": "100000000"
        },
    "auth_admin_poll_config": {
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8
        "proposal_deposit": "100000000"
        },
    "voter_weight": "0.5",
    "snapshot_period": 8,
    "admin_manager": "terra1...",
    "poll_gas_limit": 8
} 
```
{% endtab %}
{% endtabs %}

| Key                      | Type       | Description                                                                                                                  |
| ------------------------ | ---------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `mirror_token`           | HumanAddr  | Contract address of Mirror Token (MIR)                                                                                       |
| `effective_delay`        | u64        | Minimum percentage of participation required for a poll to pass                                                              |
| `default_poll_config`    | PollConfig | `PollConfig` for default poll types                                                                                          |
| `migration_poll_config`  | PollConfig | `PollConfig` for migration poll types                                                                                        |
| `auth_admin_poll_config` | PollConfig | `PollConfig` for governance configuration change and admin key transfer polls                                                |
| `effective_delay`        | u64        | Number of blocks after a poll passes to apply changes                                                                        |
| `voter_weight`           | Decimal    | Ratio of protocol fee which will be distributed among the governance poll voters                                             |
| `snapshot_period`        | u64        | Minimum number of blocks before the end of voting period which snapshot could be taken to lock the current quorum for a poll |
| `admin_manager`          | String     | Address of the admin manager contract                                                                                        |
| `poll_gas_limit`         | u64        | Maximum amount of gas which a poll can consume during its execution                                                          |

#### PollConfig

| Key                | Type    | Description                                                     |
| ------------------ | ------- | --------------------------------------------------------------- |
| `proposal_deposit` | Uint128 | MIR deposit required for new polls to be submitted              |
| `voting_period`    | u64     | Number of seconds during which votes can be cast                |
| `quorum`           | Decimal | Minimum percentage of participation required for a poll to pass |
| `threshold`        | Decimal | Minimum percentage of `yes` votes required for a poll to pass   |

## ExecuteMsg

### `Receive`

Can be called during a CW20 token transfer when the Gov contract is the recipient. Allows the token transfer to execute a [Receive Hook](gov.md#receive-hooks) as a subsequent action within the same transaction.

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

| Key      | Type      | Description                                                 |
| -------- | --------- | ----------------------------------------------------------- |
| `amount` | Uint128   | Amount of tokens received                                   |
| `sender` | HumanAddr | Sender of token transfer                                    |
| `msg`\*  | Binary    | Base64-encoded JSON of [Receive Hook](gov.md#receive-hooks) |

\* = optional

### `UpdateConfig`

Updates the configuration for the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        quorum: Option<Decimal>,
        threshold: Option<Decimal>,
        voting_period: Option<u64>,
        effective_delay: Option<u64>,
        expiration_period: Option<u64>,
        proposal_deposit: Option<Uint128>,
        voter_weight: Option<Decimal>,
        snapshot_period: Option<u64>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
     "update_config": {
        "owner": "terra...1",
        "mirror_token": "terra1...",
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8,
        "effective_delay": 8,
        "expiration_period": 8,
        "proposal_deposit": "100000",
        "voter_weight": "0.5",
        "snapshot_period": 8
     }
}  
```
{% endtab %}
{% endtabs %}

| Key                   | Type      | Description                                                                                                              |
| --------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------ |
| `owner`\*             | HumanAddr | Address of owner of governance contract                                                                                  |
| `quorum`\*            | Decimal   | Minimum percentage of participation required for a poll to pass                                                          |
| `threshold`\*         | Decimal   | Minimum percentage of `yes` votes required for a poll to pass                                                            |
| `voting_period`\*     | u64       | Number of blocks during which votes can be cast                                                                          |
| `effective_delay`\*   | u64       | Number of blocks after a poll passes to apply changes                                                                    |
| `expiration_period`\* | u64       | Number of blocks after a poll's voting period during which the poll can be executed                                      |
| `proposal_deposit`\*  | Uint128   | Minimum MIR deposit required for a new poll to be submitted                                                              |
| `voter_weight`\*      | Decimal   | Ratio of protocol fee which will be distributed among the governance poll voters                                         |
| `snapshot_period`\*   | u64       | Minimum number of blocks before end of voting period which snapshot could be taken to lock the current quorum for a poll |

\* = optional

### `CastVote`

Submits a user's vote for an active poll. Once a user has voted, they cannot change their vote with subsequent messages (increasing voting power, changing vote option, cancelling vote, etc.)

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    CastVote {
        poll_id: u64,
        vote: VoteOption,
        amount: Uint128,
    },
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "cast_vote": {
        "amount": "1000000",
        "poll_id": 8,
        "vote": "yes/no/abstain"
    }
}
```
{% endtab %}
{% endtabs %}

| Key       | Type       | Description                                     |
| --------- | ---------- | ----------------------------------------------- |
| `amount`  | Uint128    | Amount of voting power (staked MIR) to allocate |
| `poll_id` | u64        | Poll ID                                         |
| `vote`    | VoteOption | Can be `yes`,`no` or `abstain`                  |

### `WithdrawVotingTokens`

Removes deposited MIR tokens from a staking position and returns them to a user's balance.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    WithdrawVotingTokens {
        amount: Option<Uint128>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw_voting_tokens": {
    "amount": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key        | Type    | Description                                                                      |
| ---------- | ------- | -------------------------------------------------------------------------------- |
| `amount`\* | Uint128 | Amount of MIR tokens to withdraw. If empty, all staked MIR tokens are withdrawn. |

\* = optional

### `WithdrawVotingRewards`

Withdraws a user’s voting reward for user’s voted governance poll after `end_poll` has happened.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    WithdrawVotingRewards {},
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "withdraw_voting_rewards": {}
}
```
{% endtab %}
{% endtabs %}

### `StakeVotingRewards`

Immediately re-stakes user's voting rewards to Gov Contract.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    StakeVotingRewards {},
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "stake_voting_rewards": {}
}
```
{% endtab %}
{% endtabs %}

### `EndPoll`

Can be issued by anyone to end the voting for an active poll. Triggers tally the results to determine whether the poll has passed. The current block height must exceed the end height of voting phase.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    EndPoll {
        poll_id: u64,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "end_poll": {
    "poll_id": 8
  }
}
```
{% endtab %}
{% endtabs %}

| Key       | Type | Description |
| --------- | ---- | ----------- |
| `poll_id` | u64  | Poll ID     |

### `ExecutePoll`

Can be issued by anyone to implement into action the contents of a passed poll. The current block height must exceed the end height of the poll's effective delay.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ExecutePoll {
        poll_id: u64,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "execute_poll": {
    "poll_id": 8
  }
}
```
{% endtab %}
{% endtabs %}

| Key       | Type | Description |
| --------- | ---- | ----------- |
| `poll_id` | u64  | Poll ID     |

### `SnapshotPoll`

Snapshot of poll’s current `quorum` status is saved when the block height enters `snapshot_period`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    SnapshotPoll {
        poll_id: u64,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "snapshot_poll": {
        "poll_id": 8
    }
}
```
{% endtab %}
{% endtabs %}

| Key       | Type | Description |
| --------- | ---- | ----------- |
| `poll_id` | u64  | Poll ID     |

## Receive Hooks

### `StakeVotingTokens`

{% hint style="danger" %}
**WARNING**

If you send MIR tokens to the Gov contract without issuing this hook, they will not be staked and will be irrevocably donated to the reward pool for stakers.
{% endhint %}

Issued when sending MIR tokens to the Gov contract to add them to their MIR staking position.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    StakeVotingTokens {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "stake_voting_tokens": {}
}
```
{% endtab %}
{% endtabs %}

### `CreatePoll`

Issued when sending MIR tokens to the Gov contract to create a new poll. Will only succeed if the amount of tokens sent meets the configured`proposal_deposit`amount. Contains a generic message to be issued by the Gov contract if it passes (can invoke messages in other contracts it owns).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
#[allow(clippy::large_enum_variant)]
pub enum Cw20HookMsg {
   CreatePoll {
        title: String,
        description: String,
        link: Option<String>,
        execute_msg: Option<PollExecuteMsg>,
        admin_action: Option<PollAdminAction>,
    },

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct PollExecuteMsg {
    pub contract: String,
    pub msg: Binary,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "create_poll": {
    "description": "...",
    "execute_msg": {
      "contract": "terra1...",
      "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
    },
    "link": "...",
    "title": "...",
    "admin_action": {
      "contract": "terra1...",
      "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
    }   
  }
}
```
{% endtab %}
{% endtabs %}

| Key              | Type            | Description                                               |
| ---------------- | --------------- | --------------------------------------------------------- |
| `description`    | string          | Poll description                                          |
| `execute_msg`\*  | ExecuteMsg      | Message to be executed by Gov contract                    |
| `link`\*         | string          | URL to external post about poll (forum, PDF, etc.)        |
| `title`          | string          | Poll title                                                |
| `admin_action`\* | PollAdminAction | Messages to be executed for migration and authorize polls |

\* = optional

### `DepositReward`

Reward is distributed between MIR stakers and governance poll voters based on `voter_weight` when rewards are sent from [Mirror Collector](collector.md).&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    DepositReward {},
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "deposit_reward": {}
}
```
{% endtab %}
{% endtabs %}

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
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: String,
    pub mirror_token: String,
    pub effective_delay: u64,
    pub default_poll_config: PollConfig,
    pub migration_poll_config: PollConfig,
    pub auth_admin_poll_config: PollConfig,
    pub voter_weight: Decimal,
    pub snapshot_period: u64,
    pub admin_manager: String,
    pub poll_gas_limit: u64,
}
```

| Key                      | Type       | Description                                                                                                              |
| ------------------------ | ---------- | ------------------------------------------------------------------------------------------------------------------------ |
| `mirror_token`           | HumanAddr  | Contract address of Mirror Token (MIR)                                                                                   |
| `default_poll_config`    | PollConfig | `PollConfig` for default polls                                                                                           |
| `migration_poll_config`  | PollConfig | `PollConfig` for migration polls                                                                                         |
| `auth_admin_poll_config` | PollConfig | `PollConfig` for Authorize polls                                                                                         |
| `effective_delay`        | u64        | Number of blocks after a poll passes to apply changes                                                                    |
| `proposal_deposit`       | Uint128    | Minimum MIR deposit required for a new poll to be submitted                                                              |
| `voter_weight`           | Decimal    | Ratio of protocol fee which will be distributed among the governance poll voters                                         |
| `snapshot_period`        | u64        | Minimum number of blocks before end of voting period which snapshot could be taken to lock the current quorum for a poll |
| `admin_manager`          | String     | Address of admin manager contract                                                                                        |
| `poll_gas_limit`         | u64        | Maximum amount of gas which a poll can consume during its execution                                                      |
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
    "mirror_token": "terra1...",
    "effective_delay": 8
    "default_poll_config": {
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8
        "proposal_deposit": "100000000"
        },
    "migration_poll_config": {
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8
        "proposal_deposit": "100000000"
        },
    "auth_admin_poll_config": {
        "quorum": "0.1",
        "threshold": "0.5",
        "voting_period": 8
        "proposal_deposit": "100000000"
        },
    "voter_weight": "0.5",
    "snapshot_period": 8,
    "admin_manager": "terra1...",
    "poll_gas_limit": 8
} 
}
```

| Key                      | Type       | Description                                                                                                              |
| ------------------------ | ---------- | ------------------------------------------------------------------------------------------------------------------------ |
| `mirror_token`           | HumanAddr  | Contract address of Mirror Token (MIR)                                                                                   |
| `default_poll_config`    | PollConfig | `PollConfig` of default polls                                                                                            |
| `migration_poll_config`  | PollConfig | `PollConfig` of migration polls                                                                                          |
| `auth_admin_poll_config` | PollConfig | `PollConfig` of Authorize polls                                                                                          |
| `quorum`                 | Decimal    | Minimum percentage of participation required for a poll to pass                                                          |
| `threshold`              | Decimal    | Minimum percentage of `yes` votes required for a poll to pass                                                            |
| `voting_period`          | u64        | Number of blocks during which votes can be cast                                                                          |
| `effective_delay`        | u64        | Number of blocks after a poll passes to apply changes                                                                    |
| `proposal_deposit`       | Uint128    | Minimum MIR deposit required for a new poll to be submitted                                                              |
| `voter_weight`           | Decimal    | Ratio of protocol fee which will be distributed among the governance poll voters                                         |
| `snapshot_period`        | u64        | Minimum number of blocks before end of voting period which snapshot could be taken to lock the current quorum for a poll |
| `admin_manager`          | String     | Address of admin manager contract                                                                                        |
| `poll_gas_limit`         | u64        | Maximum amount of gas which a poll can consume during its execution                                                      |
{% endtab %}
{% endtabs %}

### `State`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    State {}
}
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct StateResponse {
    pub poll_count: u64,
    pub total_share: Uint128,
    pub total_deposit: Uint128,
    pub pending_voting_rewards: Uint128,
}
```

| Key                      | Type    | Description                                                     |
| ------------------------ | ------- | --------------------------------------------------------------- |
| `poll_count`             | u64     | Total number of polls that have been created on Mirror Protocol |
| `total_share`            | Uint128 | Amount of staked MIR to governance contract                     |
| `total_deposit`          | Uint128 | Amount of locked MIR to governance polls                        |
| `pending_voting_rewards` | Uint128 | Amount of voting rewards that are not claimed yet               |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "state": {}
}
```

#### Response

```javascript
{
    "state_response": {
        "poll_count": 100,
        "total_share": "1000000",
        "total_deposit": "1000000",
        "pending_voting_rewards": "1000000"
    }
}
```

| Key                      | Type    | Description                                                     |
| ------------------------ | ------- | --------------------------------------------------------------- |
| `poll_count`             | u64     | Total number of polls that have been created on Mirror Protocol |
| `total_share`            | Uint128 | Amount of staked MIR to governance contract                     |
| `total_deposit`          | Uint128 | Amount of locked MIR to governance polls                        |
| `pending_voting_rewards` | Uint128 | Amount of voting rewards that are not claimed yet               |
{% endtab %}
{% endtabs %}

### `Staker`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Staker {
        address: HumanAddr,
    }
}
```

| Key       | Type      | Description       |
| --------- | --------- | ----------------- |
| `address` | HumanAddr | Address of staker |

#### Response

```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct StakerResponse {
    pub balance: Uint128,
    pub share: Uint128,
    pub locked_balance: Vec<(u64, VoterInfo)>,
    pub pending_voting_rewards: Uint128,
}
```

| Key                      | Type                  | Description                                                      |
| ------------------------ | --------------------- | ---------------------------------------------------------------- |
| `balance`                | Uint128               | Amount of MIR staked by the user                                 |
| `share`                  | Uint128               | Weight of the user's staked MIR                                  |
| `locked_balance`         | Vec<(u64, VoterInfo)> | Total number of staked MIR used as votes, and chosen vote option |
| `pending_voting_rewards` | u64                   | Amount of voting rewards that are not claimed yet                |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "staker": {
    "address": "terra1..."
  }
}
```

| Key       | Type      | Description       |
| --------- | --------- | ----------------- |
| `address` | HumanAddr | Address of staker |

#### Response

```javascript
{
    "staker_response": {
        "balance": "1000000",
        "share": "1000000",
        "locked_balance": [
            [100, yes, "1000000"],
            [100, abstain, "1000000"]
        ]
        "pending_voting_rewards": "1000000"
    }
}
```

| Key                      | Type                  | Description                                                      |
| ------------------------ | --------------------- | ---------------------------------------------------------------- |
| `balance`                | Uint128               | Amount of MIR staked by the user                                 |
| `share`                  | Uint128               | Weight of the user's staked MIR                                  |
| `locked_balance`         | Vec<(u64, VoterInfo)> | Total number of staked MIR used as votes, and chosen vote option |
| `pending_voting_rewards` | u64                   | Amount of voting rewards that are not claimed yet                |
{% endtab %}
{% endtabs %}

### `Poll`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Poll {
        poll_id: u64,
    }
}
```

| Key       | Type | Description |
| --------- | ---- | ----------- |
| `poll_id` | u64  | Poll ID     |

#### Response

```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct PollResponse {
    pub id: u64,
    pub creator: HumanAddr,
    pub status: PollStatus,
    pub end_height: u64,
    pub title: String,
    pub description: String,
    pub link: Option<String>,
    pub deposit_amount: Uint128,
    pub execute_data: Option<ExecuteMsg>,
    pub yes_votes: Uint128, // balance
    pub no_votes: Uint128,  // balance
    pub abstain_votes: Uint128, // balance
    pub total_balance_at_end_poll: Option<Uint128>,
    pub voters_reward: Uint128,
    pub staked_amount: Option<Uint128>,
}
```

| Key                             | Type       | Description                                                                                |
| ------------------------------- | ---------- | ------------------------------------------------------------------------------------------ |
| id                              | u64        | Poll ID                                                                                    |
| creator                         | HumanAddr  | Address of the poll creator                                                                |
| status                          | PollStatus | Could be one of "in progress", "rejected", "passed", "executed", "expired"                 |
| end\_height                     | u64        | Amount of voting rewards that are not claimed yet                                          |
| title                           | String     | Title of the poll                                                                          |
| description                     | String     | Description submitted by the creator                                                       |
| link\*                          | Uint128    | URL link                                                                                   |
| deposit\_amount                 | binary     | Initial MIR deposit at poll creation                                                       |
| execute\_data\*                 | ExecuteMsg | Message to be executed by Gov contract                                                     |
| yes\_votes                      | Uint128    | Amount of yes votes                                                                        |
| no\_votes                       | Uint128    | Amount of no votes                                                                         |
| abstain\_votes                  | Uint128    | Amount of abstain votes                                                                    |
| total\_balance\_at\_end\_poll\* | Uint128    | Total balanced used as yes, no, or abstain votes at the end of the poll                    |
| voters\_reward                  | Uint128    | Amount of MIR reward accumulated to be distributed to the voters                           |
| staked\_amount\*                | Uint128    | Total number of MIR staked on governance contract (used when Snapshot Poll has been taken) |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "poll": {
    "poll_id": 8
  }
}
```

| Key       | Type | Description |
| --------- | ---- | ----------- |
| `poll_id` | u64  | Poll ID     |

#### Response

```javascript
{
    "poll_response": {
        "id": 8,
        "creator": "terra1...",
        "status": "in_progress",
        "end_height": 1000000,
        "title": "Register mCOIN Parameters",
        "description": "mCOIN has been...",
        "link": "https://mirror.finance",
        "deposit_amount": "1000000",
        "execute_data": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9",
        "yes_votes": "1000000",
        "no_votes": "1000000",
        "abstain_votes": "1000000",
        "total_balance_at_end_poll": "1000000",
        "voters_reward": "1000000",
        "staked_amount": "1000000"
    }
}
```

| Key                             | Type       | Description                                                                                |
| ------------------------------- | ---------- | ------------------------------------------------------------------------------------------ |
| id                              | u64        | Poll ID                                                                                    |
| creator                         | HumanAddr  | Address of the poll creator                                                                |
| status                          | PollStatus | Could be one of "in progress", "rejected", "passed", "executed", "expired"                 |
| end\_height                     | u64        | Amount of voting rewards that are not claimed yet                                          |
| title                           | String     | Title of the poll                                                                          |
| description                     | String     | Description submitted by the creator                                                       |
| link\*                          | Uint128    | URL link                                                                                   |
| deposit\_amount                 | binary     | Initial MIR deposit at poll creation                                                       |
| execute\_data\*                 | ExecuteMsg | Message to be executed by Gov contract                                                     |
| yes\_votes                      | Uint128    | Amount of yes votes                                                                        |
| no\_votes                       | Uint128    | Amount of no votes                                                                         |
| abstain\_votes                  | Uint128    | Amount of abstain votes                                                                    |
| total\_balance\_at\_end\_poll\* | Uint128    | Total balanced used as yes, no, or abstain votes at the end of the poll                    |
| voters\_reward                  | Uint128    | Amount of MIR reward accumulated to be distributed to the voters                           |
| staked\_amount\*                | Uint128    | Total number of MIR staked on governance contract (used when Snapshot Poll has been taken) |
{% endtab %}
{% endtabs %}

### `Polls`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Polls {
        filter: Option<PollStatus>,
        limit: Option<u32>,
        start_after: Option<u64>,
    }
}
```

| Key             | Type       | Description                        |
| --------------- | ---------- | ---------------------------------- |
| `filter`\*      | PollStatus | Can be `yes` or `no`               |
| `limit`\*       | u32        | Limit of results to fetch          |
| `start_after`\* | u64        | Begins search query at specific ID |

\* = optional

#### Response

```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct PollsResponse {
    pub polls: Vec<PollResponse>,
}
```

| Key     | Type               | Description                   |
| ------- | ------------------ | ----------------------------- |
| `polls` | Vec\<PollResponse> | Array of poll query responses |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "polls": {
    "filter": "in_progress",
    "limit": 8,
    "start_after": 8
  }
}
```

| Key             | Type       | Description                        |
| --------------- | ---------- | ---------------------------------- |
| `filter`\*      | PollStatus | Can be `yes` or `no`               |
| `limit`\*       | u32        | Limit of results to fetch          |
| `start_after`\* | u64        | Begins search query at specific ID |

\* = optional

#### Response

```javascript
{
    "polls_response": {
        "polls": [
            "poll_response": {
                "id": 8,
                "creator": "terra1...",
                "status": "in_progress",
                "end_height": 1000000,
                "title": "Register mCOIN Parameters",
                "description": "mCOIN has been...",
                "link": "https://mirror.finance",
                "deposit_amount": "1000000",
                "execute_data": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9",
                "yes_votes": "1000000",
                "no_votes": "1000000",
                "abstain_votes": "1000000",
                "total_balance_at_end_poll": "1000000",
                "voters_reward": "1000000",
                "staked_amount": "1000000"
            }
            ...
        ]
    }
}
```

| Key     | Type               | Description                   |
| ------- | ------------------ | ----------------------------- |
| `polls` | Vec\<PollResponse> | Array of poll query responses |
{% endtab %}
{% endtabs %}



### `Voters`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Voters {
        limit: Option<u32>,
        poll_id: u64,
        start_after: Option<HumanAddr>,
    }
}
```

| Key             | Type      | Description                     |
| --------------- | --------- | ------------------------------- |
| `limit`\*       | u32       | Limit of results to fetch       |
| `poll_id`       | u64       | Poll ID                         |
| `start_after`\* | HumanAddr | Begins search query with prefix |

\* = optional

#### Response

```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct VotersResponseItem {
    pub voter: HumanAddr,
    pub vote: VoteOption,
    pub balance: Uint128,
}
```

| Key       | Type       | Description                            |
| --------- | ---------- | -------------------------------------- |
| `voter`   | HumanAddr  | Address of the voter                   |
| `vote`    | VoteOption | Could be one of `yes`, `no`, `abstain` |
| `balance` | Uint128    | Amount of staked MIR used for voting   |
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "voters": {
    "limit": 8,
    "poll_id": 8,
    "start_after": "terra1..."
  }
}
```

| Key             | Type      | Description                     |
| --------------- | --------- | ------------------------------- |
| `limit`\*       | u32       | Limit of results to fetch       |
| `poll_id`       | u64       | Poll ID                         |
| `start_after`\* | HumanAddr | Begins search query with prefix |

\* = optional

#### Response

```rust
{
    "voter_response": {
        "voter": "terra1...",
        "vote": "yes",
        "balance": "1000000"
    }
}
```

| Key       | Type       | Description                            |
| --------- | ---------- | -------------------------------------- |
| `voter`   | HumanAddr  | Address of the voter                   |
| `vote`    | VoteOption | Could be one of `yes`, `no`, `abstain` |
| `balance` | Uint128    | Amount of staked MIR used for voting   |
{% endtab %}
{% endtabs %}
