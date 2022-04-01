# TeFi Oracle

The TeFi oracle is a set of smart contracts that support price oracles used across multiple DeFi protocols built on top of Terra blockchain, providing an interface for accessing the latest reported prices for the assets provided by its whitelisted oracle services. Price quotes are kept up-to-date by oracle providers that fetch exchange rates for real-world assets from reputable sources.

On the Mirror Protocol, these prices are used for CDP operations (mint, burn, short, deposit, withdraw) while the price feed is active. Prices are considered stale when there is no new valid price for 60 seconds.

### Smart Contracts

| Contract | Function                                                                                 |
| -------- | ---------------------------------------------------------------------------------------- |
| Hub      | A central directory that holds whitelisted oracle provider information and their proxies |
| Proxy    | Storage of price information maintained by the oracle provider                           |

## Hub

The Hub contract is a central directory for all oracle price providers and their proxies. On the Mirror Protocol, the Hub contract is owned by the Mirror Governance contract, and transactions can only be called through Mirror’s governance consensus.

Through the interaction with the Hub contract, the following actions can happen:&#x20;

* Whitelisting a new oracle service provider
* Registering new price sources on an existing proxy
* Removing and changing priorities of already existing prices

## InstantiateMsg

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InstantiateMsg {
    pub owner: String,
    pub base_denom: String,
    pub max_proxies_per_symbol: u8,
}
```

| Key                      | Type   | Description                                                 |
| ------------------------ | ------ | ----------------------------------------------------------- |
| `owner`                  | String | Owner of Hub contract (Mirror Factory)                      |
| `base_denom`             | String | Token in which the price will be displayed                  |
| `max_proxies_per_symbol` | u8     | Number of sources that can be registered to a single mAsset |

## ExecuteMsg

{% hint style="info" %}
All Oracle Hub contract operations can be only called by owner - Mirror Factory, which is owned by Mirror Governance.&#x20;
{% endhint %}

### UpdateOwner

Operation to update owner of the Hub contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
UpdateOwner { 
    owner: String 
    },
```

| Key     | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| `owner` | String | Address of the owner to be changed to |
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
| `owner` | String | Address of the owner to be changed to |
{% endtab %}
{% endtabs %}

### &#x20;UpdateMaxProxies

Operation used to update the maximum number of price sources that can be registered per mAsset

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	UpdateMaxProxies { max_proxies_per_symbol: u8 },
```

| Key                      | Type | Description                                                   |
| ------------------------ | ---- | ------------------------------------------------------------- |
| `max_proxies_per_symbol` | u8   | Max number of price sources that can be registered per mAsset |
{% endtab %}

{% tab title="JSON" %}


```json
{
"update_max_proxies": {
    "max_proxies_per_symbol": 3
    }
}
```

| Key                      | Type | Description                                                   |
| ------------------------ | ---- | ------------------------------------------------------------- |
| `max_proxies_per_symbol` | u8   | Max number of price sources that can be registered per mAsset |
|                          |      |                                                               |
|                          |      |                                                               |
{% endtab %}
{% endtabs %}

### RegisterSource

The operation used to register a new price source for an asset. Source can only be registered once a proxy is whitelisted to the Hub contract through the `WhitelistProxy` operation.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	RegisterSource {
        symbol: String,
        proxy_addr: String,
        priority: Option<u8>,
    }
```

| Key          | Type   | Description                                                         |
| ------------ | ------ | ------------------------------------------------------------------- |
| `symbol`     | String | Symbol of the asset to register price source for                    |
| `proxy_addr` | String | Address of the proxy contract through which the price is updated    |
| `priority`   | u8     | Defines the priority for this price source over other existing ones |
{% endtab %}

{% tab title="JSON" %}
```json
{
"register_source": {
    "symbol": "AAPL",
    "proxy_addr": "terra1...", 
    "priority": 30
    }
}
```

| Key          | Type   | Description                                                         |
| ------------ | ------ | ------------------------------------------------------------------- |
| `symbol`     | String | Symbol of the asset to register price source for                    |
| `proxy_addr` | String | Address of the proxy contract through which the price is updated    |
| `priority`   | u8     | Defines the priority for this price source over other existing ones |
{% endtab %}
{% endtabs %}

### BulkRegisterSource

Registers multiple sources in one transaction.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	BulkRegisterSource {
        sources: Vec<(String, String, Option<u8>)>, // (symbol, proxy_addr, priority)
    },
