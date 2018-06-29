# Cosmos Hub (Gaia) LCD API

This document describes the API that is exposed by the specific LCD implementation of the Cosmos
Hub (Gaia). Those APIs are exposed by a REST server and can easily be accessed over HTTP/WS(websocket)
connections.

The complete API is comprised of the sub-APIs of different modules. The modules in the Cosmos Hub
(Gaia) API are:

* ICS0 (TendermintAPI) - not yet implemented
* ICS1 (KeyAPI)
* ICS20 (TokenAPI)
* ICS21 (StakingAPI) - not yet implemented
* ICS22 (GovernanceAPI) - not yet implemented

Error messages my change and should be only used for display purposes. Error messages should not be
used for determining the error type.

## ICS0 - TendermintAPI - not yet implemented

Exposes the same functionality as the Tendermint RPC from a full node. It aims to have a very
similar API.

## ICS1 - KeyAPI

This API exposes all functionality needed for key creation, signing and management.

### /keys - GET

url: /keys

Functionality: Gets a list of all the keys.

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {
        "keys": [
           {
                "name": "monkey",
                "address": "cosmosaccaddr1fedh326uxqlxs8ph9ej7cf854gz7fd5zlym5pd",
                "pub_key": "cosmosaccpub1zcjduc3q8s8ha96ry4xc5xvjp9tr9w9p0e5lk5y0rpjs5epsfxs4wmf72x3shvus0t"
            },
            {
                "name": "test",
                "address": "cosmosaccaddr1thlqhjqw78zvcy0ua4ldj9gnazqzavyw4eske2",
                "pub_key": "cosmosaccpub1zcjduc3qyx6hlf825jcnj39adpkaxjer95q7yvy25yhfj3dmqy2ctev0rxmse9cuak"
            }
        ],
        "block_height": 5241
    }
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error":"Could not retrieve the keys.",
    "result":{}
}
```

### /keys/recover - POST

url: /keys/recover

Functionality: Recover your key from seed and persist it encrypted with the password.

Parameter:
| Parameter | Type   | Default | Required | Description      |
| --------- | ------ | ------- | -------- | ---------------- |
| name      | string | null    | true     | name of key      |
| password  | string | null    | true     | password of key  |
| seed      | string | null    | true     | seed of key      |

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {
        "address":BD607C37147656A507A5A521AA9446EB72B2C907
    }
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not recover the key.",
    "result": {}
}
```

### /keys/create - POST

url: /keys/create

Functionality: Create a new key.

Parameter:
| Parameter | Type   | Default | Required | Description      |
| --------- | ------ | ------- | -------- | ---------------- |
| name      | string | null    | true     | name of key      |
| password  | string | null    | true     | password of key  |

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {
        "seed":crime carpet recycle erase simple prepare moral dentist fee cause pitch trigger when velvet animal abandon
    }
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not create new key.",
    "result": {}
}
```

### /keys/{name} - GET

url: /keys/{name}

Functionality: Get the information for the specified key.

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {
        "name": "test",
            "address": "cosmosaccaddr1thlqhjqw78zvcy0ua4ldj9gnazqzavyw4eske2",
            "pub_key": "cosmosaccpub1zcjduc3qyx6hlf825jcnj39adpkaxjer95q7yvy25yhfj3dmqy2ctev0rxmse9cuak"
    }
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not find information on the specified key.",
    "result": {}
}
```

### /keys/{name} - PUT

url: /keys/{name}

Functionality: Change the encryption password for the specified key.

Parameters:
| Parameter       | Type   | Default | Required | Description     |
| --------------- | ------ | ------- | -------- | --------------- |
| old_password    | string | null    | true     | old password    |
| new_password    | string | null    | true     | new password    |

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {}
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not update the specified key.",
    "result": {}
}
```

### /keys/{name} - DELETE

url: /keys/{name}

Functionality: Delete the specified key.

Parameters:
| Parameter | Type   | Default | Required | Description      |
| --------- | ------ | ------- | -------- | ---------------- |
| password  | string | null    | true     | password of key  |

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {}
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not delete the specified key.",
    "result": {}
}
```

