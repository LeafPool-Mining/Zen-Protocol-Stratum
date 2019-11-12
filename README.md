# Zen-Protocol Leafpool's Stratum Mining Specification


### mining.subscribe
* params: ["agent", null]
* result: [null, "nonce-prefix", "nonce2 size"]

```c
// request:
{
	"id": 1,
	"method": "mining.subscribe",
	"params": ["ccminer/3.4.0", null]
}

// response:
{
	"id": 1,
	"result": [null, "00ccddee", 4],
	"error": null
}
```

nonce-prefix (in hex) is used by the pool and is the first part of the block header nonce .

By protocol, ZP's nonce is 8 bytes long. The miner will pick nonce2 such that len(nonce2) = 8 - len(nonce-prefix) = 8 - 4 = 4 bytes.


### mining.authorize
- params: ["username", "password"]
- result: true

```c
// request:
{
	"id": 2,
	"method": "mining.authorize",
	"params": ["zp_address.worker", "x"]
}

// response:
{"id":2,"result":true,"error":null}
```


### mining.set_difficulty

* params: [difficulty]

```c
{
	"id": null,
	"method": "mining.set_difficulty",
	"params": [8192]
}
```

### mining.notify

* params: ["jobId", "block_header", "nbits", cleanJob]

```c
{
	"id": null,
	"method": "mining.notify",
	"params": ["1", "0000000000000000000028452edf193d9cac4b78dcd34d547214d2235fc7978907c0398200003c926c84ebdcbee4641b5ea9", "3eb43a5d", true]
}
```

### mining.submit
* params: [ "username", "jobId", "nonce2" ]
* result: true / false

```c
{
	"id": 3,
	"method": "mining.submit",
	"params": ["zp_address.worker", "69", "c001b33f"]
}

{"id":69,"result":true,"error":null}    // accepted share response
{"id":69,"result":false,"error":[23,"low difficulty share",null]}  // rejected share response
```

nonce2 is appended on nonce-prefix given on mining.subscribe.

```
Example

block_nonce = nonce-prefix + nonce2 = 00ccddeec001b33f (8 bytes)

block_header = 0000000000000000000028452edf193d9cac4b78dcd34d547214d2235fc7978907c0398200003c926c8400ccddee00000000  (100 bytes - nonce-prefix is already in place on byte 93-96. Nonce2 should be placed on byte 97-100)

block_header_with_nonce = block_header + nonce(on byte 97-100) = 0000000000000000000028452edf193d9cac4b78dcd34d547214d2235fc7978907c0398200003c926c8400ccddeec001b33f

block_hash = sha3(block_header_with_nonce) = 422f20e4ed54d4d7db61a6bfe04d03e04955d2bb97ef126c71a9cebfbc5cfab5

```
You can check the implementation of the stratum on this miner https://github.com/ZenProtocol-Pool/ccminer-zp