```

| Key          | Type   | Description                                                                     |
| ------------ | ------ | ------------------------------------------------------------------------------- |
| `symbol`     | String | Symbol of the asset to register price source for                                |
| `proxy_addr` | String | Address of the proxy contract which the price is updated in                     |
| `priority`   | u8     | Defines the priority of this price source over other existing ones for an asset |
{% endtab %}

{% tab title="JSON" %}


```json
{
"bulk_register_source": {
	"sources": [
		("AAPL", "terra1...", 10),
		("GOOGL", "terra1...", 20)
		]
	}
}
```

| Key          | Type   | Description                                                                     |
| ------------ | ------ | ------------------------------------------------------------------------------- |
| `symbol`     | String | Symbol of the asset to register price source for                                |
| `proxy_addr` | String | Address of the proxy contract which the price is updated in                     |
| `priority`   | u8     | Defines the priority of this price source over other existing ones for an asset |
{% endtab %}
{% endtabs %}

### UpdateSourcePriorityList

Updates the priorities for proxies that are already registered

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	UpdateSourcePriorityList {
        symbol: String,
        priority_list: Vec<(String, u8)>,
    }
```

| Key             | Type              | Description                                                |
| --------------- | ----------------- | ---------------------------------------------------------- |
| `symbol`        | String            | Symbol of the asset to change source priority for          |
| `priority_list` | Vec<(String, u8)> | Vector of Source address (String) and priority number (u8) |
{% endtab %}

{% tab title="JSON" %}
```json
{
"update_source_priority_list": {
        "symbol": "AAPL",
        "priority_list": [
                ("terra1...", 10),
                ("terra1...", 20)
                ]
        }
}
```

| Key             | Type              | Description                                                |
| --------------- | ----------------- | ---------------------------------------------------------- |
| `symbol`        | String            | Symbol of the asset to change source priority for          |
| `priority_list` | Vec<(String, u8)> | Vector of Source address (String) and priority number (u8) |
{% endtab %}
{% endtabs %}

### RemoveSource

Removes a price source of a specified asset symbol from a proxy address.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	RemoveSource { 
		symbol: String, 
		proxy_addr: String 
},
```

| Key          | Type   | Description                                                               |
| ------------ | ------ | ------------------------------------------------------------------------- |
| `symbol`     | String | Symbol of the asset to remove price source from                           |
| `proxy_addr` | String | The address of the proxy contract from which the price source is provided |
{% endtab %}

{% tab title="JSON" %}
```json
{
"remove_source": {
	"symbol": "AAPL",
	"proxy_addr": "terra1..."
	}
}
```

| Key          | Type   | Description                                                               |
| ------------ | ------ | ------------------------------------------------------------------------- |
| `symbol`     | String | Symbol of the asset to remove price source from                           |
| `proxy_addr` | String | The address of the proxy contract from which the price source is provided |
{% endtab %}
{% endtabs %}

### WhitelistProxy

Whitelists a new proxy contract as a price source. After the proxy is whitelisted, it can be registered as a source.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	WhitelistProxy {
        proxy_addr: String,
        provider_name: String,
    },
```

| Key             | Type   | Description                                       |
| --------------- | ------ | ------------------------------------------------- |
| `proxy_addr`    | String | Address of the proxy contract to whitelist to Hub |
| `provider_name` | String | Name to give to the newly whitelisted proxy       |
{% endtab %}

{% tab title="JSON" %}


```json
{
"whitelist_proxy": {
        "proxy_addr": "terra1...",
        "provider_name": "Band Protocol Feeder"
        }
}
```

| Key             | Type   | Description                                       |
| --------------- | ------ | ------------------------------------------------- |
| `proxy_addr`    | String | Address of the proxy contract to whitelist to Hub |
| `provider_name` | String | Name to give to the newly whitelisted proxy       |
{% endtab %}
{% endtabs %}

### RemoveProxy

Removes a whitelisted proxy contract entirely from the Hub contract. This is different from RemoveSource which only removes a single price of an asset, instead of removing the entire set of prices from the proxy.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	RemoveProxy { 
		proxy_addr: String
},
```

| Key          | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| `proxy_addr` | String | Address of the proxy to remove from Hub contract |
{% endtab %}

{% tab title="JSON" %}
```json
{
"remove_proxy": {
	"proxy_addr": "terra1..."
	}
}
```

| Key          | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| `proxy_addr` | String | Address of the proxy to remove from Hub contract |
{% endtab %}
{% endtabs %}

### InsertAssetSymbolMap

Updates the map of asset\_token to symbol. Asset mapping storage is overwritten by this operation if it already exists.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
	InsertAssetSymbolMap {
        map: Vec<(String, String)>, // (address, symbol)
    },
```

| Key       | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| `address` | String | Address of the asset token contract     |
| `symbol`  | String | Symbol applied to the specified address |
{% endtab %}

