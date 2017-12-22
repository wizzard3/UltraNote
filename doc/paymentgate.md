## Payment Gate JSON RPC

Paymentgate daemon `walletd` provides multi-user wallets for services like exchages or online wallets.
Natural way to use it in your application is JSON RPC. Popular programming laguages provide libraries for making RPC calls seamlessly, but in this document we are going to show only raw JSON RPC requests using a barebones HTTP client. 


### Running the daemon

#### 1. Add `walletd` to any bin directory or set `PATH` variable. And check if it is working:
```sh
~$ walletd --help
```


#### 2. The daemon has plenty of programm options, but the best way is to use a configuration file.
```
~$ walletd --config=/etc/paymentgate.conf
```

##### Available configuration options:
```ini
container-file=				# REQUIRED. Path to wallet container
container-password= 		# REQUIRED. Password
data-dir=/srv/ultranote		# Where blockchain data is stored. Defaults to /home/user/.UltraNote
bind-address=0.0.0.0		# Use server IP or 127.0.0.1
bind-port=8070				# Port
rpc-user=                   # Auth user for RPC service
rpc-password=               # Password for RPC service
server-root=/srv/ultranote  # Working directory
log-level=                  # Log level 0-4
local=1                     # Use In-process node
daemon-address=localhost    # remote daemon address, don't use it with in-process mode
daemon-port=30000           # 
```

##### Minimal Configuration
Here is minimal cofiguration example required to run the daemon:
`/etc/paymentgate.conf`
```ini
container-file=/etc/ultranote/container.iwallet
bind-port=8070
bind-address=127.0.0.1
container-password=sectet123
log-file=/var/log/walletd.log
local=1
```

#### 3. Generate wallet container:
```sh
~$ walletd --config=/etc/paymentgate.conf --generate-container
```


#### 4. Running the daemon:
```
~$ walletd --config=/etc/paymentgate.conf
```
*To keep it warm use your favorite process manager like `systemd` or `supervisor`.
*Configure firewall rules if you run it in a container or virtual environment
*Do use a https proxy if you run it outside host environment  






### JSON RPC Methods

URL: `http://localhost:8070/json_rpc`






#### * `getStatus()`

Request:
```json
{
	"method":"getStatus", 
	"params": {},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{
		"blockCount":30375,
		"knownBlockCount":30375,
		"lastBlockHash":"ffa6dda0facc4a7ea903b0cededd897d2f281ac09eb6f50b9fed840dfe5073b5",
		"peerCount":22
	}
}
```

Use Case:
_Check if the daemon is syncronized:_
`peerCount` > 0
`knownBlockCount` == `blockCount`




#### * `createAddress()`
Create a new wallet

Request:
```json
{
	"method":"createAddress",
	"params": {},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{
		"address":"Xun3sfN5CzibcUqXqmqMFngzwaPS15iEySg2SVggQPArXzDVpJCHM5yhHPSR7X6nPG5DVpjrpNJ2Jg7Ej4DV3xgL5PEsCwEgBs"
	}
}
```

#### `deleteAddress`

Request:
```json
{
	"method":"deleteAddress",
	"params": {
		"address":"Xun3sfN5CzibcUqXqmqMFngzwaPS15iEySg2SVggQPArXzDVpJCHM5yhHPSR7X6nPG5DVpjrpNJ2Jg7Ej4DV3xgL5PEsCwEgBs"
	},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{}
}
```



#### * `getBalance()`

Request:
```json
{
	"method":"getBalance",
	"params": {
		"address":"Xun3sfN5CzibcUqXqmqMFngzwaPS15iEySg2SVggQPArXzDVpJCHM5yhHPSR7X6nPG5DVpjrpNJ2Jg7Ej4DV3xgL5PEsCwEgBs"
	},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{
		"availableBalance":0,
		"lockedAmount":0
	}
}
```



#### * `getUnconfirmedTransactionHashes()`

Request:
```json
{
	"method":"getUnconfirmedTransactionHashes",
	"params": {
		"address":"Xun3sfN5CzibcUqXqmqMFngzwaPS15iEySg2SVggQPArXzDVpJCHM5yhHPSR7X6nPG5DVpjrpNJ2Jg7Ej4DV3xgL5PEsCwEgBs"
	},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{
		"transactionHashes":[
			'7031309d4a0d87241c3700cafaeeefc04b226dc3387cedef4e93226aa76f21cb',
			'7031309d4a0d87241c3700cafaeeefc04b226dc3387cedef4e93226aa76f21cb' 
		]
	}
}
```

Use Case:
_Check if there are new transactions in the pool_




#### * `getTransactions()`

Request:
```json
{
	"method":"getTransactions",
	"params": {"firstBlockIndex": 53888, "blockCount": 5},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{
		"items":[
			{"blockHash":"ca9179c4e02017adcd538efb3874b1857b4121814c7d15ec15f8fc0a93481466","transactions":[{..}]},
			{"blockHash":"ca9179c4e02017adcd538efb3874b1857b4121814c7d15ec15f8fc0a93481466","transactions":[{..}]}
			...
		]
	}
}
```

Use Case:
_Get transactions from blockchain after the service was down_





#### * `getTransaction()`

Request:
```json
{
	"method":"getTransaction",
	"params": {
		"transactionHash":"9fb36d4bb905ef360c877ce8eba9c9dafc81da27cae85bd4727af46c5ba75ef7"
	},
	"jsonrpc": "2.0", 
	"id": "1"
}
```

Response:
```json
{
	"id":"1",
	"jsonrpc":"2.0",
	"result":{
		"transaction":{
			"amount":-1000,
			"blockIndex":4294967295,
			"extra":"015371f8a3cdae5b7ec5fd8e79e3f1d524f8f4d9d884d6c74b81d63e4076b04b1201",
			"fee":0,
			"isBase":false,
			"paymentId":"",
			"state":2,
			"timestamp":0,
			"transactionHash":"9fb36d4bb905ef360c877ce8eba9c9dafc81da27cae85bd4727af46c5ba75ef7",
			"transfers":[
				{
					"address":"Xun3qFybMCTcqzexD68QMjDoHUDUqUCWEJ82svTJ5vtbYF652s7o3njYe2AvyWtSL2iiiELby9mGH6dkQZryga4P4fVVUmGVMk",
					"amount":1000,
					"type":0
				},
				{
					"address":"Xun3ZtBPE7eYvz1Uokg9zg9m8UJsYdWFyEFT6Mmk4snXgMeaSfAQRGKhHPSR7X6nPG5DVpjrpNJ2Jg7Ej4DV3xgL5PEsCMBnGV",
					"amount":899000,
					"type":2
				},
				{
					"address":"Xun3ZtBPE7eYvz1Uokg9zg9m8UJsYdWFyEFT6Mmk4snXgMeaSfAQRGKhHPSR7X6nPG5DVpjrpNJ2Jg7Ej4DV3xgL5PEsCMBnGV",
					"amount":-900000,
					"type":0
				}
			],
		"unlockTime":0
		}
	}
}

```



#### `reset`
Re-sync cointainer starting from 0 block.


