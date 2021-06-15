# Governance

{% hint style="info" %}
All community discussion can be done at the [Mirror Protocol Forum](https://forum.mirror.finance/)
{% endhint %}

Governance is the democratized process through which proposals for change in Mirror Protocol are introduced and accepted by the community through voting.

There are no admin keys with privileged access. After the initial bootstrapping of contracts, the [Gov contract](../../contracts/gov.md) is set to be the owner of the [Mirror Protocol contracts](../../contracts/architecture.md) and all changes must be made through the governance with the [procedure](./#procedure) defined in this section.

## Mirror Token

The [Mirror Token](../mirror-token-mir.md) \(MIR\) serves as Mirror Protocol's governance token. Only users with a staked MIR position can vote on polls, and each user receives voting power weighted by their amount of staked MIR. For every poll, a user can choose to allocate up to their total staked MIR. Users with higher MIR stake will therefore have more influence when deciding in governance polls.

{% hint style="info" %}
Although a user receives 1 vote per staked MIR for every poll, voting in polls does not have any effect on the user's current staking balance.
{% endhint %}

## Polls

New governance proposals in Mirror are called **polls.** Any user can create a poll by paying an initial deposit of MIR tokens. If the poll fails to pass the minimum voting quorum, the MIR deposit is given to MIR stakers and distributed proportionately according to their relative stake.

Polls consist of a text description of the proposition \(with perhaps an external link to further resources / discussions\), and include an executable message encoding the instructions to be run if it passes. The message will be executed with the privileges of the [Mirror Gov contract](../../contracts/gov.md), which has the power to invoke any function defined by the other Mirror smart contracts.

Once submitted, a poll can be voted on by the community until its voting period has concluded. If the poll passes quorum and threshold conditions \(defined below\), it is ratified and its contents can automatically be applied after a set period of time. These changes take effect without requiring updates to the core Mirror Protocol contracts.

{% hint style="danger" %}
Staked MIR tokens utilized in on-going polls cannot be withdrawn until the poll completes. In addition, the number of MIR used in a proposal **cannot** be modified after the vote as been submitted.
{% endhint %}

## Procedure

The following steps outline the governance procedure:

1. A new poll is created with an initial deposit that meets `proposal_deposit`
2. The poll enters the voting phase, where it can voted for by anybody with a staked MIR position. Users can vote `yes`, `no` or `abstain` and can assign how many of their staked MIR to use for voting.
3. The voting period ends after `voting_period` blocks have passed.
4. The poll's votes are tallied and **passes** if both quorum \(minimum participation of all staked MIR\) and threshold \(minimum ratio of `yes` to `no` votes\) are met.
5. If the poll passes, its contents will be executed after `effective_delay` blocks have ended. The poll must be executed prior to `expiration_period`, otherwise it will automatically expire and no longer be considered valid.