{% tab title="JSON" %}
```json
{
"insert_asset_symbol_map": {
	"map": [
		("terra1...", "AAPL"),
		("terra1...", "GOOGL")
		]
	}
}
```

| Key       | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| `address` | String | Address of the asset token contract     |
| `symbol`  | String | Symbol applied to the specified address |
{% endtab %}
{% endtabs %}

## QueryMsg

### Config

Returns the configuration of the Oracle Hub contract

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    Config {},
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: String,
    pub base_denom: String,
    pub max_proxies_per_symbol: u8,
}
```

| Key                      | Type   | Description                                                  |
| ------------------------ | ------ | ------------------------------------------------------------ |
| `owner`                  | String | Owner of oracle hub contract (Factory)                       |
| `base_denom`             | String | Base price denomination unit (UST)                           |
| `max_proxies_per_symbol` | u8     | Maximum number of proxies that can be registered to a symbol |
{% endtab %}

{% tab title="JSON" %}
```json
{
"config": {}
}
```

#### Response

```json
{
"config_response": {
    "owner": "terra1...",
    "base_denom": "uusd",
    "max_proxies_per_symbol": 2
    }
}
```

| Key                      | Type   | Description                                                  |
| ------------------------ | ------ | ------------------------------------------------------------ |
| `owner`                  | String | Owner of oracle hub contract (Factory)                       |
| `base_denom`             | String | Base price denomination unit (UST)                           |
| `max_proxies_per_symbol` | u8     | Maximum number of proxies that can be registered to a symbol |
{% endtab %}
{% endtabs %}

### ProxyWhitelist

Returns the list of whitelisted proxies / oracle providers.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    ProxyWhitelist {},
```

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ProxyWhitelistResponse {
    pub proxies: Vec<ProxyInfoResponse>,
}

//ProxyInfoResponse
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ProxyInfoResponse {
    pub address: String,
    pub provider_name: String,
}
```

| Key             | Type                    | Description                                      |
| --------------- | ----------------------- | ------------------------------------------------ |
| `proxies`       | Vec\<ProxyInfoResponse> | Vector list of proxies whitelisted in Oracle Hub |
| `address`       | String                  | Address of the whitelisted proxy                 |
| `provider_name` | String                  | Name applied to the given proxy address          |
{% endtab %}

{% tab title="JSON" %}
```json
{
"proxy_whitelist": {}
}
```

#### Response

```json
{
"proxy_whitelist_response": {
    "proxies": [
        ("terra1...", "Band Protocol Feeder"),
        ("terra1...", "Band protocol Feeder")
        ]
    }
}
```

| Key             | Type                    | Description                                      |
| --------------- | ----------------------- | ------------------------------------------------ |
| `proxies`       | Vec\<ProxyInfoResponse> | Vector list of proxies whitelisted in Oracle Hub |
| `address`       | String                  | Address of the whitelisted proxy                 |
| `provider_name` | String                  | Name applied to the given proxy address          |


{% endtab %}
{% endtabs %}

### AllSources

Returns the list of all symbols with all the sources

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
        AllSources {
        start_after: Option<String>, // symbol for pagination
        limit: Option<u32>,
    },
```

| Key           | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| `start_after` | String | Symbol of asset to start from   |
| `limit`       | u32    | Max number of entries to return |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct AllSourcesResponse {
    pub list: Vec<SourcesResponse>,
}

//SourceResponse
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PriceListResponse {
    pub price_list: Vec<(u8, ProxyInfoResponse, PriceQueryResult)>, // (priority, proxy_info, result)
}
```

| Key          | Type                                           | Description                                                                |
| ------------ | ---------------------------------------------- | -------------------------------------------------------------------------- |
| `list`       | Vec\<SourceResponse>                           | Vector list of price list entries                                          |
| `price_list` | Vec<(u8, ProxyInfoResponse, PriceQueryResult)> | Returns a list of price priority (u8), proxy information and price results |
|              |                                                |                                                                            |
{% endtab %}

{% tab title="JSON" %}
```json
{
"all_sources": {
    "start_after": "AAPL",
    "limit": 10
    }
}
```

| Key           | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| `start_after` | String | Symbol of asset to start from   |
| `limit`       | u32    | Max number of entries to return |
{% endtab %}
{% endtabs %}

### Sources

Returns the information all registered proxies for the provided asset\_token.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    Sources { asset_token: String },
```

