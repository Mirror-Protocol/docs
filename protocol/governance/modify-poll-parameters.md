# Modify Poll Parameters

On Mirror Protocol, there are three different types of governance polls:

* **Default Poll**: Polls including text poll, community grants, parameter registration and modification polls and asset whitelisting polls.&#x20;
* **Migration Poll**: Polls to execute migrations through Mirror governance.&#x20;
* **Authorization Poll:** Polls to transfer admin key rights to an address specified through Mirror Governance, or to modify governance configurations.&#x20;

Poll parameter modifications are considered as **Authorization Poll,** and has the same `quorum`, `threshold`, `voting_period` and `proposal deposit`.

{% hint style="warning" %}
Due to sensitivity of **migration** and **authorization** polls, Mirror web application does not provide user interface to submit these polls to modify the configuration of the two polls.
{% endhint %}

### 1. Default Poll Parameter

Default poll parameters can be modified by submitting a poll on Mirror web application. Currently, the default poll parameters are as below:&#x20;

| Parameter        | Current Value           |
| ---------------- | ----------------------- |
| Proposal Deposit | 1,000 MIR               |
| Voting Period    | 604800 seconds (7 days) |
| Quorum           | 18%                     |
| Threshold        | 50%                     |

### 2. Migration Poll Parameter

The developer wanting to create polls to modify migration poll parameters must include the following message within the `admin_action` parameter of the `CreatePoll` transaction:

```rust
pub enum PollAdminAction {
UpdateConfig {
    migration_poll_config: Option<PollConfig>,
    },
```

Currently, the migration poll parameters are as below:&#x20;

| Parameter        | Current Value                                                                                                     |
| ---------------- | ----------------------------------------------------------------------------------------------------------------- |
| Proposal Deposit | 2,000 MIR                                                                                                         |
| Voting Period    | 7 days (but if poll reaches quorum and threshold within the voting period, then poll can be executed immediately) |
| Quorum           | 40%                                                                                                               |
| Threshold        | 66.7%                                                                                                             |

### 3. Authorization Poll

The following message has to be included in `admin_action` parameter of `CreatePoll` transaction in order to create Authorization poll:&#x20;

```rust
pub enum PollAdminAction {
UpdateConfig {
    auth_admin_poll_config: Option<PollConfig>,
    },
```

Currently, the authorization poll parameters are as below:&#x20;

| Parameter          | Current Value                                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- |
| Proposal Deposit   | 3,000 MIR                                                                                                         |
| Voting Period      | 7 days (but if poll reaches quorum and threshold within the voting period, then poll can be executed immediately) |
| Quorum             | 40%                                                                                                               |
| Threshold          | 66.7%                                                                                                             |
| Admin claim period | 7 days                                                                                                            |

