---
sidebar_position: 5
slug: /smart-contracts/creation
---

# Verification Smart Contract



What is the Verification Contract?
Our Verification Contract revolutionizes the way users interact with the blockchain. It empowers individuals to securely add their claims to the blockchain, while others in the community can collectively verify the authenticity of each claim. This groundbreaking contract promotes trust and accountability, ensuring a transparent and reliable platform for all.

Key Advantages:
- Unparalleled Security: You can confidently add your claims to the blockchain, knowing that they are safeguarded by the blockchain's immutable nature and cryptographic protocols.
- Community Verification: Embrace the power of decentralization! Let the collective wisdom of the community verify the validity of claims, fostering a robust and trustworthy ecosystem.
- Thoroughly Tested: We take reliability seriously. Our team has meticulously crafted and rigorously tested the Verification Contract, guaranteeing a seamless and dependable user experience.

## Using verification contract:

Below represents transaction version for creating a new verification:

### Add verification 
Make a raw transaction to call verification smart contract using API mentioned below using curl. Please replace signers, asset if, verifiers address, doc_hash and link list, remarks and deletable after.
```
curl --location 'localhost:4018/call-sc' \
--header 'Content-Type: application/json' \
--data '{
   "sc_address":"ct2f2965d549732903dc3ad63a527b53bf2fa84f00",
   "function_called":"add_verification",
   "signers":[
      "0xd831a46f5dd3db77a27de82e14b70fc34695fa88"
   ],
   "params":{
      "asset_id":"sample_id",
      "asset_type":"token",
      "verifier_address":"0xd831a46f5dd3db77a27de82e14b70fc34695fa88",
      "outcome":1,
      "doc_hash_list":[
         "hash1"
      ],
      "doc_link_list":[
         "link1"
      ],
      "remarks":"remarks",
      "deletable_after":1347344
   }
}'
```
Use wallet.newrl.net to sign and submit your add verification transaction (Obtained from above response) onto chain. 

To get your verification data state , use get-sc-state api as mentioned below

```
curl -X 'GET' \
  'http://localhost:4018/sc-state?table_name=verification&contract_address=ct2f2965d549732903dc3ad63a527b53bf2fa84f00&unique_column=asset_id&unique_value=sample_asset_id' \
  -H 'accept: application/json'
```