| Key           | Type   | Description                                        |
| ------------- | ------ | -------------------------------------------------- |
| `asset_token` | String | Address of the asset token to return responses for |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct SourcesResponse {
    pub symbol: String,
    pub proxies: Vec<(u8, ProxyInfoResponse)>,
}
```

| Key          | Type                                           | Description                                                                |
| ------------ | ---------------------------------------------- | -------------------------------------------------------------------------- |
| `list`       | Vec\<SourceResponse>                           | Vector list of price list entries                                          |
| `price_list` | Vec<(u8, ProxyInfoResponse, PriceQueryResult)> | Returns a list of price priority (u8), proxy information and price results |
{% endtab %}

{% tab title="JSON" %}
```json
{
"sources": {
    "asset_token": "terra1..."
    }
}
```

| Key           | Type   | Description                                        |
| ------------- | ------ | -------------------------------------------------- |
| `asset_token` | String | Address of the asset token to return responses for |

#### Response

```json
{
"sources_response": {
    "symbol": "AAPL", 
    "proxies": [
        (10, "terra1...", "Band Protocol Feeder"),
        (10, "terra1...", "Band Protocol Feeder")
        ]
    }
}
```

|              |                                                |                                                                            |
| ------------ | ---------------------------------------------- | -------------------------------------------------------------------------- |
| `list`       | Vec\<SourceResponse>                           | Vector list of price list entries                                          |
| `price_list` | Vec<(u8, ProxyInfoResponse, PriceQueryResult)> | Returns a list of price priority (u8), proxy information and price results |
{% endtab %}
{% endtabs %}

### SourcesBySymbol

Returns the information of all registered proxies for a provided `asset_token`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    SourcesBySymbol { symbol: String },
```

| Key      | Type   | Description                                          |
| -------- | ------ | ---------------------------------------------------- |
| `symbol` | String | Symbol of the asset to return source information for |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct SourcesResponse {
    pub symbol: String,
    pub proxies: Vec<(u8, ProxyInfoResponse)>,
}
```

| Key       | Type                         | Description                                                |
| --------- | ---------------------------- | ---------------------------------------------------------- |
| `symbol`  | String                       | Symbol of the asset to return source information for       |
| `proxies` | Vec<(u8, ProxyInfoResponse)> | Returns proxy priority (u8), and general proxy information |
{% endtab %}

{% tab title="JSON" %}
```json
{
"sources_by_symbol": {
    "symbol": "AAPL"
    }
}
```

| Key      | Type   | Description                                          |
| -------- | ------ | ---------------------------------------------------- |
| `symbol` | String | Symbol of the asset to return source information for |

#### Response

```json
{
"sources_response": {
    "symbol": "AAPL",
    "proxies": [
        (10, "terra1...", "Band Protocol Feeder")
        ]
    }
}
```

| Key       | Type                         | Description                                                |
| --------- | ---------------------------- | ---------------------------------------------------------- |
| `symbol`  | String                       | Symbol of the asset to return source information for       |
| `proxies` | Vec<(u8, ProxyInfoResponse)> | Returns proxy priority (u8), and general proxy information |
{% endtab %}
{% endtabs %}

### Price

Queries the highest priority available price within the timeframe. If timeframe is not provided, the age of the price will be ignored.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    Price {
        asset_token: String,
        timeframe: Option<u64>,
    },
```

| Key           | Type         | Description                                                      |
| ------------- | ------------ | ---------------------------------------------------------------- |
| `asset_token` | String       | Address of the asset to query prices for                         |
| `timeframe`   | Option\<u64> | Optional field to enter timeframe of the asset's price to return |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PriceResponse {
    pub rate: Decimal,
    pub last_updated: u64,
}
```

| Key            | Type    | Description                                  |
| -------------- | ------- | -------------------------------------------- |
| `rate`         | Decimal | Price denominated in `base_denom`            |
| `last_updated` | u64     | Last updated time of the given asset's price |
{% endtab %}

{% tab title="JSON" %}
```json
{
"price": {
    "asset_token": "terra1...",
    "timeframe": 23451234
    }
}
```

| Key           | Type   | Description                                                      |
| ------------- | ------ | ---------------------------------------------------------------- |
| `asset_token` | String | Address of the asset to query prices for                         |
| `timeframe`   | u64    | Optional field to enter timeframe of the asset's price to return |

#### Response

```json
{
"price_response": {
    "rate": 142.123,
    "last_updated": 23451234
    }
}
```

| Key            | Type    | Description                                  |
| -------------- | ------- | -------------------------------------------- |
| `rate`         | Decimal | Price denominated in `base_denom`            |
| `last_updated` | u64     | Last updated time of the given asset's price |
{% endtab %}
{% endtabs %}

### PriceBySymbol

Returns the highest priority available price within the time frame, using the symbol instead of the asset token address. If timeframe is not provided, it will be ignored.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
        PriceBySymbol {
        symbol: String,
        timeframe: Option<u64>,
    },
```

