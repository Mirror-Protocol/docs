# Oracle Whitelist Procedure

{% hint style="warning" %}
No web application user interface is provided to enable to creation of these polls as adding a new oracle source which can be malicious can result in unwanted attacks on borrow and short positions.
{% endhint %}

New oracle service providers such as Chainlink can be added to Mirror Protocol through voting on Mirror Governance.&#x20;

Before creating a poll to whitelist a new oracle provider, a proxy contract owned and managed by the oracle provider party must be created and deployed onto Terra blockchain, using the smart contract template provided [here](https://github.com/terra-money/tefi-oracle-contracts/tree/main/contracts/oracle-proxy-template). The proxy contract must provide mapping between underlying assets symbol and price denominated in UST.

In order to suggest a new oracle provider to Mirror Protocol, the following `execute_msg` must be included in `CreatePoll` transaction to be directed to Factory contract using `PassCommand` and then to the Oracle Hub contract when the poll is being executed.&#x20;

```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema, Debug)]
#[serde(rename_all = "snake_case")]
pub enum HubExecuteMsg {
    WhitelistProxy {
        proxy_addr: String,
        provider_name: String,
    },
```

Once this poll passes through Mirror governance, users can choose this oracle provider when registering new assets onto the protocol.&#x20;
