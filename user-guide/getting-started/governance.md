# Governance

The **Governance** page displays operations related to MIR staking and creating / voting on polls. For more details regarding the Mirror Governance process, [click here](../../protocol/governance.md).

## Stake / Unstake MIR Tokens

MIR tokens are distributed as reward for staked LP tokens, which are generated when user provides liquidity. [Click here](stake.md) to learn more about staking LP tokens.

In order to vote on governance polls, MIR must be staked to be used as voting power.

1. Navigate to the [**Governance**](https://app-staging.mirrorprotocol.com/gov) page. The Governance page displays the following data:
2. **Market information about MIR Staking**: Information about current staked MIR in the market, and staking ratio are displayed
3. **MIR Staking Position:** MIR staking pool box displays expected APR \(annual percentage rate\) from staking MIR tokens, user's staked and unstaked MIR balances.
4. **Poll History**: Each poll has a unique poll ID, poll type \(whitelist or parameter change\), poll result, voting ratio and end time.
5. Click **`STAKE`**

![](../../.gitbook/assets/image%20%2840%29.png)

6. Select either **Stake** / **Unstake**

![](../../.gitbook/assets/image%20%2838%29.png)

7. Enter amount to Stake or Unstake

![](../../.gitbook/assets/image%20%2830%29.png)

8. Click **`STAKE`** / **`UNSTAKE`**. _\*\*_Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%2867%29.png)

## Create Poll

1. Navigate to the [**Governance**](https://app-staging.mirrorprotocol.com/gov) page

![](../../.gitbook/assets/image%20%2851%29.png)

2. Click **`CREATE POLL`**

![](../../.gitbook/assets/image%20%2843%29.png)

3. Select either **Whitelist** or **Parameter Change**

![](../../.gitbook/assets/image%20%2820%29.png)

4. After entering desired values, click activated button to confirm. 

![](../../.gitbook/assets/image%20%2839%29.png)

5. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%2861%29.png)

## Vote on Poll

1. Navigate to the [**Governance**](https://app-staging.mirrorprotocol.com/gov) page

![](../../.gitbook/assets/image%20%2854%29.png)

2. Select poll that is `IN PROGRESS`

![](../../.gitbook/assets/image%20%2845%29.png)

3. Click **`Vote`**

![](../../.gitbook/assets/image%20%2826%29.png)

4. Select **YES** or **NO**. 

![](../../.gitbook/assets/image%20%2831%29.png)

5. Click **`Submit`** to confirm. Station Extension should prompt you to sign the transaction. Confirm the details presented and input your password to sign.

![](../../.gitbook/assets/image%20%2844%29.png)

For every poll to be passed, they must meet the conditions below:

* Voting quorum must be greater than or equal to 10%. Without meeting this condition, no matter how much portion of the voting power has been voted Yes, the poll is ineffective.
* Over 50% of voting power which is dedicated to the poll must be **Yes**
* Voting period \(number of blocks corresponding to 2 weeks\) has ended
* After the voting period is over and the poll passes, it takes 7 days \(number of blocks corresponding to 7 days\) for the poll to become effective in the protocol 

