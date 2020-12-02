# Mirror on Ethereum

### Mirror Protocol Asset Support on Ethereum

MIR and mAssets pools also exist on [Uniswap](https://app.uniswap.org/#/swap) which runs on Ethereum. Assets from Mirror Protocol can be transferred to Ethereum chain, through a custom bridge named [Shuttle](https://github.com/terra-project/shuttle). Shuttle provides cross-chain transfer between Terra and Ethereum, enabling Terra blockchain assets, including MIR, mAsset and UST, to be transferred to and traded on Uniswap. 

Assets transferred from Terra to Ethereum are will have the same name and ticker, but these tokens will now be ERC-20 tokens, backed 1:1 in value by its underlying CW-20 assets such as MIR and mAssets.  This allows users to trade and provide liquidity for all types of Mirror Protocol assets on both Mirror Protocol and Uniswap, which runs on Ethereum. 

### ETH.mirror.finance

\*\*\*\*[**ETH.mirror.finance**](https://eth.mirror.finance/) **or mETH** is a web interface which supports staking of LP tokens generated from liquidity provision of Mirror Protocol assets on Ethereum-side. It resembles the interface of Mirror Web Application but only supports staking and viewing of assets and positions. All trading and liquidity provision transactions for MIR and mAssets can be executed on Uniswap. Similar to Mirror Web App, MIR tokens are distributed to mETH users as staking reward once everyday. 

{% hint style="warning" %}
[Metamask](https://metamask.io) is required for signing transactions on mETH.   
User must hold ETH in order to pay transaction fees for mETH.
{% endhint %}

Below are the key differences between Mirror Web App and mETH:  

| Features | **Mirror \(Terra\)** | **mETH \(Ethereum\)** |
| :--- | :--- | :--- |
| **Wallet used to sign Tx** | \*\*\*\*[Station Extension](../getting-started/#terra-station-extension) | [Metamask](https://metamask.io/) |
| **Asset type** | CW-20 | ERC-20 |
| **Average Block Time** | 6 seconds | 15 seconds |
| **Trade** | O | Uniswap |
| **Mint** | O | X |
| **Pool** | O | Uniswap |
| **Stake** | O | O |
| **Governance** | O | X |
| **Staking Reward Token** | MIR | MIR \(ERC-20\) |
| **Tx fee token** | UST | ETH |

mETH only provides interface for [My Page](https://eth.mirror.finance/my), where the user can view their asset holdings and staking positions, and [Stake](https://eth.mirror.finance) page to earn MIR reward by staking LP tokens. 

### Sending Tokens to Terra / Ethereum Address

From My Page of mETH, user can navigate to Send page and transfer ERC-20 Mirror tokens to other Ethereum or Terra addresses. Tokens transferred from mETH to Terra are converted back to CW-20 and  be used on Mirror Web App. 

![](../../.gitbook/assets/image%20%2892%29.png)

Once the user enters the recipient's address, the app will automatically determine which network this address belongs. Once, the user selects `SEND` and confirms on Metamask, the transaction will be submitted to the network for confirmation.   
  
To learn how to buy or get LP tokens to use on mETH, please refer to [Uniswap docs](https://uniswap.org/docs/v2/). 

