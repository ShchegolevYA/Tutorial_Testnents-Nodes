# Running Nodes For Incentivized Testnet

![Twitter](https://img.shields.io/twitter/url?label=Twitter&style=social&url=https%3A%2F%2Ftwitter.com%2Faptoslabs)

![logo](https://miro.medium.com/max/1400/0*1QPBZhIrgVi-_UwG)

* [Official blog post on Medium about the launch of the Incentivized Testnet](https://medium.com/aptoslabs/launch-of-aptos-incentivized-testnet-registration-2e85696a62d0)

* [Site for registering a node and passing KYC](https://community.aptoslabs.com/)




* [Official guide to running a full node and a validator node](https://aptos.dev/tutorials/validator-node/intro/)

* [Terms and Conditions](https://aptos.dev/tutorials/validator-node/intro/)

## **Server prepare**

### Update

```
sudo apt update && sudo apt upgrade -y
```
### Packages

```
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```
```
sudo apt-get install -y cargo
```
```
sudo apt install rustc
rustc --version
```
```
sudo apt install -y uidmap dbus-user-session
```

## **Installation**

### 1. Install Docker

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 2. Install Docker-compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```
### 3. Install the aptos CLI
* Execute the below step to ensure that your current shell environment knows where cargo is:
```
source $HOME/.cargo/env
```
* Install the aptos CLI tool by running the below command. You can run this command from any directory. The aptos CLI tool will be installed into your CARGO_HOME, usually ~/.cargo:
```
cargo install --git https://github.com/aptos-labs/aptos-core.git aptos --tag aptos-cli-latest
```
* Confirm that the aptos CLI tool is installed successfully by running the below command. The terminal will display the path to the aptos CLI's location:
```
which aptos
```
### 4. Create a directory for your Aptos node composition. e.g.:

```
export WORKSPACE=testnet
mkdir ~/$WORKSPACE
cd ~/$WORKSPACE
```
### 5. Download the validator.yaml, fullnode.yaml and docker-compose.yaml configuration files into this directory:
```
wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/docker-compose.yaml
wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/validator.yaml
wget https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/fullnode.yaml
```
### 6. Generate key pairs (node owner key, consensus key and networking key) in your working directory:
```
aptos genesis generate-keys --output-dir ~/$WORKSPACE
```
This will create three files: `private-keys.yaml`, `validator-identity.yaml`, `validator-full-node-identity.yaml` for you. IMPORTANT: Backup your key files somewhere safe. These key files are important for you to establish ownership of your node, and you will use this information to claim your rewards later if eligible. Never share those keys with anyone else.

### 7. Configure validator information, you need to setup a static IP / DNS address which can be used by the node, and make sure the network / firewalls are properly configured to accept external connections. See Network Identity For FullNode for how to do this:

```
aptos genesis set-validator-configuration \
    --keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \
    --username <select a username for your node> \
    --validator-host <Validator Node IP / DNS address>:<Port> \
    --full-node-host <Full Node IP / DNS address>:<Port>

 # for example, with IP:

aptos genesis set-validator-configuration \
    --keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \
    --username aptosbot \
    --validator-host 35.232.235.205:6180 \
    --full-node-host 34.232.235.205:6182

# for example, with DNS:

aptos genesis set-validator-configuration \
    --keys-dir ~/$WORKSPACE --local-repository-dir ~/$WORKSPACE \
    --username aptosbot \
    --validator-host bot.aptosdev.com:6180 \
    --full-node-host fn.bot.aptosdev.com:6182

his will create a YAML file in your working directory with your username, e.g. `aptosbot.yml`, it should looks like:

account_address: 7410973313fd0b5c69560fd8cd9c4aaeef873f869d292d1bb94b1872e737d64f
consensus_key: "0x4e6323a4692866d54316f3b08493f161746fda4daaacb6f0a04ec36b6160fdce"
account_key: "0x83f090aee4525052f3b504805c2a0b1d37553d611129289ede2fc9ca5f6aed3c"
network_key: "0xa06381a17b090b8db5ffef97c6e861baad94a1b0e3210e6309de84c15337811d"
validator_host:
  host: 35.232.235.205
  port: 6180
full_node_host:
  host: 35.232.235.205
  port: 6182
stake_amount: 1       
```

### 8. Create layout YAML file, which defines the node in the validatorSet, for test mode, we can create a genesis blob containing only one node:
```
cd ~/$WORKSPACE
vi layout.yaml
```
Add the public key for root account, node username, and chain_id in the `layout.yaml`  file, for example:
```
---
root_key: "0x5243ca72b0766d9e9cbf2debf6153443b01a1e0e6d086c7ea206eaf6f8043956"
users:
  - <username you specified in step 5>
chain_id: 23
```
You can use the same root key as the example, or generate new one yourself by running `aptos key generate --output-file <file name>`

To save and exit vim :
```
:wq
```


### 9. Download AptosFramework Move bytecodes:
Download the Aptos Framework from the release page: https://github.com/aptos-labs/aptos-core/releases/tag/aptos-framework-v0.1.0

```
sudo apt install unzip
cd ~/$WORKSPACE
wget https://github.com/aptos-labs/aptos-core/releases/download/aptos-framework-v0.1.0/framework.zip
unzip framework.zip
```
You should now have a folder called `framework`, which contains move bytecodes with format `.mv`.

### 10. Compile genesis blob and waypoint:
```
cd ~/$WORKSPACE
ptos genesis generate-genesis --local-repository-dir ~/$WORKSPACE --output-dir ~/$WORKSPACE
```
This should create two files in your working directory, `genesis.blob` and `waypoint.txt`.
### 11. To recap, in your working directory, you should have a list of files:

* `validator.yam` validator config file
* `fullnode.yaml` fullnode config file
* `docker-compose.yaml` docker compose file to run validator and fullnode
* `private-keys.yaml` Private keys for owner account, consensus, networking
* `validator-identity.yaml` Private keys for setting validator identity
* `validator-full-node-identity.yaml` Private keys for setting validator full node identity
* `<username>.yaml` Node info for both validator / fullnode
* `layout.yaml` layout file to define root key, validator user, and chain ID
* `framework` folder which contains all the move bytecode for AptosFramework.
* `waypoint.txt` waypoint for genesis transaction
* `genesis.blob` genesis binary contains all the info about framework, validatorSet and more.

### 12. Run docker-compose:
```
cd ~/$WORKSPACE
docker-compose up -d
```

## **Service**

### 1. Logs:

```
docker logs -f testnet_fullnode_1
docker logs -f testnet_validator_1
```

### 2. Explorer:
https://www.nodex.run/aptos_test/

https://aptos-node.info/

## **Connecting to Aptos Incentivized Testnet**
Only do this if you got confirmation email from Aptos team for your eligibility. Nodes not selected will not be included in the genesis, thus not be able to connecting to incentivized testnet as a validator node. You can still run public fullnode in this case if you want.

https://aptos.dev/tutorials/validator-node/connect-to-testnet
