# Governance

The **Governance** page displays operations related to MIR staking and creating / voting on polls. For more details regarding the Mirror Governance process, [click here](../../protocol/governance/).

## Governance Main

The Governance page displays the following data:

![](<../../.gitbook/assets/image (182).png>)

* **Market information about MIR Staking**: Information about current staked MIR in the market,  staking ratio and maximum APR are displayed. To learn how APR for governance is calculated, refer to [this page](../../protocol/mirror-token-mir.md#mir-staking-rewards).
* **MIR Staking Position:** MIR staking pool box displays user's staked and unstaked MIR balances.&#x20;
* **Poll History**: Each poll has a unique poll ID, poll type (whitelist or parameter change), poll result, voting ratio, and end time.

## Stake / Unstake MIR Tokens

MIR tokens are distributed as a reward for staked LP tokens, which are generated when users provide liquidity. [Click here](../../protocol/staking-tokens-lp-and-slp.md#lp-tokens) to learn more about staking LP tokens.

In order to vote on governance polls, MIR must be staked to be used as voting power.

1\. Navigate to the [**Governance**](https://terra.mirror.finance/gov) page and click **`Stake`**

![](<../../.gitbook/assets/image (130).png>)

2\. Select either **Stake** / **Unstake**

![](<../../.gitbook/assets/image (148).png>)

3\. Enter amount to Stake or Unstake

{% hint style="warning" %}
MIR tokens that are voted to on-going governance polls will be unstakable when the voting period is over.
{% endhint %}

![](<../../.gitbook/assets/image (131).png>)

4\. Click **`STAKE`** / **`UNSTAKE`**. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

## Create Poll

1. Navigate to the [**Governance**](https://terra.mirror.finance/gov) page and click `Create Poll`.

![](<../../.gitbook/assets/image (205).png>)

2\. Select a [poll type](../../protocol/governance/proposal-types.md) to submit.

![](<../../.gitbook/assets/image (132).png>)

3\. After entering desired inputs, click the activated button to confirm.&#x20;

![](<../../.gitbook/assets/image (147).png>)

5\. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

## Vote on Poll

1. Navigate to the [**Governance**](https://terra.mirror.finance/gov) page and select a poll that is `In Progress`.

![](<../../.gitbook/assets/image (196).png>)

2\. After checking the details of this poll, click **`Vote`**

![](<../../.gitbook/assets/image (204).png>)

3\. Select `YES`, `NO` or `ABSTAIN`, and enter the amount to use as voting power.

![](<../../.gitbook/assets/image (154).png>)

5\. Click **`Submit`** to confirm. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

##

&#x20;For every poll to be passed, they must meet the conditions below:

* Voting quorum must be greater than or equal to `quorum` (initially set to 10% on Mainnet) of the total staked MIR. Regardless of `YES` ratio, the poll will not pass if it does not meet the quorum.&#x20;
* **YES** votes must be greater than **NO** votes
* Voting period (number of blocks corresponding to 1 week) has ended
* After the voting period is over and the poll passes, it takes 1 day (number of blocks corresponding to a day) for the poll to become effective in the protocol.