| Key         | Type   | Description                                        |
| ----------- | ------ | -------------------------------------------------- |
| `symbol`    | String | Symbol of the asset to return price for (ex. AAPL) |
| `timeframe` | u64    | Optional timeframe to return the price at          |

#### Response

Same as `Price` QueryMsg
{% endtab %}

{% tab title="JSON" %}
```json
{
"price_by_symbol": {
    "symbol": "AAPL",
    "timeframe": 23451234
    }
}
```

| Key         | Type   | Description                                        |
| ----------- | ------ | -------------------------------------------------- |
| `symbol`    | String | Symbol of the asset to return price for (ex. AAPL) |
| `timeframe` | u64    | Optional timeframe to return the price at          |

#### Response

Same as `Price` QueryMsg
{% endtab %}
{% endtabs %}

### PriceList

Returns all registered proxy prices for the provided `asset_token`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    PriceList { asset_token: String },
```

| Key           | Type   | Description                                                            |
| ------------- | ------ | ---------------------------------------------------------------------- |
| `asset_token` | String | Address of the asset token contract to return all available prices for |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PriceListResponse {
    pub price_list: Vec<(u8, ProxyInfoResponse, PriceQueryResult)>, // (priority, proxy_info, result)
}
```

| Key          | Type                                           | Description                                                                                                            |
| ------------ | ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `price_list` | Vec<(u8, ProxyInfoResponse, PriceQueryResult)> | Returns all proxy sources available for a given symbol asset, including priority, proxy information and price queries. |
{% endtab %}

{% tab title="JSON" %}
```json
{
"price_list": {
    "asset_token": "terra1..."
    }
}
```

| Key           | Type   | Description                                            |
| ------------- | ------ | ------------------------------------------------------ |
| `asset_token` | String | Address of the token contract to return price list for |
{% endtab %}
{% endtabs %}

### PriceListBySymbol

Returns all registered proxy prices for the provided `asset_token`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    PriceListBySymbol { symbol: String },
```

| Key      | Type   | Description                                         |
| -------- | ------ | --------------------------------------------------- |
| `symbol` | String | Symbol of the asset to return all price sources for |

#### Response

Same as `PriceList`
{% endtab %}

{% tab title="JSON" %}
```json
{
"price_list_by_symbol": {
    "symbol": "AAPL"
    }
}
```

| Key      | Type   | Description                                         |
| -------- | ------ | --------------------------------------------------- |
| `symbol` | String | Symbol of the asset to return all price sources for |

#### Response

Same as `PriceList`
{% endtab %}
{% endtabs %}

### AssetSymbolMap

Returns the map of `asset_token` to `symbol`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    AssetSymbolMap {
        start_after: Option<String>, // address for pagination
        limit: Option<u32>,
    },
```

| Key           | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| `start_after` | String | Address for pagination          |
| `limit`       | `u32`  | Max number of entries to return |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct AssetSymbolMapResponse {
    pub map: Vec<(String, String)>, // address, symbol
}
```

| Key       | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| `address` | String | Address of the proxy contract |
| `symbol`  | String | Symbol of the asset           |
{% endtab %}

{% tab title="JSON" %}
```json
{
"asset_symbol_map": {
    "start_after": "terra1...",
    "limit": 10
    }
}
```

| Key           | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| `start_after` | String | Address for pagination          |
| `limit`       | u32    | Max number of entries to return |

#### Response

```json
{
"asset_symbol_map_response": {
    "map": [
        ("terra1...", "AAPL"),
        ("terra1...", "GOOGL"),
        ...
        ]
    }
}
```

| Key       | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| `address` | String | Address of the proxy contract |
| `symbol`  | String | Symbol of the asset           |
{% endtab %}
{% endtabs %}



### CheckSource

Check to see if `proxy_addr` is whitelisted and has price feed for the specified `symbol`. Returns the `PriceResponse` or `error` to check if the price feed is valid or not.&#x20;

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubQueryMsg {
    CheckSource { proxy_addr: String, symbol: String },
```

| Key          | Type   | Description                   |
| ------------ | ------ | ----------------------------- |
| `proxy_addr` | String | Address of the proxy contract |
| `symbol`     | String | Symbol of the asset           |

#### Response

Same as `PriceResponse` or an Error
{% endtab %}

{% tab title="JSON" %}
```json
{
"check_source": {
    "proxy_addr": "terra1...",
    "symbol": "AAPL"
    }
}
```

#### Response

Same as `PriceResponse` or an Error
{% endtab %}
{% endtabs %}
