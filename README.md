

# Vote-Chain- - Decentralized Voting System


## Overview

The **Decentralized Voting System** is a tamper-proof, on-chain governance contract that allows communities to create proposals and vote transparently. It supports weighted voting, permissioned proposal creation, and a robust structure for tallying results. This smart contract is ideal for DAOs, community decisions, and governance of decentralized applications.

---

## Features

- ✅ **Proposal Creation** with multiple voting options  
- ✅ **Weighted Voting System** based on voter privileges  
- ✅ **Transparent Result Calculation** after voting ends  
- ✅ **Admin Controls** for setting governance tokens and voter weights  
- ✅ **Vote Modification** (revote with overwrite)  
- ✅ **Execution Phase** to finalize proposal outcomes  

---

## Data Structures

### 🗂 Maps

- **`proposals`** – Stores proposal metadata: title, description, options, timing, etc.  
- **`votes`** – Stores individual votes per user per proposal.  
- **`vote-counts`** – Tracks cumulative vote counts per option.  
- **`voter-weights`** – Stores voting power per principal.

###  Variables

- **`proposal-count`** – Tracks number of proposals created.  
- **`governance-token`** – Optional principal defining governance token logic.  
- **`admin`** – The contract's admin principal.

---

## Error Codes

| Error Code | Meaning |
|------------|---------|
| `u100` | Not authorized |
| `u101` | Proposal already executed |
| `u102` | Proposal does not exist |
| `u103` | Already voted (logic internally overwrites instead) |
| `u104` | Voting period is over |
| `u105` | Voting period still active |
| `u106` | Invalid voting option |
| `u107` | Voter has zero weight |

---

## Functions

###  Read-Only

- `get-proposal(proposal-id)` – Fetch proposal details.
- `get-vote(proposal-id, voter)` – Fetch a user's vote.
- `get-vote-count(proposal-id, option-idx)` – Tally per option.
- `get-voter-weight(voter)` – Weight of a voter.
- `get-proposal-results(proposal-id)` – Aggregated results (list of counts).
- `is-voting-active(proposal-id)` – Check if voting is ongoing.

###  Public

- `set-admin(new-admin)` – Change admin (admin-only).
- `set-governance-token(token-contract)` – Set an optional governance token (admin-only).
- `set-voter-weight(voter, weight)` – Assign vote power (admin-only).
- `create-proposal(title, description, options, voting-period)` – Start a new proposal. Requires non-zero weight and at least 2 options.
- `vote(proposal-id, option-idx)` – Vote or change vote.
- `execute-proposal(proposal-id)` – Mark proposal as executed after voting ends.

---

## Voting Lifecycle

1. **Admin assigns voter weights** via `set-voter-weight`.
2. **Eligible voter** creates a proposal via `create-proposal`.
3. **Community members** vote during the open block range.
4. **Votes can be changed** during the voting period.
5. **After end-block**, creator or admin calls `execute-proposal`.
6. **Results can be queried** using `get-proposal-results`.

---

## Initialization

Upon deployment, the contract automatically:
- Sets the deployer as the `admin`.
- Assigns the admin a voting weight of `u100`.

```clarity
(map-set voter-weights { voter: (var-get admin) } { weight: u100 })
```

---

## Example Use Cases

- DAO governance proposals
- Community decision-making platforms
- Token-weighted elections
- On-chain public referendums

---

## Security Considerations

- Only the `admin` can manage voter weights or set a governance token.
- Proposal creation is gated by voter weight > 0.
- Votes are stored immutably but can be overwritten during voting period.
- Proposal results cannot be tampered with after execution.

---

## Future Improvements

- Integration with token contracts for automatic weight calculation  
- Off-chain data syncing with event indexing  
- Proposal execution logic extensions for automatic on-chain actions  

---