## ICS20 - TokenAPI

The TokenAPI exposes all functionality needed to query account balances and send transactions.

### /balance/{account} - GET

url: /balance/{account}

Functionality: Query the specified account.

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {
        [
            "atom": 1000,
            "photon": 500,
            "ether": 20
        ]
    }
}
```

Returns on error:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not find any balance for the specified account.",
    "result": {}
}
```

### /bank/create_transfer - POST

url: /bank/create_transfer

Functionality: Create a transfer in the bank module.

Parameters:
| Parameter    | Type   | Default | Required | Description               |
| ------------ | ------ | ------- | -------- | ------------------------- |
| sender       | string | null    | true     | Address of sender         |
| receiver     | string | null    | true     | address of receiver       |
| chain_id     | string | null    | true     | chain id                  |
| amount       | int    | null    | true     | amount of the token       |
| denomonation | string | null    | true     | denomonation of the token |

Returns on success:

```json
{
    "rest api": "2.0",
    "code":200,
    "error": "",
    "result": {
     "transaction": "TODO:<JSON sign bytes for the transaction>"
    }
}
```

Returns on failure:

```json
{
    "rest api": "2.0",
    "code":500,
    "error": "Could not create the transaction.",
    "result": {}
}
```

### /signed_transfer - POST

url: /signed_transfer, Method: POST

Functionality: transfer asset

Parameters:

| Parameter       | Type   | Default | Required | Description            |
| ------------    | ------ | ------- | -------- | ----------------------------------------------- |
| signed_transfer | []byte | null    | true     | bytes of a valid transaction and it's signature |

* The above command returns JSON structured like this if success:

```
{
    "error": "",
    "code":200,
    "result": {
     "tx_hash": ""
    },
    "rest api": "2.0"
}
```

* The above command returns JSON structured like this if fails:

```
{
    "error": "Invalid Signature",
    "code":500,
    "result": {},
    "rest api": "2.0"
}
```
## StakingAPI

This API exposes all functionality needed for staking info query.

### /stake/validators - GET

url: /stake/validators, Method: GET

Functionality: Get all validators' detailed information

Parameters: null

* The above command returns JSON structured like this if success:

```
{
    "error": "cannot verify the latest block",
    "code": 500,
    "result": [
        {
            "owner": "cosmosvaladdr1fedh326uxqlxs8ph9ej7cf854gz7fd5ze4wr05",
            "pub_key": "cosmosvalpub1zcjduc3qpp0k3kaxnk8pn5syrcltx5ndx6gml7kuxz62zvh87ga42f3tak0sglfqx5",
            "revoked": false,
            "pool_shares": {
                "status": 2,
                "amount": "100"
            },
            "delegator_shares": "0",
            "description": {
                "moniker": "monkey",
                "identity": "",
                "website": "",
                "details": ""
            },
            "bond_height": 0,
            "bond_intra_tx_counter": 0,
            "proposer_reward_pool": null,
            "commission": "0",
            "commission_max": "0",
            "commission_change_rate": "0",
            "commission_change_today": "0",
            "prev_bonded_shares": "0"
        }
    ],
    "rest api": "2.0"
}
```

* The above command returns JSON structured like this if fails:

```
{
    "error": "Encountered internal error",
    "code":500,
    "result": {},
    "rest api": "2.0"
}
```

### /stake/{delegator}/bonding_status/{validator} - GET

url: /stake/{delegator}/bonding_status/{validator}, Method: GET

Functionality: Get and verify the delegation informantion

Parameters:
```
delegator: cosmosaccaddr1fedh326uxqlxs8ph9ej7cf854gz7fd5zlym5pd
validator: cosmosvaladdr1fedh326uxqlxs8ph9ej7cf854gz7fd5ze4wr05
```
* The above command returns JSON structured like this if success:

```
{
    "error": "",
    "code":200,
    "result": {
    },
    "rest api": "2.0"
}
```

* The above command returns JSON structured like this if fails:

```
{
    "error": "invalid bech32 prefix. Expected cosmosaccaddr, Got cosmosvaladdr",
    "code":500,
    "result": {},
    "rest api": "2.0"
}
```