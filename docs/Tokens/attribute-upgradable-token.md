---
sidebar_position: 6
slug: /token-transfers
---


# Token Transfers

Newrl now supports the update of token attributes.

1. A token attribute can be updated only if the token was created with  ```"editable": True ``` in its token attributes .
2. By default or if editable attribute is not set, it is considered non-editable.

## Creating a token that can have editable attributes
The transaction pattern is same as any other token creation (type-2) with only ```"editable": True ``` additionaly. 
### Parameters
- `<transfer_type>` - integer 5
- `<asset1_code>` - string - Token code to sent
- `<asset2_code>` - string - Leave blank
- `<wallet1>` - string - From wallet
- `<wallet2>` - string - To wallet
- `<asset1_number>` - string - Amount to send
- `<asset2_number>` - string - Leave 0
- `<additional_data>` - dictionary - Any arbitrary data about the transaction for reference by dapp.


### Example transaction to create token with editable attributes
```json
{
  "token_name": "sampleToken",
  "token_code": "sampleToken",
  "token_type": 1,
  "first_owner" :"0x52c3a0758644133fbbf85377244a35d932443bf0",
  "custodian" : "0x52c3a0758644133fbbf85377244a35d932443bf0",
  "legal_doc" : "test-net-doc",
  "amount_created" : 5,
  "tokendecimal" : 0,
  "disallowed_regions" : [],
  "is_smart_contract_token" : false,
  "token_attributes" : {
     "tat1":"tat1value",
     "editable":true
  }
}

```

## Updating a token attribute

A token attribute update transaction is same as token creation transaction (type 2), only addition is that we pass ```"token_update":True``` in specific data and the updated attributes itslef.

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
  "token_name": "sampleToken",
  "token_code": "sampleToken",
  "token_type": 1,
  "first_owner" :"0x52c3a0758644133fbbf85377244a35d932443bf0",
  "custodian" : "0x52c3a0758644133fbbf85377244a35d932443bf0",
  "legal_doc" : "test-net-doc",
  "amount_created" : 5,
  "tokendecimal" : 0,
  "disallowed_regions" : [],
  "is_smart_contract_token" : false,
  "token_attributes" : {
     "tat1":"tat1valueUpdated",
     "editable":true
  },
  "token_update":true
}
```
