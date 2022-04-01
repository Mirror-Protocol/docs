# Contract Migration Procedure

### Migrations and Updates

The Mirror Protocol allows developers to propose a smart contract migration through Mirror governance consensus. Any developer who can build Mirror’s smart contracts can submit these polls to allow feature upgrades on Mirror Protocol’s core mechanism.

The Mirror Admin Manager contract holds the admin keys which can only be used when this specific governance poll passes, and initiates admin actions detailed by the poll.

{% hint style="warning" %}
There is no user interface on the web application to create migration polls since these are are highly sensitive polls that can cause unwanted results for Mirror Protocol.

Mirror users can still view and vote on polls created by developers that are able to interact with Mirror governance contract without a user interface.
{% endhint %}

#### Migration Poll

This poll allows existing Mirror contracts to be migrated to newly built versions. The developer wanting to create this poll must include the following message within the `admin_action` parameter of the `CreatePoll` transaction:

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
	ExecuteMigrations {
        migrations: Vec<(String, u64, Binary)>,
    }
```

where `string` is the contract address to be migrated, `u64` is the code ID, and `Binary` is the migration execution message.

#### Admin Key Transfer

This poll is used for special occasions where the admin key for migration is required by an entity that is preparing an update to prepare for Terra blockchain or CosmWasm contract updates.

To create this poll, the following message must be included within the `admin_action` parameter of `CreatePoll` transaction:

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
	AuthorizeClaim { authorized_addr: String },
```

where `authorized_addr` is the address to transfer the admin keys for `admin_claim_period`.
