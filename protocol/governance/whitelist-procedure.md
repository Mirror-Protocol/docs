# Whitelist Procedure

{% hint style="info" %}
Whitelisting discussion can be started at the [Mirror Protocol Forum](https://forum.mirror.finance/)
{% endhint %}

For an mAsset to be fully functional on the Mirror Protocol, _two_ governance polls will need to be passed. The whitelisting consists of three major steps.

1. A user should make a [**Whitelist a New mAsset**](proposal-types.md#2-whitelist-a-new-masset) ****poll following the instructions listed. The proposal should be as detailed as possible, providing all the necessary information that a voter would need to be able to make an informed decision.
2. Once the above poll is nearing completion and seems like it will pass, a user should request oracle support for said asset from either Band Protocol \(**contact information to be announced shortly**\) or Chainlink \(**support@smartcontract.com**\). The user should refer to the specific proposal that has passed and include a link.
3. Once the oracle has tested for stability and is fully operational, any user may submit a [**Register Whitelist Parameters**](proposal-types.md#3-register-whitelist-parameters) ****with the provided oracle address in the `Oracle Feeder` field.
4. If the poll again passes, the new Mirrored Asset is immediately added to the active set of assets with prices being fed. However, there is an approximate one week delay before the new asset can be added to ensure that the feed is running smoothly on the Mirror Protocol.



