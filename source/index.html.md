---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://origo.network/'>Documentation Powered by Origo Foundation.</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Origo JSON-RPC API! You can use our API to access Origo API, which can get information about account, block, transaction in our BlockChain Network.

We have language bindings in Shell! You can view code examples in the dark area to the right.

# Origo Private Transaction

## Get New Address

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
address | String | The new private address.

