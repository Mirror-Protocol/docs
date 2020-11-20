# Mirror API

The **Mirror API** is a service that allows anybody to query data regarding the current protocol state of Mirror across all of its deployments.

## Usage

```graphql
query asset {
  asset(token: "terra1f9pk063a99g27l5nu83pd55x6rs649s3ax7pw3") {
    symbol
    # name
    # description
    # news {
    #   datetime
    #   source
    #   headline
    # }
    # positions {
    #   mint
    #   liquidity
    #   asCollateral
    # }
    prices {
      history(from: 1604016000000, to: 1604026800000, interval: 5) {
        timestamp
        price
      }
    #   price
    #   priceAt(timestamp: 1602232570097)
    #   oraclePrice
    #   oraclePriceAt(timestamp: 1602232570097)
    }
  }
}
```

