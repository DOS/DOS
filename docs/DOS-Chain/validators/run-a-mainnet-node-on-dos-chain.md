# Run a Mainnet Node on DOS Chain

### Introduction <a href="#lke9o" id="lke9o"></a>

This article describes how to run a Mainnet node on [DOS Chain](https://doscan.io/). The following necessary steps are needed to run your node on the DOS Chain:

1. Install AvalancheGo using the Install Script
2. Download the plugin binary for the DOS Subnet-EVM
3. Track the DOS Subnet
4. Connect to the DOS Subnet!

This install script assumes:

* AvalancheGo is not running and not already installed as a service
* User running the script has superuser privileges (can run sudo)

Official documents from Avalanche:

{% embed url="https://docs.avax.network/nodes/run/subnet-node" %}

### Install AvalancheGo using the Install Script <a href="#id-5d2kj" id="id-5d2kj"></a>

First, you need to download and install AvalancheGo (handles the orchestration of running Custom VMs). You can follow [this comprehensive guide](https://docs.avax.network/nodes/build/set-up-node-with-installer) to complete this step. For this tutorial, we recommend using the AvalancheGo Installer.

**Notes:** Some configurations you should notice:

* Enter your connection type \[1,2]: **2**
* RPC port should be public (this is a public API node) or private (this is a validator)? \[public, private]: **private**
* Detected 'x.x.x.x' as your public IP. Is this correct? \[y,n]: **y**
* **Done**

Notes:

* Your node should now be bootstrapping. Node configuration file is /home/DOS/.avalanchego/configs/node.json&#x20;
* C-Chain configuration file is /home/DOS/.avalanchego/configs/chains/C/config.json
* Plugin directory, for storing subnet VM binaries, is /home/DOS/.avalanchego/plugins
* To check that the service is running use the following command (q to exit): `sudo systemctl status avalanchego`&#x20;
* To follow the log use (ctrl-c to stop): `sudo journalctl -u avalanchego -f`

### Get the ID, the BLS key, and the proof of possession(BLS signature) of this node

* `nodeID` Node ID is the unique identifier of the node that you set to act as a validator on the Primary Network.
* `nodePOP` is this node's BLS key and proof of possession. Nodes must register a BLS key to act as a validator on the Primary Network. Your node's POP is logged on startup and is accessible over this endpoint.
  * `publicKey` is the 48 byte hex representation of the BLS key.
  * `proofOfPossession` is the 96 byte hex representation of the BLS signature.

To get these info:

```
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.getNodeID"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

### Install Avalanche CLI <a href="#uxyy9" id="uxyy9"></a>

[https://docs.avax.network/tooling/cli-guides/install-avalanche-cli](https://docs.avax.network/tooling/cli-guides/install-avalanche-cli)

{% embed url="https://docs.avax.network/tooling/cli-guides/install-avalanche-cli" %}

## Run a Subnet Node

[https://build.avax.network/docs/nodes/run-a-node/avalanche-l1-nodes](https://build.avax.network/docs/nodes/run-a-node/avalanche-l1-nodes)

{% embed url="https://build.avax.network/docs/nodes/run-a-node/avalanche-l1-nodes" %}

### Option1: Build Subnet Binaries[​](https://docs.avax.network/nodes/run/subnet-node#manage-the-subnet-binaries) <a href="#manage-the-subnet-binaries" id="manage-the-subnet-binaries"></a>

_After building AvalancheGo successfully,_

#### 1. Clone [Subnet-EVM](https://github.com/ava-labs/subnet-evm)[​](https://docs.avax.network/nodes/run/subnet-node#1-clone-subnet-evm) <a href="#id-1-clone-subnet-evm" id="id-1-clone-subnet-evm"></a>

```
cd $GOPATH/src/github.com/ava-labs
git clone https://github.com/ava-labs/subnet-evm.git
```

#### 2. Build the Binary and Save as a Plugin[​](https://docs.avax.network/nodes/run/subnet-node#2-build-the-binary-and-save-as-a-plugin) <a href="#id-2-build-the-binary-and-save-as-a-plugin" id="id-2-build-the-binary-and-save-as-a-plugin"></a>

In the Subnet-EVM directory, run the build script, and save it in the “plugins” folder of your `.avalanchego` data directory. Name the plugin after the `VMID` of the Subnet you wish to track. The `VMID` of the DOS Chain is the value beginning with “X5tF...”.

```
cd $GOPATH/src/github.com/ava-labs/subnet-evm
./scripts/build.sh ~/.avalanchego/plugins/X5tFvg9JwoXgaYPQbceSzhGoCF6dhwrDm5BnAav6meFp3xxmg
```

<details>

<summary>Where can I find Subnet parameters like VMID?</summary>

VMID, Subnet ID, ChainID, and all other parameters can be found in the "Chain Info" section of the Subnet Explorer.

* [Avalanche Mainnet](https://subnets.avax.network/c-chain)
* [Fuji Testnet](https://subnets-test.avax.network/wagmi)

</details>

### Option 2: Download subnet-evm Binary <a href="#uxyy9" id="uxyy9"></a>

_For the steps below, we will assume that you completed first step successfully and are now in your AvalancheGo directory (within your $GOPATH)._

Next, you will download the DOS-EVM binary (please notice that we use the same Binary file with the Ava Labs repo):

Download and unzip the latest subnet evm at: [https://github.com/ava-labs/subnet-evm/releases](https://github.com/ava-labs/subnet-evm/releases)

{% @github-files/github-code-block url="https://github.com/ava-labs/subnet-evm/releases" %}

The long string `X5tFvg9JwoXgaYPQbceSzhGoCF6dhwrDm5BnAav6meFp3xxmg` is the CB58 encoded VMID of the DOS Subnet-EVM. AvalancheGo will use the name of this file to determine what VMs are available to run from the plugins directory.

### Tracking DOS Chain and Restarting the Node <a href="#id-5rks9" id="id-5rks9"></a>

#### Joining DOS Subnet[​](https://docs.avax.network/build/subnet/deploy/on-prod-infra#joining-a-subnet) <a href="#joining-a-subnet" id="joining-a-subnet"></a>

For a node to join a Subnet, there are two prerequisites:

* Primary Network validation
* Subnet tracking

Primary Network validation means that a node cannot join a Subnet as a validator before becoming a validator on the Primary Network itself. So, after you add the node to the validator set on the Primary Network, node can join a Subnet.&#x20;

To have a node start syncing the Subnet, you need to add the `--track-subnets` command line option, or `track-subnets` key to the node config file (found at `.avalanchego/configs/node.json` for installer-script created nodes). A single node can sync multiple Subnets, so you can add them as a comma-separated list of Subnet IDs.

An example of a node config syncing two Subnets:

```
{
  "public-ip-resolution-service": "opendns",
  "http-host": "",
  "track-subnets": "nQCwF6V9y8VFjvMuPeQVWWYn6ba75518Dpf6ZMWZNb3NyTA94,Ai42MkKqk8yjXFCpoHXw7rdTWSHiKEMqh5h8gbxwjgkCUfkrk"
}
```

But that is not all. Besides tracking the SubnetID, the node also needs to have the plugin that contains the VM instance the blockchain in the Subnet will run.&#x20;

If you want to inspect the process of Subnet syncing, you can use the RPC call to check for the blockchain status.&#x20;

**Example Call:**

```
curl -X POST --data '{
    "jsonrpc": "2.0",
    "method": "platform.getBlockchainStatus",
    "params":{
        "blockchainID":"22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv"
    },
    "id": 1
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/bc/P
```

**Example Response:**

```
{
  "jsonrpc": "2.0",
  "result": {
    "status": "Validating"
  },
  "id": 1
}
```

### Running the Node <a href="#e9pzu" id="e9pzu"></a>

First, make sure to shut down your node in case it is still running. Then, you will navigate back into the AvalancheGo directory:

If you went through the steps to set up a config file, then you can launch your node by specifying the config file on the command line:

```
./build/avalanchego --config-file ~/.avalanchego/configs/node.json
```

If you want to track the Subnets through the command-line flag. You can append the other flags or even the --config-file flag as well, according to your need.

```
./build/avalanchego --track-subnets nQCwF6V9y8VFjvMuPeQVWWYn6ba75518Dpf6ZMWZNb3NyTA94
```

### Import DOS Chain Subnet

You firstly import DOS Chain using subnet import public  since DOS Chain is already live.

```
~/.avalanchego$ avalanche subnet import public "DOS Chain"
✔ Mainnet
Provide the path to the genesis file: /home/DOS/.avalanchego/configs/genesis.json
✔ Yes
Please provide an API URL of such a node so we can query its VM version (e.g. http://111.22.33.44:5555): http://20.6.105.146:9650
What is the ID of the blockchain?: 22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv
Getting information from the Mainnet network...
Retrieved information. BlockchainID: 22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv, SubnetID: nQCwF6V9y8VFjvMuPeQVWWYn6ba75518Dpf6ZMWZNb3NyTA94, Name: DOS Chain, VMID: X5tFvg9JwoXgaYPQbceSzhGoCF6dhwrDm5BnAav6meFp3xxmg
✔ Subnet-EVM
Subnet "DOS Chain" imported successfully
```

To see the Subnet Config:

`avalanche subnet describe "DOS Chain"`



```
avalanche subnet describe "DOS Chain"
```

#### Config Chain <a href="#id-7mifm" id="id-7mifm"></a>

DOS Chain allows the node operator to provide some custom configurations. The config file for DOS Chain is located at \~/.avalanchego/configs/chains/22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv/config.json

The content of config.json file should be like:

{ "feeRecipient": "0x50B65C1e3206621b6f9A5Ab2950Ae69f20634b82",//input your wallet }

### Just Want the Commands? We Got You <a href="#iavke" id="iavke"></a>

And finally, share us your **nodeID**.



## Run Another Node

You can reduce the node boostraping time by copying the database.

### **Database Copy**[**​**](https://docs.avax.network/build/subnet/deploy/on-prod-infra#database-copy)

Good way to cut down on bootstrap times on multiple nodes is database copy. Database is identical across nodes, and as such can safely be copied from one node to another. Just make sure to that the node is not running during the copy process, as that can result in a corrupted database. Database copy procedure is explained in detail [here](https://docs.avax.network/nodes/maintain/node-backup-and-restore#database).

Please make sure you don't reuse any node's NodeID by accident, especially don't restore another node's ID, see [here](https://docs.avax.network/nodes/maintain/node-backup-and-restore#nodeid) for details. Each node must has its own unique NodeID, otherwise, the nodes sharing the same ID will not behave correctly, which will impact your validator's uptime, thus staking rewards, and the stability of your Subnet.

{% embed url="https://docs.avax.network/nodes/maintain/node-backup-and-restore#database" %}
