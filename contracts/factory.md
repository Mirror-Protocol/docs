# Factory

The Factory contract is Mirror Protocol's central directory and organizes information related to mAssets and the Mirror Token \(MIR\). It is also responsible for minting new MIR tokens each block and distributing them to the [Staking Contract](staking.md) for rewarding LP Token stakers.

After the initial bootstrapping of Mirror Protocol contracts, the Factory is assigned to be the owner for the Mint, Oracle, Staking, and Collector contracts. The Factory is owned by the [Gov Contract.](gov.md)

## Config

| Name | Type | Description |
| :--- | :--- | :--- |
| `mirror_token` | HumanAddr | Contract address of Mirror Token \(MIR\) |
| `mint_contract` | HumanAddr | Contract address of [Mirror Mint](mint.md) |
| `oracle_contract` | HumanAddr | Contract address of [Mirror Oracle](oracle.md) |
| `terraswap_factory` | HumanAddr | Contract address of Terraswap Factory |
| `staking_contract` | HumanAddr | Contract address of [Mirror Staking](staking.md) |
| `commission_collector` | HumanAddr | Contract address of [Mirror Collector](collector.md) |
| `mint_per_block` | Uint128 | Amount of new MIR tokens to mint per block |
| `token_code_id` | u64 | Code ID for CW20 contract for generating new mAssets |
| `base_denom` | String | Native token denom for Terraswap pairs \(TerraUSD\) |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub token_code_id: u64,
    pub base_denom: String,
    pub distribution_schedule: Vec<(u64, u64, Uint128)>, // [[start_time, end_time, distribution_amount], [], ...]
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "token_code_id": 8,
  "base_denom": "uusd",
  "distribution_schedule": [
    [3600, 7200, "1000000"],
    [7200, 10800, "1000000"]
  ]
}
```
{% endtab %}
{% endtabs %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>token_code_id</code>
      </td>
      <td style="text-align:left">u64</td>
      <td style="text-align:left">Code ID for CW20 contract for generating new mAssets</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>base_denom</code>
      </td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Native token denom for Terraswap pairs (TerraUSD)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>distribution_schedule</code>
      </td>
      <td style="text-align:left">Vec</td>
      <td style="text-align:left">
        <p>Distribution schedule for the minting of new MIR tokens. Each entry consists
          of:</p>
        <ul>
          <li>start time (seconds)</li>
          <li>end time (seconds)</li>
          <li>distribution amount for the interval</li>
        </ul>
        <p>Determines the total amount of new MIR tokens minted as rewards for LP
          stakers over the interval [start time, end time].</p>
      </td>
    </tr>
  </tbody>
</table>

## HandleMsg

### `PostInitialize`

Issued by the Factory contract's owner after bootstrapping to initialize the contract's configuration.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    PostInitialize {
        commission_collector: HumanAddr,
        mint_contract: HumanAddr,
        mirror_token: HumanAddr,
        oracle_contract: HumanAddr,
        owner: HumanAddr,
        staking_contract: HumanAddr,
        terraswap_factory: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "post_initialize": {
    "commission_collector": "terra1...",
    "mint_contract": "terra1...",
    "mirror_token": "terra1...",
    "oracle_contract": "terra1...",
    "owner": "terra1...",
    "staking_contract": "terra1...",
    "terraswap_factory": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `commission_collector` | HumanAddr | Contract address of [Mirror Collector](collector.md) |
| `mint_contract` | HumanAddr | Contract address of [Mirror Mint](mint.md) |
| `mirror_token` | HumanAddr | Contract address of Mirror Token \(MIR\) |
| `oracle_contract` | HumanAddr | Contract address of [Mirror Oracle](oracle.md) |
| `**owner` | HumanAddr | Address of the owner of [Mirror Factory](factory.md) |
| `staking_contract` | HumanAddr | Contract address of [Mirror Staking](staking.md) |
| `terraswap_factory` | HumanAddr | Contract address of Terraswap Factory |

### `UpdateConfig`

Updates the configuration for the contract. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>,
        token_code_id: Option<u64>,
        distribution_schedule: Option<Vec<(u64, u64, Uint128)>>
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...",
    "token_code_id": 8,
    "distribution_schedule": [
      [3600, 7200, "1000000"],
      [7200, 10800, "1000000"]
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | Address of the owner of [Mirror Factory](factory.md) |
| `token_code_id`\* | u64 | Code ID for CW20 contract for generating new mAssets |
| `**distribution_schedule`\* | Vec&lt;\(u64, u64, Uint128\)&gt; | New distribution schedule |

\* = optional

### `Whitelist`

Introduces a new mAsset to the protocol and creates markets on Terraswap. This process will:

* Instantiate the mAsset contract as a new Terraswap CW20 token
* Register the mAsset with [Mirror Oracle](oracle.md) and [Mirror Mint](mint.md)
* Create a new Terraswap Pair for the new mAsset against TerraUSD
* Instantiate the LP Token contract associated with the pool as a new Terraswap CW20 token
* Register the LP token with the [Mirror Staking](staking.md) contract

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Whitelist {
        name: String,
        oracle_feeder: HumanAddr,
        params: Params,
        symbol: String,
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct Params {
    pub auction_discount: Decimal,
    pub min_collateral_ratio: Decimal,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "whitelist": {
    "name": "Mirrored Apple Derivative",
    "oracle_feeder": "terra1...",
    "params": {
      "auction_discount": "0.2",
      "min_collateral_ratio": "1.5",
    },
    "symbol": "mAAPL"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of new asset to be whitelisted |
| `oracle_feeder` | HumanAddr | Address of Oracle Feeder for mAsset |
| `params` | Params | mAsset parameters |
| `symbol` | String | mAsset symbol \(ex: `mAAPL`\) |

#### mAsset Params

| Key | Type | Description |
| :--- | :--- | :--- |
| `auction_discount` | Decimal | Liquidation discount for purchasing CDP's collateral |
| `min_collateral_ratio` | Decimal | Minimum C-ratio for CDPs that mint the mAsset |

### `TokenCreationHook`

`(INTERNAL)`

Called after mAsset token contract is created in the [Whitelist](factory.md#whitelist) process. \*\*Why this is necessary

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    TokenCreationHook {
        oracle_feeder: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "token_creation_hook": {
    "oracle_feeder": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `oracle_feeder` | HumanAddr | Address of Oracle Feeder for mAsset |

### `TerraswapCreationHook`

`(INTERNAL)`

Called after mAsset token contract is created in the [Whitelist](factory.md#whitelist) process.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct enum HandleMsg {
    TerraswapCreationHook {
        asset_token: HumanAddr,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "terraswap_creation_hook": {
    "asset_token": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of mAsset token |

### `PassCommand`

Calls the contract specified with the message to execute. Used for invoking functions on other Mirror Contracts since Mirror Factory is defined to be the owner. To be controlled through governance.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    PassCommand {
        contract_addr: HumanAddr,
        msg: Binary,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "pass_command": {
    "contract_addr": "terra1...",
    "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `contract_addr` | HumanAddr | Contract address of contract to call |
| `msg` | Binary | Base64-encoded JSON of ExecuteMsg |

### `Distribute`

Mints the appropriate amount of new MIR tokens as reward for LP stakers by sends the newly minted tokens to the Mirror Staking contract to be distributed to its stakers. Can be called by anyone at any time to trigger block reward distribution for LP stakers.

The contract keeps track of the last height at which `Distribute` was called for a specific asset, and uses it to calculate the amount of new assets to mint for the blocks occurred in the interval between.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Distribute {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribute": {}
}
```
{% endtab %}
{% endtabs %}

### `MigrateAsset`

Can be issued by the oracle feeder of an mAsset to trigger the [mAsset migration procedure](../protocol/mirrored-assets-massets.md#deprecation-and-migration).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    MigrateAsset {
        end_price: Decimal,
        from_token: HumanAddr,
        name: String,
        symbol: String,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "migrate_asset": {
    "end_price": "123.456789",
    "asset_token": "terra1...",
    "name": "...",
    "symbol": "..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `end_price` | Decimal | Freezes the oracle price of mAsset at this value |
| `from_token` | HumanAddr | Contract address of mAsset to migrate |
| `name` | String | Name of the new asset post-migration |
| `symbol` | String | Symbol for the new asset post-migration |

## QueryMsg

### `Config`

Get the Mirror Factory configuration.

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


### `DistributionInfo`

Get the distribution schedules for MIR token.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    DistributionInfo {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribution_info": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `asset_token` | HumanAddr | Contract address of asset token |

