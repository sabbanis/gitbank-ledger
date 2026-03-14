# GitBank Ledger

This repository **is** the bank. GitHub is the backend.

| Path | Purpose |
|------|---------|
| `accounts/{user}.json` | Account profile |
| `balances/{user}.json` | Current balance |
| `transactions/{txId}.json` | Pending/in-flight transfer (lives on a PR branch) |
| `transactions/completed/{txId}.json` | Settled receipt (merged to main) |

## How a transfer works

1. Web app creates branch `tx/{txId}` and commits a `transactions/{txId}.json` file.
2. Web app opens a PR titled `Transfer: alice → bob $50`.
3. **Validator** GitHub Action reads sender's balance and labels the PR `approved` or `insufficient-funds`.
4. Merging the PR triggers the **Settlement** GitHub Action which:
   - Deducts from sender's balance file
   - Credits recipient's balance file
   - Marks the transaction `settled`
   - Copies receipt to `transactions/completed/`
5. The commit history is the immutable audit trail.

## Seed accounts

| User | Starting balance |
|------|-----------------|
| alice | $1,000.00 USD |
| bob | $1,000.00 USD |
