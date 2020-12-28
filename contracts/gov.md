# Gov

The Gov Contract contains logic for holding polls and Mirror Token \(MIR\) staking, and allows the Mirror Protocol to be governed by its users in a decentralized manner. After the initial bootstrapping of Mirror Protocol contracts, the Gov Contract is assigned to be the owner of itself and [Mirror Factory.](factory.md)

New proposals for change are submitted as polls, and are voted on by MIR stakers through the [voting procedure](../protocol/governance.md). Polls can contain messages that can be executed directly without changing the Mirror Protocol code.

The Gov Contract keeps a balance of MIR tokens, which it uses to reward stakers with funds it receives from trading fees sent by the [Mirror Collector](collector.md) and user deposits from creating new governance polls. This balance is separate from the [Community Pool](../protocol/governance.md#community-pool), which is held by the [Community](community.md) contract \(owned by the Gov contract\).

## Config

| Name | Type | Description |
| :--- | :--- | :--- |
| `mirror_token` | HumanAddr | Contract address of Mirror Token \(MIR\) |
| `quorum` | Decimal | Minimum percentage of participation required for a poll to pass |
| `threshold` | Decimal | Minimum percentage of `yes` votes required for a poll to pass |
| `voting_period` | u64 | Number of blocks during which votes can be cast |
| `proposal_deposit` | Uint128 | Minimum MIR deposit required for a new poll to be submitted |
| `effective_delay` | u64 | Number of blocks after a poll passes to apply changes |
| `expiration_period` | u64 | Number of blocks after a poll's voting period during which the poll can be executed. |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub mirror_token: HumanAddr,
    pub quorum: Decimal,
    pub threshold: Decimal,
    pub voting_period: u64,
    pub proposal_deposit: Uint128,
    pub effective_delay: u64
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "mirror_token": "terra1...",
  "quorum": "0.4",
  "threshold": "0.5",
  "voting_period": 8,
  "proposal_deposit": "1000000"
  "effective_delay": 8
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `mirror_token` | HumanAddr | Contract address of Mirror Token \(MIR\) |
| `quorum` | Decimal | Minimum percentage of participation required for a poll to pass |
| `threshold` | Decimal | Minimum percentage of `yes` votes required for a poll to pass |
| `voting_period` | u64 | Number of blocks during which votes can be cast |
| `proposal_deposit` | Uint128 | Minimum MIR deposit required for a new poll to be submitted |
| `effective_delay` | u64 | Number of blocks after a poll passes to apply changes |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Mint contract is the recipient. Allows the token transfer to execute a [Receive Hook](gov.md#receive-hooks) as a subsequent action within the same transaction.

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
| `amount` | Uint128 | Amount of tokens received |
| `sender` | HumanAddr | Sender of token transfer |
| `msg`\* | Binary | Base64-encoded JSON of [Receive Hook](gov.md#receive-hooks) |

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
        effective_delay: Option<u64>,
        owner: Option<HumanAddr>,
        proposal_deposit: Option<Uint128>,
        quorum: Option<Decimal>,
        threshold: Option<Decimal>,
        voting_period: Option<u64>,
        expiration_period: Option<u64>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "effective_delay": 8,
    "owner": "terra1...",
    "proposal_deposit": "10000000",
    "quorum": "123.456789",
    "threshold": "123.456789",
    "voting_period": 8,
    "expiration_period": 8
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `effective_delay`\* | u64 | Number of blocks after a poll passes to apply changes |
| `owner`\* | HumanAddr | Address of owner of governance contract |
| `proposal_deposit`\* | Uint128 | Minimum deposited MIR tokens for a poll to enter voting |
| `quorum`\* | Decimal | Percentage of participation \(of total staked MIR\) required for a poll to pass |
| `threshold`\* | Decimal | Percentage of `yes` votes needed for a poll to pass |
| `voting_period`\* | u64 | Number of blocks during which votes for a poll can be cast after it has finished its deposit |
| `expiration_period`\* | u64 | Number of blocks after a poll's voting period during which the poll can be executed. |

\* = optional

### `CastVote`

Submits a user's vote for an active poll. Once a user has voted, they cannot change their vote with subsequent messages \(increasing voting power, changing vote option, cancelling vote, etc.\)

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    CastVote {
        amount: Uint128,
        poll_id: u64,
        vote: VoteOption,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "cast_vote": {
    "amount": "10000000",
    "poll_id": 8,
    "vote": "yes/no"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of voting power \(staked MIR\) to allocate |
| `poll_id` | u64 | Poll ID |
| `vote` | VoteOption | Can be `yes` or `no` |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount`\* | Uint128 | Amount of MIR tokens to withdraw. If empty, all staked MIR tokens are withdrawn. |

\* = optional

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

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

Issued when sending MIR tokens to the Gov contract to create a new poll. Will only succeed if the amount of tokens sent meets the configured `proposal_deposit` amount. Contains a generic message to be issued by the Gov contract if it passes \(can invoke messages in other contracts it owns\).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    CreatePoll {
        description: String,
        execute_msg: Option<ExecuteMsg>,
        link: Option<String>,
        title: String,
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct ExecuteMsg {
    pub contract: HumanAddr,
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
    "title": "..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `description` | string | Poll description |
| `execute_msg`\* | ExecuteMsg | Message to be executed by Gov contract |
| `link`\* | string | URL to external post about poll \(forum, PDF, etc.\) |
| `title` | string | Poll title |

\* = optional

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
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "state": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |


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
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "staker": {
    "address": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of staker |

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
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "poll": {
    "poll_id": 8
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

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
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `filter`\* | PollStatus | Can be `yes` or `no` |
| `limit`\* | u32 | Limit of results to fetch |
| `start_after`\* | u64 | Begins search query at specific ID |

\* = optional

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
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `limit`\* | u32 | Limit of results to fetch |
| `poll_id` | u64 | Poll ID |
| `start_after`\* | HumanAddr | Begins search query with prefix |

\* = optional

