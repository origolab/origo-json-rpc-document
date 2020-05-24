---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://origo.network/'>Documentation Powered by Origo Foundation.</a>

search: true
---

# Introduction

Welcome to the Origo JSON-RPC API! You can use our API to access Origo API, which can get information about account, block, transaction in our BlockChain Network.

We have language bindings in Shell! You can view code examples in the dark area to the right.

# Private Transaction Methods

## Get New Private Address

```shell
curl --data '{"jsonrpc":"2.0","method":"origo_getNewAddress","params":["password"],"id":1}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
    "jsonrpc": "2.0",
    "result": "ogo1dj3lwla9h4axf8wpqkuazyuzv3m0j3pdxuvm0x36q3wgm2y0qskkkzp7anznjtpemggswkmuwe0",
}
```

Return a new private address for sending and receiving payments. The address will be added to the node’s wallet.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
password | String | The passord is used to encrypt the private address in the node's wallet.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
address | String | The new private address.

## List Private Addresses

```shell
curl --data '{"jsonrpc":"2.0","method":"origo_listAddresses","id":1}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
    "jsonrpc": "2.0",
    "result": [
                "ogo1dj3lwla9h4axf8wpqkuazyuzv3m0j3pdxuvm0x36q3wgm2y0qskkkzp7anznjtpemggswkmuwe0",
                "ogo127hk2tmx3pktg0pvdskrtjal5yt9en5zn67vm3tuxau5v5vvvl8p34phy0n4znfq7h4f5n6l2yw"
              ],
}
```

Return all private addresses in the node's wallet.

### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Array | A list of private addresses.


## Send Public to Private Transaction

```shell
curl --data '{"method":"personal_sendShieldTransaction","params":[{ "from":"0x00a329c0648769a73afac7f9381e08fb43dbea72","gas": "0x76c00", "gasPrice": "0x9184e72a000", "value": "0x174876e800", 
"shieldAmounts": [{"address":"ogo180m058urhazk8j98zvz9fsq5zd0vd9dpsc8c6ednwd2xkc3l8z9thmxsezepzx4aascp6nrlkd6","amount": "0x174876e800", "memo":"test" }] }, ""] ,"id":1,"jsonrpc":"2.0"}'
 -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x62e05075829655752e146a129a044ad72e95ce33e48ff48118b697e15e7b41e4"
}
```

Send transaction from public address to private address and signs it in a single call. 
The account does not need to be unlocked to make this call, and will not be left unlocked after.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
1 | Object | The Shield Transaction Object.
2 | String | Passphrase to unlock the from account.

