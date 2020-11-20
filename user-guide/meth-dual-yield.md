# Staking on Ethereum

### Mirror Protocol Asset support on Ethereum

MIR and mAssets pools also exist on [**Uniswap**](https://app.uniswap.org/#/swap) which runs on Ethereum. Assets from Mirror Protocol can be transferred to Ethereum chain, via **Wormhole**. Wormhole provides cross-chain transfer between Terra and Ethereum, enabling Terra blockchain assets, including MIR, mAsset and UST, to be transferred to and traded on Uniswap.   
  
Assets transferred from Terra to Ethereum are “wrapped”, hence giving the assets names such as wUST \(wrapped UST\), or wmAsset \(wrapped mAsset\) as a result. **Wrapped** tokens are ERC-20 tokens backed 1:1 in value by its underlying asset, which in the case of mETH are CW-20 tokens, such as MIR and mAssets. Any transfer of wrapped tokens from Ethereum to Terra will “unwrap” these tokens. This allows users to trade and provide liquidity for all types of Mirror Protocol assets on both Mirror and Uniswap. 

### mETH.mirror.finance

\*\*\*\*[**mETH.mirror.finance**](https://meth.mirror.finance/) is a web interface which resembles Mirror Web App, to enable users to stake LP tokens that were minted by providing liquidity for wrapped MIR and mAssets on Uniswap. Similar to Mirror Web App, wMIR tokens are distributed to users as staking reward once everyday. 

Below are the key differences between Mirror Web App and mETH:  

| Features | **Mirror Web App** | **mETH** |
| :--- | :--- | :--- |
| Wallet used to sign Tx | Station Extension | Metamask |
| Asset name | MIR, mAsset, UST | wMIR, wmAsset, wUST |
| Asset type | CW-20 | ERC-20 |
| Trade | O | Uniswap |
| Mint | O | X |
| Pool | O | Uniswap |
| Stake | O | O |
| Governance | O | X |
| Staking Reward Token | MIR | wMIR |
| Tx fee token | UST | ETH |

mETH only provides interface for [**My Page**](https://app-staging.mirror.finance/my), where the user can view their asset holdings and staking positions, and [**Stake**](https://app-staging.mirror.finance/stake) page to earn wMIR reward by staking LP tokens. From My Page of mETH, user can send their wrapped tokens to other Ethereum addresses or to Terra addresses. Once the token is sent to any Terra address, they can be used on Mirror Web App.   
  
The interface for mETH staking is the same as Mirror Web App. To learn more about staking, please refer to this [user guide](getting-started/stake.md). To learn how to get LP tokens to use on mETH, please refer to [Uniswap docs](https://uniswap.org/docs/v2/).   
  


