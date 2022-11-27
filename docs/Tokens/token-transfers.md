---
sidebar_position: 6
slug: /token-transfers
---


# Token Transfers

Newrl supports two types of token transfers
1. Unilateral transfers(type5) where tokens are transferred from one wallet to another in a transaction.
2. Bilateral transfers(type6) where tokens are exchanged between two wallets in a single transaction.

## Unilateral Transfer [Type 5]
Type 5 transactions send tokens from one wallet to another. The sender[wallet1] has to sign the transaction. 
### Parameters
- `<transfer_type>` - integer 5
- `<asset1_code>` - string - Token code to sent
- `<asset2_code>` - string - Leave blank
- `<wallet1>` - string - From wallet
- `<wallet2>` - string - To wallet
- `<asset1_number>` - string - Amount to send
- `<asset2_number>` - string - Leave 0
- `<additional_data>` - dictionary - Any arbitrary data about the transaction for reference by dapp.


### Example transaction
```json
{
  "transaction": {
      "timestamp": 1669379031000,
      "trans_code": "b3df7d8ac5b7caa2cb31695028c0d890c22c72b7",
      "type": 5,
      "currency": "NWRL",
      "fee": 1000000,
      "descr": "",
      "valid": 1,
      "block_index": 0,
      "specific_data": {
          "transfer_type": 5,
          "asset1_code": "NWRL",
          "asset2_code": "",
          "wallet1": "0x1342e0ae1664734cbbe522030c7399d6003a07a8",
          "wallet2": "0xd6c038f5c25dae8a8f7350a58fb79ef0c3c625a5",
          "asset1_number": 50000000,
          "asset2_number": 0,
          "additional_data": {}
      }
  },
  "signatures": []
}
```

## Bilateral Transfer [Type 4]
Type 4 transactions swap tokens between two wallets, that is exchange one type of token with another. An example would be to transfer 10 NWRL tokens for 10 NUSDC tokens in return. This transaction has to be signed by both parties(wallet1 and wallet2) as both wallets have outflows.
### Parameters
- `<transfer_type>` - integer - 4 for bilateral transfer
- `<asset1_code>` - string - Token code to sent
- `<asset2_code>` - string - Leave blank
- `<wallet1>` - string - From wallet
- `<wallet2>` - string - To wallet
- `<asset1_number>` - string - Amount to send
- `<asset2_number>` - string - Leave 0
- `<additional_data>` - dictionary - Any arbitrary data about the transaction for reference by dapp.


### Example transaction
```json
{
  "transaction": {
      "timestamp": 1669379031000,
      "trans_code": "b3df7d8ac5b7caa2cb31695028c0d890c22c72b7",
      "type": 4,
      "currency": "NWRL",
      "fee": 1000000,
      "descr": "",
      "valid": 1,
      "block_index": 0,
      "specific_data": {
          "transfer_type": 5,
          "asset1_code": "NWRL",
          "asset2_code": "NUSDC",
          "wallet1": "0x1342e0ae1664734cbbe522030c7399d6003a07a8",
          "wallet2": "0xd6c038f5c25dae8a8f7350a58fb79ef0c3c625a5",
          "asset1_number": 1000000,
          "asset2_number": 1000000,
          "additional_data": {}
      }
  },
  "signatures": []
}
```

### Sample code for transferring tokens
The below python code does a type 4 or type 5 transfer based on the input provided.
```python

import json
import requests

NODE_URL = 'http://archive1-testnet1.newrl.net:8421'
WALLET = {"public": "CcGRdIzGC0ODmycwg8xWWBHCb1zlSxftS0oXxh561riA/HrDCBucDPKHVuohzlAXibWej5ED82aMzyyGEIYo7g==", "private": "1j58o3vNa0OmvfKBsvm03n5k8CfA90H/4SoQW/OVXsa=", "address": "0x667663f36ac08e78bbf259f1361f02dc7dad593b"}

transfer_type = input('Enter transfer type[4 for bilateral, 5 for unilateral]: ')
wallet1 = input('Enter first wallet address[leave blank for custodian]: ')
token1 = input('Enter first token code: ')
amount1 = input('Enter amount of first token: ')
wallet2 = input('Enter second wallet address: ')

if wallet1 == '':
  wallet1 = WALLET["address"]

if transfer_type == '4':
  token2 = input('Enter second token code: ')
  amount2 = input('Enter amount of second token: ')
else:
  token2 = ''
  amount2 = 0

add_transfer_request = {
  "transfer_type": int(transfer_type),
  "asset1_code": token1,
  "asset2_code": token2,
  "wallet1_address": wallet1,
  "wallet2_address": wallet2,
  "asset1_qty": int(amount1),
  "asset2_qty": int(amount2),
  "description": "",
  "additional_data": {}
}

print(add_transfer_request)

response = requests.post(NODE_URL + '/add-transfer', json=add_transfer_request)

unsigned_transaction = response.json()
unsigned_transaction['transaction']['fee'] = 1000000
response = requests.post(NODE_URL + '/sign-transaction', json={
    "wallet_data": WALLET,
    "transaction_data": unsigned_transaction
})
signed_transaction = response.json()

# Transfer type 4 need to be signed by both wallets
if transfer_type == '4':
  WALLET2 = input('Enter complete json for wallet2: ')
  WALLET2 = json.loads(WALLET2)

  response = requests.post(NODE_URL + '/sign-transaction', json={
    "wallet_data": WALLET2,
    "transaction_data": signed_transaction
  })
  signed_transaction = response.json()

print('signed_transaction', signed_transaction)
response = requests.post(NODE_URL + '/validate-transaction', json=signed_transaction)
print(response.text)
print(response.status_code)
assert response.status_code == 200

```