### The Shield Transaction Object
Parameter | Type | Description
--------- | ------- | -----------
from | Address | The address the transaction is send from.
to | Address | (optional) The address the transaction is directed to.
gas | Quantity | Integer of the gas provided for the transaction execution.
gasPrice | Quantity | Integer of the gas price used for each paid gas.
value | Quantity | Integer of the value sent with this transaction, the value should be the multiple of 10^9.
data | Data | (optional) 4 byte hash of the method signature followed by encoded parameters. For details see Ethereum Contract ABI.
nonce | Quantity | (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
condition | Object | (optional) Conditional submission of the transaction. Can be either an integer block number or UTC timestamp (in seconds) or null.
shield_amouts | List | private amount info in shield transaction.

### ShieldAmounts Parameters

Parameter | Type | Description
--------- | ------- | -----------
address | String | private address the transaction is directed to.
amount | Quantity | the numeric amount to the address.
memo | String | (optional) raw data represented in hexadecimal string.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
Data | 32 Bytes | the transaction hash, or the zero hash if the transaction is not yet available.


## Get Balance For Private Address

```shell
curl --data '{"jsonrpc":"2.0","method":"origo_getBalance","params":["ogo127hk2tmx3pktg0pvdskrtjal5yt9en5zn67vm3tuxau5v5vvvl8p34phy0n4znfq7h4f5n6l2yw"],"id":1}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
    "jsonrpc":"2.0",
    "result":"0x174876e800",
    "id":1
}
```

Returns the balance of a private address belonging to the node’s wallet.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
address | String | The selected address. It may be a transparent or private address.
miniconf | Quantity | (optional) default = 1, Only include transactions confirmed at least this many times.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Quantity | The total amount balance received for this address



## List Unspent Note For Private Address

```shell
curl --data '{"jsonrpc":"2.0","method":"origo_listUnspent","params":["ogo1td987xe2lmhez8juecmnlk4mxfwe6m4jft8g20czh9kp5p4mfhqn8uf7cua3zh454p6uzc4z8td"],"id":1}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
    "jsonrpc":"2.0",
    "result":[
              {
                "address":"ogo1td987xe2lmhez8juecmnlk4mxfwe6m4jft8g20czh9kp5p4mfhqn8uf7cua3zh454p6uzc4z8td",
                "amount":"0x174876e800",
                "change":false,
                "confirmations":"0x0",
                "jsindex":"0x0",
                "jsoutindex":"0x0",
                "memo":"test",
                "outindex":"0x0",
                "spendable":true,
                "txid":"0x348c…89cf"
               }
              ],
    "id":1
}
```

Returns array of unspent shielded notes with between minconf and maxconf (inclusive) confirmations. 
Optionally filter to only include notes sent to specified addresses.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
address | String | The private address to filter on.
miniconf | Quantity |  (optional) default = 1, The minimum confirmations to filter.
maxconf | Quantity |  (optional) default = 9999999, The maximum confirmations to filter.
include_watch_only | bool | (optional) default = false, whether include watchonly addresses.

### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Array | The unspent notes for the private address.

### Unspent Notes

Parameter | Type | Description
--------- | ------- | -----------
address | String | The private address to filter on.
amout | Quantity | amount of value in the note, it is multiple of 10^9.
change | Bool | true if the address received the note is also in the sending addresses.
confirmations | Quantity | number of confirmations, default is 0.
jsindex | Quantity | joinsplit index.
jsoutindex | Quantity | output index of the joinsplit.
memo | String | raw data represented in hexadecimal string for this note.
outindex | Quantity | output index of the transaction for this note.
spendable | Bool | True if note can be spent by wallet.
txid | 32 Bytes | the transaction hash include this note.


## Send Transaction From Private Address to Private Address

```shell
curl --data '{"jsonrpc":"2.0","method":"origo_sendMany", "params":["ogo1mj73lnwek33kppywv520yfnt58thshan8rfacfpum6hcr2ftwt4kn50kdmlm5r3js3pcvcmtxp8", [{"address":"ogo1fklhxyhg0c20yaqkfkwyf2khtn7e3nme97y28nhlvdexcvzqytzhtuz46admr44vas99wpqjfqt", "amount":"0xba43b7400", "memo":"test" }], "password"],"id":1}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
    "jsonrpc":"2.0",
    "result": "0x01458993fd9414eaad5b4bee990d5117aa0080f35c51906837fe1c0365d5af84",
    "id":1
}
```

Transfer value from private address to private address. while change generated from private address returns to itself.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
from | String | the from private address to transfer balance.
password | Quantity | The passord is used to decrypt the from private address in the node's wallet.
miniconf | Quantity | (optional) default = 0, Only use funds confirmed at least this many times.
gas | Quantity | (optional), default=21000, The gas limit of the transaction.
gasPrice | Quantity | (optional), default=1, The gas price of the transaction.
Amounts | Array | An array of json objects representing the amounts to send.

### Amounts Parameters

Parameter | Type | Description
--------- | ------- | -----------
address | String | the private address to send.
Amount | Quantity | The value to send to the address，it should be multiple of 10^9.
Memo | String | raw data represented in hexadecimal string format.



# Public Account Methods
## Create Public Account From Phrase

```shell
curl --data '{"method":"parity_newAccountFromPhrase","params":["stylus outing overhand dime radial seducing harmless uselessly evasive tastiness eradicate imperfect","hunter2"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
}
```

Creates a new account from a recovery phrase.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
1 | String | Recovery phrase.
2 | String | Password



### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Address | The created public address.

## Create Public Account From Secret

```shell
curl --data '{"method":"parity_newAccountFromSecret","params":["0x1db2c0cf57505d0f4a3d589414f0a0025ca97421d2cd596a9486bc7e2cd2bf8b","hunter2"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
}
```

Creates a new account from a private ethstore secret key.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
1 | Data | Secret, 32-byte hex.
2 | String | Password


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Address | The created public address.

## Remove Public Address 

```shell
curl --data '{"method":"parity_removeAddress","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": true
}
```

Removes an address from the addressbook.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
1 | Data | Secret, 32-byte hex.
2 | String | Password


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Boolean | true if the call was successful.


## Test Password For Account

```shell
curl --data '{"method":"parity_removeAddress","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": true
}
```

Removes an address from the addressbook.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
1 | Data | Secret, 32-byte hex.
2 | String | Password


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Boolean | true if the call was successful.


## List Public Accounts 

```shell
curl --data '{"method":"parity_listGethAccounts","params":[],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
}
```

Returns a list of the accounts available from Geth.

### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Array | 20 Bytes addresses owned by the client.



More methods for public account can refer <a href='https://wiki.parity.io/JSONRPC-parity_accounts-module'>parity_accounts module</a>


# Personal Related Methods

## Create New Account

```shell
curl --data '{"method":"personal_newAccount","params":["hunter2"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x8f0227d45853a50eefd48dd4fec25d5b3fd2295e"
}
```

Creates new account.

### Query Parameters
Parameter | Type | Description
--------- | ------- | -----------
1 | String | Password for the new account.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Address | 20 Bytes - The identifier of the new account.


## List All Stored Public Accounts

```shell
curl --data '{"method":"personal_listAccounts","params":[],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": [
    "0x7bf87721a96849d168de02fd6ea5986a3a147383",
    "0xca807a90fd64deed760fb98bf0869b475c469348"
  ]
}
```

Lists all stored accounts.

### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Array | A list of 20 byte account identifiers.


## Send Public to Public Transaction

```shell
curl --data '{"method":"personal_sendTransaction","params":[{"from":"0x407d73d8a49eeb85d32cf465507dd71d507100c1",
"to":"0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b","data":"0x41cd5add4fd13aedd64521e363ea279923575ff39718065d38bd46f0e6632e8e","value":"0x186a0"},"hunter2"],
"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x62e05075829655752e146a129a044ad72e95ce33e48ff48118b697e15e7b41e4"
}
```

Creates new account.

### Query Parameters
Parameter | Type | Description
--------- | ------- | -----------
1 | Object | The transaction object.
2 | String | Passphrase to unlock the from account.


### The Transaction Object
Parameter | Type | Description
--------- | ------- | -----------
from | Address | The address the transaction is send from.
to | Address | (optional) The address the transaction is directed to.
gas | Quantity | (optional) Integer of the gas provided for the transaction execution. eth_call consumes zero gas, but this parameter may be needed by some executions.
gasPrice | Quantity | Integer of the gas price used for each paid gas.
value | Quantity | Integer of the value sent with this transaction, the value should be the multiple of 10^9.
data | Data | (optional) 4 byte hash of the method signature followed by encoded parameters. For details see Ethereum Contract ABI.
nonce | Quantity | (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
condition | Object | (optional) Conditional submission of the transaction. Can be either an integer block number or UTC timestamp (in seconds) or null.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Data | 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available

More methods for personal module can refer <a href='https://wiki.parity.io/JSONRPC-personal-module'>personal module</a>



# BlockChain Related Methods

## Get Balance For Public Address

```shell
curl --data '{"method":"eth_getBalance","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58"
}
```

Returns the balance of the account of given address.

### Query Parameters
Parameter | Type | Description
--------- | ------- | -----------
1 | Address | 20 Bytes - address to check for balance.
2 | Quantity or Tag |  (optional) integer block number, or the string 'latest', 'earliest' or 'pending'.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Quantity | integer of the current balance in wei.

## Get Block By Hash

```shell
curl --data '{"method":"eth_getBlockByHash","params":["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",true],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": {
    "number": "0x1b4", // 436
    "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
    "parentHash": "0x9646252be9520f6e71339a8df9c55e4d7619deeb018d2a3f2d21fc165dde5eb5",
    "sealFields": [
      "0xe04d296d2460cfb8472af2c5fd05b5a214109c25688d3704aed5484f9a7792f2",
      "0x0000000000000042"
    ],
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "logsBloom": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
    "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
    "miner": "0x4e65fda2159562a496f9f3522f89122a3088497a",
    "difficulty": "0x27f07", // 163591
    "totalDifficulty": "0x27f07", // 163591
    "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "size": "0x27f07", // 163591
    "gasLimit": "0x9f759", // 653145
    "minGasPrice": "0x9f759", // 653145
    "gasUsed": "0x9f759", // 653145
    "timestamp": "0x54e34e8e", // 1424182926
    "transactions": [{ ... }, { ... }, ...],
    "uncles": [
      "0x1606e5...",
      "0xd5145a9..."
    ]
  }
}
```

Returns information about a block by hash.

### Query Parameters
Parameter | Type | Description
--------- | ------- | -----------
1 | Hash | Hash of a block.
2 | Boolean|  If true it returns the full transaction objects, if false only the hashes of the transactions.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
number| Quantity |The block number. null when its pending block
hash |Hash  | 32 Bytes - hash of the block. null when its pending block
parentHash | Hash  |32 Bytes - hash of the parent block
nonce | Data | 8 Bytes - hash of the generated proof-of-work. null when its pending block. Missing in case of PoA.
sha3Uncles |Data  | 32 Bytes - SHA3 of the uncles data in the block
logsBloom | Data | 256 Bytes - the bloom filter for the logs of the block. null when its pending block
transactionsRoot | Data  | 32 Bytes - the root of the transaction trie of the block
stateRoot | Data  |32 Bytes - the root of the final state trie of the block
receiptsRoot |Data  |32 Bytes - the root of the receipts trie of the block
author | Address  | 20 Bytes - the address of the author of the block (the beneficiary to whom the mining rewards were given)
miner |Address  | 20 Bytes - alias of ‘author’
difficulty |Quantity | integer of the difficulty for this block
totalDifficulty | Quantity  | integer of the total difficulty of the chain until this block
extraData | Data  | the ‘extra data’ field of this block
size |Quantity  |integer the size of this block in bytes
gasLimit |Quantity  |the maximum gas allowed in this block
gasUsed | Quantity | the total used gas by all transactions in this block
timestamp | Quantity  | the unix timestamp for when the block was collated
transactions | Array  |Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter
uncles |Array | Array of uncle hashes


## Get Block by Number

```shell
curl --data '{"method":"eth_getBlockByNumber","params":["0x1b4",true],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST  https://rpc.origo.network 
```

> The above command returns JSON structured like this:

```json
{
   "jsonrpc":"2.0",
   "result":{
      "author":"0x1dcb045967571fba8e876881bcfcc3ad94de1c6f",
      "difficulty":"0x463",
      "extraData":"0xd3820500854f7269676f86312e33382e30826c69",
      "gasLimit":"0x53998bab641fc",
      "gasUsed":"0x0",
      "hash":"0x929a04a738ac9a959e309446b7ae1f0d7763513effd0ae0b98b4b22afd30fe81",
      "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "miner":"0x1dcb045967571fba8e876881bcfcc3ad94de1c6f",
      "nonce":"0x0004a5277b3728f628983ae0f267b221677d2bf800000000011f8aa195000000",
      "number":"0x1b4",
      "parentHash":"0xcdcc21b02d5a9f78ca2efc2a3a3152ec344ff661b3f24741e51654c45ff64de8",
      "receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      "sealFields":[
         "0xa00004a5277b3728f628983ae0f267b221677d2bf800000000011f8aa195000000",
         "0xb9019004456ea9414ba0b71237d59946de999c472b8f46e9cffc2be12277f4145ffa2a857f3ced544275497c92824ef1d89faa8f6e0760f5676a7ec5b45e32d9bc472f4bcd69f9d661d8bb9364df157549355d778a6409346755737818c6321a037aae21dc2f4a0659bfe8e765da1dc6555d88210e59323e7d3107d49d88eaf714ba00aae36c50517bac254cd56d9bb4e3eb16265059c708850db576d00cf18809fad6c5be92ed286eaa85fedf73558c8d4a2e31436e80306c2ee45c7597a41a716f46e75e9f0b1f7f8fc70c08842ad20976cce55ea1c941afc00f36eed886705b07862f2da5ad1dee25b7dcddff54b098d7854fa31cc2cffb3bee7bf40ed21f94874de553d61ac9c1a955c31f9993d6a01553ee67922d9b296f5f959a0bef9459ebb53a9656ad1bd55746aab1e04f14631c6a7ed19b4a52727d1973d833c70f9bf52bf9170845e7300fc06dabeee9db543c746eb88062b70fe8c2845063d588df61ec56f29b68ea5d83b94e9127d213f72badcec3fd55b00a0a651f464c216c25cd31373c8f076b5046f2ef33a7898ff0176a"
      ],
      "sha3Uncles":"0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
      "size":"0x3a2",
      "solution":"0x04456ea9414ba0b71237d59946de999c472b8f46e9cffc2be12277f4145ffa2a857f3ced544275497c92824ef1d89faa8f6e0760f5676a7ec5b45e32d9bc472f4bcd69f9d661d8bb9364df157549355d778a6409346755737818c6321a037aae21dc2f4a0659bfe8e765da1dc6555d88210e59323e7d3107d49d88eaf714ba00aae36c50517bac254cd56d9bb4e3eb16265059c708850db576d00cf18809fad6c5be92ed286eaa85fedf73558c8d4a2e31436e80306c2ee45c7597a41a716f46e75e9f0b1f7f8fc70c08842ad20976cce55ea1c941afc00f36eed886705b07862f2da5ad1dee25b7dcddff54b098d7854fa31cc2cffb3bee7bf40ed21f94874de553d61ac9c1a955c31f9993d6a01553ee67922d9b296f5f959a0bef9459ebb53a9656ad1bd55746aab1e04f14631c6a7ed19b4a52727d1973d833c70f9bf52bf9170845e7300fc06dabeee9db543c746eb88062b70fe8c2845063d588df61ec56f29b68ea5d83b94e9127d213f72badcec3fd55b00a0a651f464c216c25cd31373c8f076b5046f2ef33a7898ff0176a",
      "stateRoot":"0x8e0297ac0f52dec350be3f1c0a13813e56939363927ba43c15310476f35a28e6",
      "timestamp":"0x5e275854",
      "totalDifficulty":"0x6ecfa",
      "transactions":[

      ],
      "transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      "uncles":[

      ]
   },
   "id":1
}
```

Returns information about a block by block number.

### Query Parameters
Parameter | Type | Description
--------- | ------- | -----------
1 | Quantity or Tag | Integer of a block number, or the string 'earliest', 'latest' or 'pending'.
2 | Boolean|  If true it returns the full transaction objects, if false only the hashes of the transactions.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
number| Quantity |The block number. null when its pending block
hash |Hash  | 32 Bytes - hash of the block. null when its pending block
parentHash | Hash  |32 Bytes - hash of the parent block
nonce | Data | 8 Bytes - hash of the generated proof-of-work. null when its pending block. Missing in case of PoA.
sha3Uncles |Data  | 32 Bytes - SHA3 of the uncles data in the block
logsBloom | Data | 256 Bytes - the bloom filter for the logs of the block. null when its pending block
transactionsRoot | Data  | 32 Bytes - the root of the transaction trie of the block
stateRoot | Data  |32 Bytes - the root of the final state trie of the block
receiptsRoot |Data  |32 Bytes - the root of the receipts trie of the block
author | Address  | 20 Bytes - the address of the author of the block (the beneficiary to whom the mining rewards were given)
miner |Address  | 20 Bytes - alias of ‘author’
difficulty |Quantity | integer of the difficulty for this block
totalDifficulty | Quantity  | integer of the total difficulty of the chain until this block
extraData | Data  | the ‘extra data’ field of this block
size |Quantity  |integer the size of this block in bytes
gasLimit |Quantity  |the maximum gas allowed in this block
gasUsed | Quantity | the total used gas by all transactions in this block
timestamp | Quantity  | the unix timestamp for when the block was collated
transactions | Array  |Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter
uncles |Array | Array of uncle hashes

## Get Last Block Number

```shell
curl --data '{"method":"eth_blockNumber","params":[],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST https://rpc.origo.network
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": "0x4b7" // 1207
}
```

Returns the number of most recent block.

### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
result | Quantity | integer of the current block number the client is on.


## Get Transaction by Hash

```shell
curl --data '{"method":"eth_getTransactionByHash","params":["0xa45a148967056c9c62cb07846ee6f3bd3efcfcaf2cea0ae40c786d7beec3909d"],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST  https://rpc.origo.network 
```

> The above command returns JSON structured like this:

```json
{
   "jsonrpc":"2.0",
   "result":{
      "balancingValue":null,
      "blockHash":"0x180f2e8990c9c0e68f249d8e369dc8f0be7b7b47e59732bf17448c241396bd22",
      "blockNumber":"0xca8de",
      "chainId":"0x1a",
      "condition":null,
      "creates":null,
      "from":"0x5dfbb5d46fc208735cf5c0a4a61a37dbc2967986",
      "gas":"0x5208",
      "gasPrice":"0x59682f000",
      "hash":"0xa45a148967056c9c62cb07846ee6f3bd3efcfcaf2cea0ae40c786d7beec3909d",
      "input":"0x",
      "isPrivate":false,
      "nonce":"0x7",
      "publicKey":"0xd6b543a25e925b876d69b1dc3f5cb4652d4511700d705d4d719e83747e05eed5b0b7b249b8ffadfd530c4015bffb93f15d492278c1797f3eb29d0534088b1e87",
      "r":"0xf192adf62b88aa4fe8e7ac5cf7a4dc096b6a7c4863d12346b29a302d50c1e6f7",
      "raw":"0xf86e0785059682f000825208948669c63d545b647ef76ec989b1cd9525068b09e98a74d71ebe7143601539008058a0f192adf62b88aa4fe8e7ac5cf7a4dc096b6a7c4863d12346b29a302d50c1e6f7a0104ca088883b234f826aeb4f3de72021c7a92e83d453676fe3983b975d707201",
      "s":"0x104ca088883b234f826aeb4f3de72021c7a92e83d453676fe3983b975d707201",
      "standardV":"0x1",
      "to":"0x8669c63d545b647ef76ec989b1cd9525068b09e9",
      "transactionIndex":"0x0",
      "v":"0x58",
      "value":"0x74d71ebe714360153900"
   },
   "id":1
}
```

Returns information about a block by block number.

### Query Parameters
Parameter | Type | Description
--------- | ------- | -----------
1 | Hash | 32 Bytes - hash of a transaction.


### Response Parameters

Parameter | Type | Description
--------- | ------- | -----------
Object | A transaction object, or null when no transaction was found |
hash | Hash | 32 Bytes | hash of the transaction.
nonce | Quantity | the number of transactions made by the sender prior to this one.
blockHash | Hash | 32 Bytes | hash of the block where this transaction was in. null when its pending.
blockNumber | Quantity or Tag | block number where this transaction was in. null when its pending.
transactionIndex | Quantity | integer of the transactions index position in the block. null when its pending.
from | Address | 20 Bytes | address of the sender.
to | Address | 20 Bytes | address of the receiver. null when its a contract creation transaction.
value | Quantity | value transferred in Wei.
gasPrice | Quantity | gas price provided by the sender in Wei.
gas | Quantity | gas provided by the sender.
input | Data | the data send along with the transaction.
v | Quantity | the standardised V field of the signature.
standardV | Quantity | the standardised V field of the signature (0 or 1).
r | Quantity | the R field of the signature.
raw | Data | raw transaction data
publicKey | Hash | public key of the signer.
chainId | Quantity | the chain id of the transaction, if any.
creates | Hash | creates contract hash
condition | Object | (optional) conditional submission, Block number in block or timestamp in time or null.


More methods for blockchain info can refer <a href='https://wiki.parity.io/JSONRPC-eth-module'>BlockChain module</a>

