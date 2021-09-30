# Mirror API

{% hint style="info" %}
This section provides a brief guide on how to get set up with Mirror API. For more information, be sure to check out source code and documentation on [GitHub](https://github.com/Mirror-Protocol/mirror-graph).
{% endhint %}

The **Mirror API** \(also known as **Mirror Graph**\) is an GraphQL-based data service that allows anybody to query data regarding the current and aggregate application state of the Mirror Protocol. Potential consumers of data include: dashboards, mAsset arbitrage trading bots, dApp activity trackers, etc.

## Endpoints

Since Mirror API is a GraphQL-based data service, it is not accessed like a traditional REST API with many different endpoints. Instead, there is only one endpoint served over HTTP, to which you can submit a POST request containing a GraphQL query.

|  | Chain ID | URL |
| :--- | :--- | :--- |
| Mainnet | `columbus-5` | [https://graph.mirror.finance/graphql](https://graph.mirror.finance/graphql) |
| Testnet | `bombay-12` | [https://bombay-graph.mirror.finance/graphql](https://bombay-graph.mirror.finance/graphql) |

## Usage

You can access Mirror API through a variety of interfaces that support GraphQL. Below are some examples of how you can use Mirror API:

### GraphiQL

Playground: [https://graph.mirror.finance/graphql](https://tequila-graph.mirror.finance/graphql)

An easy way to explore the Mirror API is to use your browser to access the GraphiQL interface served directly on the endpoint address. GraphiQL is an interactive interface where you can construct queries alongside easy access to the API's schema and documentation. This allows for quick, iterative experimentation as you can directly run your queries and compare results.

### HTTP

You can access Mirror API through standard HTTP by simply sending a GraphQL query to the endpoint through a POST request. The response will be in JSON.

If you intend to use Mirror API in a programmatic context \(like within JavaScript or Python\), it is recommended to use a GraphQL client such as [Relay](https://relay.dev/) or [Apollo](https://github.com/apollographql/apollo-client) to handle the ceremony of making HTTP requests and parsing returned responses.

#### Example using cURL

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"query": "{ assets { symbol name } }"}' \
https://tequila-graph.mirror.finance/graphql
```

#### Response \(prettified\)

```javascript
{
  "data":{
    "assets":[
      {
        "symbol":"MIR",
        "name":"Mirror",
        "prices":{
          "price":null
        }
      },
      {
        "symbol":"mAAPL",
        "name":"Apple",
        "prices":{
          "price":"120.825733"
        }
      },
      {
        "symbol":"mGOOGL",
        "name":"Google",
        "prices":{
          "price":"1766.556244"
        }
      },
      ...
    ]
}
```

