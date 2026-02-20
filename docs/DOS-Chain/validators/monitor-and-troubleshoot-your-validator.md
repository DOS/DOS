# Monitor and troubleshoot your validator

### Resource <a href="#qbe89" id="qbe89"></a>

* https://docs.avax.network/apis/avalanchego/apis/info
* https://docs.avax.network/apis/avalanchego/apis/health

### &#x20;<a href="#id-8hete" id="id-8hete"></a>

## Backup And Restore

[https://docs.avax.network/nodes/maintain/node-backup-and-restore](https://docs.avax.network/nodes/maintain/node-backup-and-restore)

{% embed url="https://docs.avax.network/nodes/maintain/node-backup-and-restore" %}

### How to check avalanchego logs? <a href="#id-8hete" id="id-8hete"></a>

tail .avalanchego/logs/main.log

### How to check the subnet successfully deployed?  <a href="#jayq1" id="jayq1"></a>

if the logs file contain these lines:

\[04-17|15:50:45.332] INFO server/server.go:269 adding route {"url": "/ext/bc/22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv", "endpoint": "/rpc"} \[04-17|15:50:45.332] INFO server/server.go:269 adding route {"url": "/ext/bc/22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv", "endpoint": "/ws"}

### If you place the subnet-evm in wrong directory or wrong file name, what happens?  <a href="#h7tlg" id="h7tlg"></a>

You will find this line:

ERROR node/node.go:849 failed to register VM {"vmID": "X5tFvg9JwoXgaYPQbceSzhGoCF6dhwrDm5BnAav6meFp3xxmg", "error": "handshake failed: vm process not found"}

### how to get node information?  <a href="#tvjzi" id="tvjzi"></a>

curl -X POST --data '{ "jsonrpc":"2.0", "id" :1, "method" :"info.getNodeID" }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info

sample output:

{ "jsonrpc": "2.0", "result": { "nodeID": "NodeID-5mb46qkSBj81k9g9e4VFjGGSbaaSLFRzD", "nodePOP": { "publicKey": "0x8f95423f7142d00a48e1014a3de8d28907d420dc33b3052a6dee03a3f2941a393c2351e354704ca66a3fc29870282e15", "proofOfPossession": "0x86a3ab4c45cfe31cae34c1d06f212434ac71b1be6cfe046c80c162e057...." } }, "id": 1 }

### Make sure your node healthy: <a href="#wa6ue" id="wa6ue"></a>

curl -X POST --data '{ "jsonrpc":"2.0", "id" :1, "method" :"health.health" }' -H 'content-type:application/json;' 127.0.0.1:9650/ext/health

Your should see the words "healthy": true in the response object.
