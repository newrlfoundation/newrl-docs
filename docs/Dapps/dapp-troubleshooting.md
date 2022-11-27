---
sidebar_position: 8
slug: /dapp-troubleshooting
---

# DApp Troubleshooting

This section lists some common issues for debugging DApps on chain.

### APIs

- [Check if transaction got included in a block](dapp-troubleshooting#check-if-transaction-got-included-in-a-block)


### Check if transaction got included in a block

Open [block explorer](https://www.newrlscan.io/block-explorer) and check if the transaction is add in the chain. If not wait for few seconds and verify if block index of chain is increasing.

![Check Txn](/img/newrlscan-check-txn.png)

If the chain is moving on and transaction isn't included there are two possibilities.
1. The transaction is waiting in some node's mempool for block inclusion.
2. The transaction failed during block inclusion and got deleted.

The scanner only list recent transactions. To check a transaction very old being included in chain, use the API from a node. 

![Get Txn](/img/api-get-transaction.png)