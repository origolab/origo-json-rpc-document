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

# Origo Private Transaction

## Get New Private Address

```shell
curl --data '{"jsonrpc":"2.0","method":"origo_getNewAddress","params":["password"],"id":1}' -H "Content-Type: application/json" -X POST localhost:6622
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
curl --data '{"jsonrpc":"2.0","method":"origo_listAddresses","id":1}' -H "Content-Type: application/json" -X POST localhost:6622
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
 -H "Content-Type: application/json" -X POST localhost:6622
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
curl --data '{"jsonrpc":"2.0","method":"origo_getBalance","params":["ogo127hk2tmx3pktg0pvdskrtjal5yt9en5zn67vm3tuxau5v5vvvl8p34phy0n4znfq7h4f5n6l2yw"],"id":1}' -H "Content-Type: application/json" -X POST localhost:6622
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
curl --data '{"jsonrpc":"2.0","method":"origo_listUnspent","params":["ogo1td987xe2lmhez8juecmnlk4mxfwe6m4jft8g20czh9kp5p4mfhqn8uf7cua3zh454p6uzc4z8td"],"id":1}' -H "Content-Type: application/json" -X POST localhost:6622
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
curl --data '{"jsonrpc":"2.0","method":"origo_sendMany", "params":["ogo1mj73lnwek33kppywv520yfnt58thshan8rfacfpum6hcr2ftwt4kn50kdmlm5r3js3pcvcmtxp8", [{"address":"ogo1fklhxyhg0c20yaqkfkwyf2khtn7e3nme97y28nhlvdexcvzqytzhtuz46admr44vas99wpqjfqt", "amount":"0xba43b7400", "memo":"test" }], "password"],"id":1}' -H "Content-Type: application/json" -X POST localhost:6622
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



