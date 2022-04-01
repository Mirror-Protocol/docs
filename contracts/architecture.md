# Architecture

This section describes provides a high-level overview regarding the technical implementation of Mirror Protocol.

{% hint style="info" %}
Even with a thorough understanding of Mirror Protocol, it is highly recommended to interact with Mirror through client channels such as the [Mirror Web App](../user-guide/getting-started/) or [Mirror.js.](../developer-tools/mirror.js.md)
{% endhint %}

## Smart Contracts

The source code for Mirror smart contracts can be found on [GitHub](https://github.com/Mirror-Protocol/mirror-contracts). Mirror Protocol is deployed with one of each of the following contracts, organized through the Factory.

| Contract                                  | Function                                                                                                                                                  |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Collector](collector.md)                 | Gathers protocol fees incurred from CDP withdrawals and liquidations and send to Gov                                                                      |
| [Community](community.md)                 | Manages the [Community Pool](../protocol/governance/#community-pool) fund                                                                                 |
| [Factory](factory.md)                     | Central directory that organizes the various component contracts of Mirror                                                                                |
| [Gov](gov.md)                             | <p>Allows other Mirror contracts to be controlled by decentralized governance</p><p>Distributes MIR received from Collector to MIR stakers and voters</p> |
| [Admin Manager](admin-manager.md)         | Contract owned by [Gov](gov.md) which enables contract migrations and admin actions through Mirror governance consensus.                                  |
| [Mint](mint.md)                           | Handles both long and short CDP creation, management, and liquidation                                                                                     |
| [Lock](lock.md)                           | Responsible for locking up UST from short CDP                                                                                                             |
| [Oracle Hub](tefi-oracle.md)              | Provides an interface for registering new oracle provider and fetching asset prices with highest priority                                                 |
| [Collateral Oracle](collateral-oracle.md) | Feeds price and collateral `multiplier` for each collateral asset type                                                                                    |
| [Staking](staking.md)                     | Distributes MIR rewards from block reward to LP and sLP stakers                                                                                           |
| [Limit Order](limit-order.md)             | Registers and executes swap orders at submitted limit price and amount                                                                                    |

The Mirror Token (MIR) is a Terraswap CW20 Token instance that is created during the initial bootstrapping of the protocol and is registered with the Mirror Protocol core contracts.

When new mAssets are whitelisted, Mirror Protocol will create the following contract instances:

* Terraswap CW20 Token for the new mAsset
* Terraswap Pair for the new mAsset against UST
* Terraswap CW20 Token for the new mAsset's LP Token
* Terraswap CW20 Token for the new mAsset's sLP Token
