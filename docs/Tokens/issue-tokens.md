---
sidebar_position: 6
slug: /issue-tokens
---


# Issue Tokens

Token issuance is done using a type 2 transaction on Newrl. The wallet signing this transaction will be the custodian
for the token. Further issuances can be done using the same wallet.

#### Parameters

- `<token_name>` - String representing the name for the token
- `<token_code>` - String representing the code for the token. Should be unique.
- `<token_type>` - Defaults to "1" for now. This can be used for various token types later.
- `<first_owner>` - Wallet for crediting the newly created tokens.
- `<custodian>` - Wallet of the custodian or the signer.
- `<legal_doc>` - The hash of the supporting document. This can be used to legally bind the token to a real world agreement.
- `<amount_created>` - The amount of token to create.
- `<tokendecimal>` - Decimal points for the token. 0 if no decimal.
- `<disallowed_regions>` - Array for regions the token is not allowed to be used. Can be empty array - []
- `<is_smart_contract_token>` - Should be given false for non-smart contract tokens
- `<token_attributes>` - Dictionary representing any specific data for the token

#### Sample code for issuing tokens
```python

import requests


NODE_URL = 'http://143.198.134.94:8421'
WALLET = {
    "public": "f9a8e9773c706a6c32182144dd656409853b7eb25782ba61e5b9030ae19baf63fea3672464496e8ac4ac7046bedcbe7ae9f1d20481fcbceefc22afdfbf14ee27",
    "private": "a24555c80eadb8d71da21ff3f93a3412f80ba129cf880b644f00c0b61120b23b",
    "address": "0xce4b9b89efa5ee6c34655c8198c09494dc3d95bb"
}


token_code = input('Enter token code: ')
amount = input('Issue amount: ')
first_owner = input('First owner[leave blank for custodian]: ')

if first_owner == '':
  first_owner = WALLET['address']

add_wallet_request = {
  "token_name": token_code,
  "token_code": token_code,
  "token_type": "1",
  "first_owner": first_owner,
  "custodian": WALLET['address'],
  "legal_doc": "686f72957d4da564e405923d5ce8311b6567cedca434d252888cb566a5b4c401",
  "amount_created": amount,
  "tokendecimal": 2,
  "disallowed_regions": [],
  "is_smart_contract_token": False,
  "token_attributes": {}
}

response = requests.post(NODE_URL + '/add-token', json=add_wallet_request)

unsigned_transaction = response.json()

unsigned_transaction['transaction']['trans_code'] = unsigned_transaction['transaction']['trans_code'] + '1'
unsigned_transaction['transaction']['fee'] = 12

response = requests.post(NODE_URL + '/sign-transaction', json={
    "wallet_data": WALLET,
    "transaction_data": unsigned_transaction
})

signed_transaction = response.json()

print('signed_transaction', signed_transaction)
response = requests.post(NODE_URL + '/validate-transaction', json=signed_transaction)
print(response.text)
print(response.status_code)
assert response.status_code == 200

```