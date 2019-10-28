## Nervos Common Stratum Protocol
Nervos Common Stratum Protocol is standardized by UUPool, BeePool and F2Pool.
### mining.subscribe
* params: ["agent", null]
* result: [null, "nonce-prefix", "nonce2 size"]

```c
// request:
{
	"id": 1,
	"method": "mining.subscribe",
	"params": ["ckbminer-v1.0.0", null]
}

// response:
{
	"id": 1,
	"result": [null, "00c904bd", 12],
	"error": null
}
```

nonce-prefix is first part of the block header nonce (in hex).

By protocol, CKB's nonce is 16 bytes long. The miner will pick nonce2 such that len(nonce2) = 16 - len(nonce-prefix) = 16 - 4 = 12 bytes.

### mining.authorize
- params: ["username", "password"]
- result: true

```c
{
	"id": 2,
	"method": "mining.authorize",
	"params": ["ckb1qyq2znu0gempdahctxsm49sa9jdzq9vnka7qt9ntff", "x"]
}

{"id":2,"result":true,"error":null}
```

### mining.set_target

* params: ["32bytes target in hex"]

```c
{
	"id": null,
	"method": "mining.set_target",
	"params": ["0001000000000000000000000000000000000000000000000000000000000000"]
}
```

### mining.notify

* params: ["jobId", "header hash", height, "parent hash", cleanJob]

```c
{
	"id": null,
	"method": "mining.notify",
	"params": ["1611", "d5a74fba920ad0d35ec5726f26327547cbc82180e356e5ccf6cf2e6bd75f8a66", 1, "df4f27b8a8baae4672012c7a57b3abd684e516b5a2739cfec71d8d071e85fe12", true]
}
```

### mining.submit
* params: [ "username", "jobId", "nonce2" ]
* result: true / false

```c
{
	"id": 102,
	"method": "mining.submit",
	"params": ["ckb1qyq2znu0gempdahctxsm49sa9jdzq9vnka7qt9ntff.worker1", "1611", "000000000000000000114026"]
}

{"id":102,"result":true,"error":null}    // accepted share response
{"id":102,"result":false,"error":[21,"low difficulty",null]}  // rejected share response
```

nonce2 is the second part of the nonce.

```
In this example

nonce = nonce-prefix + nonce2 = 00c904bd000000000000000000114026 (16 bytes)

headerHash = d5a74fba920ad0d35ec5726f26327547cbc82180e356e5ccf6cf2e6bd75f8a66   (32 bytes without nonce)

headerHashWithNonce = headerHash + nonce = d5a74fba920ad0d35ec5726f26327547cbc82180e356e5ccf6cf2e6bd75f8a6600c904bd000000000000000000114026   (48 bytes with nonce)

powHash = eaglesonghash(headerHashWithNonce) = 000034965758f32a266558876abba7be3ea343086bf048c9fc3c854e77a2f360
```


