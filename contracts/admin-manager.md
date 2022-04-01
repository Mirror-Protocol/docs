# Admin Manager

Admin Manager contract is owned by Gov contract, and is responsible for executing contract migrations and admin key transfers, which are used in order to execute time sensitive migrations such as Terra's `columbus` network upgrades.&#x20;

All operations in admin manager contract can only be executed by submitting and executing a poll on Mirror governance (one of `migration_poll` or `auth_admin_poll`).

### Config

| Key                  | Type   | Description                                                                                   |
| -------------------- | ------ | --------------------------------------------------------------------------------------------- |
| `owner`              | String | Address of owner of admin manager (Gov contract)                                             |
| `admin_claim_period` | u64    | The duration of admin privilege delegation to a defined address when `AuthorizeClaim` occurs. |

### InstantiateMsg

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InstantiateMsg {
    pub owner: String,
    pub admin_claim_period: u64,
}
```

| Key                  | Type   | Description                                                                                  |
| -------------------- | ------ | -------------------------------------------------------------------------------------------- |
| `owner`              | String | Address of owner of admin manager (Gov contract)                                             |
| `admin_claim_period` | u64    | The duration of admin privilege delegation to a defined address when `AuthorizeClaim` occurs |

## ExecuteMsg

### UpdateOwner

Replaces the owner address of the admin manager contract to a new one

{% tabs %}
{% tab title="Rust" %}


```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    UpdateOwner {
        owner: String,
    }
```

| Key     | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| `owner` | String | Address of the owner of admin manager |
{% endtab %}

{% tab title="JSON" %}
```json
{
"update_owner": {
    "owner": "terra1..."
    }
}
```

| Key     | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| `owner` | String | Address of the owner of admin manager |
{% endtab %}
{% endtabs %}



### ExecuteMigrations

Contract migration is executed when the Gov contract sends an `ExecuteMigrations` message.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
	ExecuteMigrations {
        migrations: Vec<(String, u64, Binary)>,
    },
```

#### `migrations`

| Type   | Description                                           |
| ------ | ----------------------------------------------------- |
| String | Address of the current Mirror contract to be migrated |
| u64    | Token code ID of the new contracts                    |
| Binary | Migration execution message                           |
{% endtab %}

{% tab title="JSON" %}
```json
{
"execute_migrations": {
    "migrations": [
        ("terra1...", 123, eBdwav...),
        ("terra1...", 123, eBdwav...)
        ]
    }
}
```

#### `migrations`

| Type   | Description                                           |
| ------ | ----------------------------------------------------- |
| String | Address of the current Mirror contract to be migrated |
| u64    | Token code ID of the new contracts                    |
| Binary | Migration execution message                           |
{% endtab %}
{% endtabs %}

### AuthorizeClaim

Delegates admin privileges to migrate contracts to a specified address until `admin_claim_period` ends.&#x20;

{% tabs %}
{% tab title="Rust" %}


```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
        AuthorizeClaim {
        authorized_addr: String,
    },
```

| Key               | Type   | Description                                            |
| ----------------- | ------ | ------------------------------------------------------ |
| `authorized_addr` | String | Address to temporarily delegate the admin privilege to |
{% endtab %}

{% tab title="JSON" %}
```json
{
"authorize_claim": {
    "authrized_addr": "terra1..."
    }
}
```

| Key               | Type   | Description                                            |
| ----------------- | ------ | ------------------------------------------------------ |
| `authorized_addr` | String | Address to temporarily delegate the admin privilege to |
{% endtab %}
{% endtabs %}

### ClaimAdmin

Once `AuthorizeClaim` is executed, the `authorized_addr` can send this transaction to claim the rights to the admin keys until the `admin_claim_period` ends.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
	ClaimAdmin {
        contract: String,
    },
```

| Key        | Type   | Description                                                          |
| ---------- | ------ | -------------------------------------------------------------------- |
| `contract` | String | Address of contracts that the `authorized_addr` has right to migrate |


{% endtab %}

{% tab title="JSON" %}
```json
{
"claim_admin": {
    "contract": "terra1..."
    }
}
```

| Key        | Type   | Description                                                          |
| ---------- | ------ | -------------------------------------------------------------------- |
| `contract` | String | Address of contracts that the `authorized_addr` has right to migrate |
{% endtab %}
{% endtabs %}

## QueryMsg

### Config

Returns the configuration of the admin manager contract

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {},
```

