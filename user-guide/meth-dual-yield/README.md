# Mirror on Ethereum

### Mirror Protocol Asset Support on Ethereum

MIR and mAssets pools also exist on [Uniswap](https://app.uniswap.org/#/swap) which runs on Ethereum. Assets from Mirror Protocol can be transferred to Ethereum chain through a custom bridge named [Shuttle](https://github.com/terra-project/shuttle). Shuttle facilitates cross-chain transfers between Terra and Ethereum, thereby enabling Terra blockchain assets, including MIR, mAsset and UST, to be transferred to and traded on [Uniswap](https://uniswap.org/). 

Assets transferred from Terra to Ethereum will have the same name and ticker, but these tokens will follow the ERC-20 token standard and backed 1:1 in value by its underlying CW-20 assets such as MIR and mAssets on the Terra blockchain.  This allows users to trade and provide liquidity for all types of Mirror Protocol assets on both Terraswap \(Terra\) and Uniswap \(Ethereum\). 

### ETH.mirror.finance

\*\*\*\*[**ETH.mirror.finance**](https://eth.mirror.finance/) **or mETH** is a web interface that supports the staking of LP tokens generated from the liquidity provision of Mirror Protocol assets on Ethereum-side. It resembles the interface of Mirror Web Application but unlike the Terra counterpart, mETH only supports staking and viewing of assets and positions. All trading and liquidity provision transactions for MIR and mAssets can be executed on Uniswap. Similar to Mirror Web App, MIR tokens are distributed to mETH users as staking rewards once per day. 

{% hint style="warning" %}
[Metamask](https://metamask.io) is required for signing transactions on mETH.   
User must hold ETH in order to pay transaction fees for mETH.
{% endhint %}

Below are the key differences between Mirror Web App and mETH:  

| Features | **Mirror \(Terra\)** | **mETH \(Ethereum\)** |
| :--- | :--- | :--- |
| **Wallet Support** | \*\*\*\*[Station Extension](../getting-started/#terra-station-extension) | [Metamask](https://metamask.io/), [WalletConnect](https://walletconnect.org/), [Coinbase](https://wallet.coinbase.com) |
| **Asset type** | CW-20 | ERC-20 |
| **Average Block Time** | 6 seconds | 15 seconds |
| **Trade** | O \(connected to Terraswap\) | Uniswap |
| **Mint** | O | X |
| **Pool** | O \(connected to Terraswap\) | Uniswap |
| **Stake** | O | O |
| **Governance** | O | X |
| **Staking Reward Token** | MIR | MIR \(ERC-20\) |
| **Tx fee token** | UST | ETH |

mETH only provides interfaces for the [My Page](https://eth.mirror.finance/my) to view asset holdings and staking positions, and the [Stake](https://eth.mirror.finance) page to earn MIR reward by staking LP tokens. 

