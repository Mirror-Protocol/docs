# Mirror API

{% hint style="info" %}
This section provides a brief guide on how to get set up with Mirror API. For more information, be sure to check out source code and documentation on [GitHub](https://github.com/Mirror-Protocol/mirror.js).
{% endhint %}

The **Mirror API** \(also known as **Mirror Graph**\) is an GraphQL-based data service that allows anybody to query data regarding the current and aggregate application state of the Mirror Protocol. Potential consumers of data include: dashboards, mAsset arbitrage trading bots, dApp activity trackers, etc.

## Endpoints

Since Mirror API is a GraphQL-based data service, it is not accessed like a traditional REST API with many different endpoints. Instead, there is only one endpoint served over HTTP, to which you can submit a POST request containing a GraphQL query. 

|  | Chain ID | URL |
| :--- | :--- | :--- |
| Mainnet | `columbus-4` | [https://graph.mirror.finance/graphql](https://tequila-graph.mirror.finance/graphql) |
| Testnet | `tequila-0004` | [https://tequila-graph.mirror.finance/graphql](https://tequila-graph.mirror.finance/graphql) |

## Usage

You can access Mirror API through a variety of interfaces that support GraphQL. Below are some examples of how you can use Mirror API:

### GraphiQL

Playground: [https://graph.mirror.finance/graphql](https://tequila-graph.mirror.finance/graphql)

An easy way to explore the Mirror API is to use your browser to access the GraphiQL interface  served directly on the endpoint address. GraphiQL is an interactive interface where you can construct queries alongside easy access to the API's schema and documentation. This allows for quick, iterative experimentation as you can directly run your queries and compare results.

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
      {
        "symbol":"mTSLA",
        "name":"Tesla",
        "prices":{
          "price":"589.509999"
        }
      },
      {
        "symbol":"mNFLX",
        "name":"Netflix",
        "prices":{
          "price":"491.360000"
        }
      },
      {
        "symbol":"mQQQ",
        "name":"Invesco QQQ Trust",
        "prices":{
          "price":"301.690000"
        }
      },
      {
        "symbol":"mTWTR",
        "name":"Twitter",
        "prices":{
          "price":"46.849999"
        }
      },
      {
        "symbol":"mMSFT",
        "name":"Microsoft Corporation",
        "prices":{
          "price":"214.610000"
        }
      },
      {
        "symbol":"mAMZN",
        "name":"Amazon.com",
        "prices":{
          "price":"3190.000000"
        }
      },
      {
        "symbol":"mBABA",
        "name":"Alibaba Group Holdings Ltd ADR",
        "prices":{
          "price":"265.489999"
        }
      },
      {
        "symbol":"mIAU",
        "name":"iShares Gold Trust",
        "prices":{
          "price":"17.119999"
        }
      },
      {
        "symbol":"mSLV",
        "name":"iShares Silver Trust",
        "prices":{
          "price":"21.519999"
        }
      },
      {
        "symbol":"mUSO",
        "name":"United States Oil Fund, LP",
        "prices":{
          "price":"31.050000"
        }
      },
      {
        "symbol":"mVIXY",
        "name":"ProShares VIX",
        "prices":{
          "price":"14.049999"
        }
      }
    ]
  }
}
```

