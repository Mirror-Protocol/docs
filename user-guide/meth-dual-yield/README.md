# Staking on Ethereum

### Mirror Protocol Asset support on Ethereum

MIR and mAssets pools also exist on [**Uniswap**](https://app.uniswap.org/#/swap) which runs on Ethereum. Assets from Mirror Protocol can be transferred to Ethereum chain, through a custom bridge named **Shuttle**. Shuttle provides cross-chain transfer between Terra and Ethereum, enabling Terra blockchain assets, including MIR, mAsset and UST, to be transferred to and traded on Uniswap. 

Assets transferred from Terra to Ethereum are will have the same name and ticker, but these tokens will now be ERC-20 tokens, backed 1:1 in value by its underlying CW-20 assets such as MIR and mAssets.  This allows users to trade and provide liquidity for all types of Mirror Protocol assets on both Mirror Protocol and Uniswap, which runs on Ethereum. 

### mETH.mirror.finance

\*\*\*\*[**mETH.mirror.finance**](https://meth.mirror.finance/) is a web interface which resembles Mirror Web App, to enable users to stake LP tokens that were minted by providing liquidity for wrapped MIR and mAssets on Uniswap. Similar to Mirror Web App, wMIR tokens are distributed to users as staking reward once everyday. 

{% hint style="warning" %}
The mETH staking requires [Metamask](https://metamask.io) to be installed. 
{% endhint %}

Below are the key differences between Mirror Web App and mETH:  

| Features | **Mirror Web App** | **mETH** |
| :--- | :--- | :--- |
| **Wallet used to sign Tx** | \*\*\*\*[Station Extension](../getting-started/#terra-station-extension) | [Metamask](https://metamask.io/) |
| **Asset type** | CW-20 | ERC-20 |
| **Trade** | O | Uniswap |
| **Mint** | O | X |
| **Pool** | O | Uniswap |
| **Stake** | O | O |
| **Governance** | O | X |
| **Staking Reward Token** | MIR | wMIR |
| **Tx fee token** | UST | ETH |

mETH only provides interface for [**My Page**](https://app-staging.mirror.finance/my), where the user can view their asset holdings and staking positions, and [**Stake**](https://app-staging.mirror.finance/stake) page to earn MIR reward by staking LP tokens. From My Page of mETH, user can send their ERC-20 Mirror tokens to other Ethereum addresses or to Terra addresses. Once the token is sent to any Terra address, they can be used on Mirror Web App.   
  
The interface for mETH staking is the same as Mirror Web App. To learn more about staking, please refer to this [user guide](staking_guide.md). To learn how to get LP tokens to use on mETH, please refer to [Uniswap docs](https://uniswap.org/docs/v2/).   
  


