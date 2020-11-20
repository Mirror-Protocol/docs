# Architecture

This section describes provides a high-level overview regarding the technical implementation of Mirror Protocol.

{% hint style="warning" %}
Even with a thorough understanding of Mirror Protocol, it is highly recommended to interact with Mirror through client channels such as the Mirror Web App or Mirror.js.
{% endhint %}

## Smart Contracts

The source code for Mirror smart contracts can be found on [GitHub](https://github.com/Mirror-Protocol/mirror-contracts). Mirror Protocol is deployed with one of each of the following contracts, organized through the Factory.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Contract</th>
      <th style="text-align:left">Function</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="contracts/collector.md">Collector</a>
      </td>
      <td style="text-align:left">Gathers protocol fees incurred from CDP withdrawals and liquidations and
        sends to Gov</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="contracts/community.md">Community</a>
      </td>
      <td style="text-align:left">Manages the <a href="protocol/governance.md#community-pool">Community Pool</a> fund</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="contracts/factory.md">Factory</a>
      </td>
      <td style="text-align:left">Central directory that organizes the various component contracts of Mirror</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="contracts/gov.md">Gov</a>
      </td>
      <td style="text-align:left">
        <p>Allows other Mirror contracts to be controlled by decentralized governance</p>
        <p>Distributes MIR received from Collector to MIR stakers</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="contracts/mint.md">Mint</a>
      </td>
      <td style="text-align:left">Handles CDP creation, management and liquidation</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="contracts/oracle.md">Oracle</a>
      </td>
      <td style="text-align:left">Provides interface for oracle feeders to post prices for mAssets</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="contracts/staking.md">Staking</a>
      </td>
      <td style="text-align:left">Distributes MIR rewards from block reward to LP stakers</td>
    </tr>
  </tbody>
</table>

The Mirror Token \(MIR\) is a Terraswap CW20 Token instance that is created during the initial bootstrapping of the protocol and is registered with the Mirror Protocol core contracts.

When new mAssets are whitelisted, Mirror Protocol will create the following contract instances:

* Terraswap CW20 Token for the new mAsset
* Terraswap Pair for the new mAsset against UST
* Terraswap CW20 Token for the new mAsset's LP Token

