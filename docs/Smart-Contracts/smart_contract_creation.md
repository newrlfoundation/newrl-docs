---
sidebar_position: 3
slug: /smart-contracts/creation
---

# Creation of Smart Contract on Newrl



Smart Contract on Newrl can be created by submiting valid `transaction`. Check [transaction format](../Dapps/transaction-format.md) for valid transaction example.


### Example:

Below represents a transaction for creating a new smart contract:
```json
{
  "timestamp": 1664278926000,
  "trans_code": "977a4b88a1bcb4ee51dea4dfb008b0809aa690f3",
  "type": 3,
  "currency": "NWRL",
  "fee": 1,
  "descr": "",
  "valid": 1,
  "specific_data": {
    "address": "ctd16b9576821b763358f7f589f577e171dfc8f1dd",
    "function": "setup",
    "signers": [
      "0xe738ffa75c377d07819d21170c7594aa89ce1618"
    ],
    "params": {
      "creator": "0xe738ffa75c377d07819d21170c7594aa89ce1618",
      "ts_init": null,
      "name": "sample_template",
      "version": "1.0.0",
      "actmode": "hybrid",
      "status": 0,
      "next_act_ts": null,
      "signatories": {
        "initialise_liquidity": null,
        "value_issue": null,
        "update_entry": null,
        "create_entry": null,
        "sample_validate": null
      },
      "parent": null,
      "oracleids": null,
      "selfdestruct": 1,
      "contractspecs": {},
      "legalparams": {}
    }
  }
}

```
### signatories
Key value pair of function name as the key and wallet address which can call these methods as value and `null` in case function can be called by any wallet. 
After successful submission of the valid transaction smart contract will be dpeloyed.
