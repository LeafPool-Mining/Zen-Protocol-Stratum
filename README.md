# Zen-Protocol Leafpool's Stratum Mining Specification

### 1.miner->pool - Miner subscribe

```
{"id":100,"method":"mining.subscribe","params":["ccminer/3.4.0"]}

```

Reply

```
{"id":100,"result":[null,"01000000", 4],"error":null}
```
"01000000" is your pool nonce prefix



### 2.miner->pool - Miner login

```
{"id":101,"method":"mining.authorize","params":["zp_address.1","x"]}
```
Reply
```
{"id":101,"result":true,"error":null}
```

### 3.pool->miner - New job notification

```
{"id":null,"method":"mining.notify","params":["1","block_header","3eb43a5d",true]}

```

Params is in the following order
1. job_id 
2. block header (100 bytes)
3. nbits
4. clean job flag

### 4.pool->miner - Difficulty adjustment

```
{"id":null,"method":"mining.set_difficulty","params":[8192]}
```

### 5.miner->pool Share submission

```
{"id":129,"method":"mining.submit","params":["zp_address.worker","40","5d2fe35c","0","01000000"]}

```

Params is in the following order
1. ZP_ADDRESS.workerid
2. JobID
3. Miner nonce that is appended to pool nonce prefix
4. ntimes (Currently ignored)
5. Pool nonce prefix

Pool Reply
```
{"id":101,"result":true,"error":null}
```

You can check the implementation of the stratum on this miner https://github.com/ZenProtocol-Pool/ccminer-zp