#### ConfigResponse

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: String,
    pub admin_claim_period: u64,
}
```

| Key                  | Type   | Description                                                             |
| -------------------- | ------ | ----------------------------------------------------------------------- |
| `owner`              | String | Address of the owner of admin manager contract (Gov contract)           |
| `admin_claim_period` | String | Length of time which the admin key is claimed and used for (in seconds) |
{% endtab %}

{% tab title="JSON" %}
```json
{
"config": {}
}
```

#### ConfigResponse

```json
{
"config_response": {
    "owner": "terra1...",
    "admin_claim_period": 8
    }
}
```

| Key                  | Type   | Description                                                             |
| -------------------- | ------ | ----------------------------------------------------------------------- |
| `owner`              | String | Address of the owner of admin manager contract (Gov contract)           |
| `admin_claim_period` | u64    | Length of time which the admin key is claimed and used for (in seconds) |
{% endtab %}
{% endtabs %}

### MigrationRecords

Returns the history of `execute_migrations` records.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
	MigrationRecords {
        start_after: Option<u64>, // timestamp (seconds)
        limit: Option<u32>,
    },
```

| Key           | Type | Description                                             |
| ------------- | ---- | ------------------------------------------------------- |
| `start_after` | u64  | Optional timestamp to return the migration history from |
| `limit`       | u32  | Max number of migration records to return               |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct MigrationRecordResponse {
    pub executor: String,
    pub time: u64,
    pub migrations: Vec<MigrationItem>,
}
```

| Key          | Type          | Description                                                                                                                   |
| ------------ | ------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `executor`   | String        | Address of the migration executor                                                                                             |
| `time`       | u64           | UNIX timestamp of when the migration was executed                                                                             |
| `migrations` | MigrationItem | Migration specific details including migrated `contract` address, token code ID and `binary` message used for the migration.  |
{% endtab %}

{% tab title="JSON" %}
```json
{
"migration_records": {
    "start_after": 12341234,
    "limit": 8
    }
}
```

| Key           | Type | Description                                             |
| ------------- | ---- | ------------------------------------------------------- |
| `start_after` | u64  | Optional timestamp to return the migration history from |
| `limit`       | u32  | Max number of migration records to return               |

#### Response

```json
{
    "executor": "terra1...",
    "time": 12341234,
    "migrations": {
        "contract": "terra1...",
        "new_code_id": 234,
        "msg": "evDw12549..."
        }
    }
```

| Key          | Type                | Description                                                                                                                 |
| ------------ | ------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `executor`   | String              | Address of the migration executor                                                                                           |
| `time`       | u64                 | UNIX timestamp of when the migration was executed                                                                           |
| `migrations` | Vec\<MigrationItem> | Migration specific details including migrated `contract` address, token code ID and `binary` message used for the migration |
{% endtab %}
{% endtabs %}

### AuthRecords

Returns the history of `AuthorizeClaim` transactions

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
	AuthRecords {
        start_after: Option<u64>, // timestamp (seconds)
        limit: Option<u32>,
    },
```

| Key           | Type | Description                                           |
| ------------- | ---- | ----------------------------------------------------- |
| `start_after` | u64  | Optional timestamp of when to return the history from |
| `limit`       | u32  | Max number of records to return                       |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct AuthRecordResponse {
    pub address: String,
    pub start_time: u64,
    pub end_time: u64,
}
```

| Key          | Type   | Description                                             |
| ------------ | ------ | ------------------------------------------------------- |
| `address`    | String | The address at which the admin key was authorized to    |
| `start_time` | u64    | UNIX timestamp of when the `admin_claim_period` started |
| `end_time`   | u64    | UNIX timestamp of when the `admin_claim_period` ended   |
{% endtab %}

{% tab title="JSON" %}
```json
{
"auth_records": {
    "start_after": 12341234,
    "limit": 8
    }
}
```

| Key           | Type | Description                                           |
| ------------- | ---- | ----------------------------------------------------- |
| `start_after` | u64  | Optional timestamp of when to return the history from |
| `limit`       | u32  | Max number of records to return                       |

#### Response



```json
{
"address": "terra1...",
"start_time": 12341234,
"end_time": 12341234
}
```

| Key          | Type   | Description                                             |
| ------------ | ------ | ------------------------------------------------------- |
| `address`    | String | The address which the admin key was authorized to       |
| `start_time` | u64    | UNIX timestamp of when the `admin_claim_period` started |
| `end_time`   | u64    | UNIX timestamp of when the `admin_claim_period` ended   |
{% endtab %}
{% endtabs %}
