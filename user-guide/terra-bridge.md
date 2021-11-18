---
description: Web application to transfer Terra's tokens to other blockchain networks
---

# Terra Bridge

[Terra Bridge](https://bridge.terra.money) enables cross-chain transfer of all tokens supported by [Shuttle](https://github.com/terra-project/shuttle), including Terra native tokens, most mAssets and also other token types from Terra ecosystem.&#x20;

The list of transferable Mirror Protocol assets can be found on [Interchain Access](../networks.md) page.&#x20;

{% hint style="warning" %}
The Terra Bridge web app is only available for Chromium-based web browsers.&#x20;
{% endhint %}

### Supported Wallets

| Blockchain | Supported Wallets                                                                                                                                                                                                                       |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Terra      | [Terra Station Extension](https://terra.money/extension)                                                                                                                                                                                |
| Ethereum   | [MetaMask](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en), [Coinbase](https://wallet.coinbase.com) or [Trustwallet](https://trustwallet.com) for [WalletConnect](https://walletconnect.org) |
| BSC        | [Binance Chain Wallet](https://chrome.google.com/webstore/detail/binance-chain-wallet/fhbohimaelbohpjbbldcngcnapndodjp?hl=en) or [MetaMask](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en)  |

## Using Terra Bridge

{% hint style="danger" %}
$1 or 0.1% fee (which ever is greater) from the transfer amount is charge on cross-chain transfer using the Shuttle bridge. \
**A transaction with amount smaller than $1 value will be ignored.**
{% endhint %}

1\. Go to [Terra Bridge](https://bridge.terra.money) page on a Chromium-based web browser.

![](<../.gitbook/assets/image (119).png>)

2\. You may connect your wallet by doing one of the actions below:&#x20;

* Click `Connect Wallet` button
* From the **From** section, select the network to transfer your tokens from.&#x20;

![](<../.gitbook/assets/image (125).png>)

3\. If **Ethereum** or **BSC** is selected, a pop-up with a list of supported wallet will appear. Select a wallet which you currently use.&#x20;

![](<../.gitbook/assets/image (121).png>)

3\. Once the right wallet is connected, click on **Assets** section to select the token to transfer.

![](<../.gitbook/assets/image (124).png>)

4\. Search and select a token type from the pop-up list.&#x20;

![](<../.gitbook/assets/image (122).png>)

5\. Enter **Amount** of tokens to transfer, and **Destination** address of the recipient.

![](<../.gitbook/assets/image (126).png>)

6\. By selecting Next button, a Confirm pop-up is provided to double-check the information entered by the user. If all details on the pop-up is correct, select `Confirm`.&#x20;

![](<../.gitbook/assets/image (128).png>)

7\. Once `Confirm` is pressed, the connected wallet will pop-up, prompting the user to sign the transaction. The signed transaction will be subsequently submitted to the  blockchain for confirmation.&#x20;

![](<../.gitbook/assets/image (120).png>)
