# Running Nodes For Genesis Validator

![logo](https://miro.medium.com/max/1400/1*FuaS0MBs0hZkyVJx21e9Tw.png)

* [Official site](https://www.lukso.network/)
  
* [L16 Public Testnet](https://docs.lukso.tech/networks/l16-testnet/)


## **Server prepare**

### Update:

```
sudo apt update && sudo apt upgrade -y
```
### Packages:

```
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```
```
sudo apt install -y uidmap dbus-user-session
```

## **Installation**

### 1. Install Docker:
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 2. Install Docker-compose:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

### 3. We have been working on a new LUKSO cli, in order to retrieve it, run the following command:

```
 curl https://raw.githubusercontent.com/lukso-network/lukso-cli/main/cli_downloader.sh | bash
```

### 4. It is recommended to put the cli binary into the PATH or in a bin directory that is already part of the PATH:

```
cp lukso /usr/local/bin/lukso
```

### 5. You also have to create a directory and setup the network:

```
 mkdir MY_NODE_DIR
 cd MY_NODE_DIR
 lukso network init
```

This will download the dependencies.

### 6. Now, unzip the validator key archive beta_x.zip (attached to email) and copy the contents into the directory MY_NODE_DIR/keystore:

```
 mkdir MY_NODE_DIR/keystore
 `Copy archive beta_x.zip to directory MY_NODE_DIR/keystore`
 sudo apt install unzip 
 unzip beta_x.zip
```

### 7. Your directory structure should look like this:

* `MY_NODE_DIR`
   * configs
   * data
   * `keystore`
       * keys
       * lodestar-secrets
       * nimbus-keys
       * password.txt
       * prysm
       * pubkeys.json
       * secrets
       * teku-keys
       * teku-secrets
   * docker-compose.yml
   * .env
   * node_config.yaml
  
  ### 8. Start the node with:

```
cd MY_NODE_DIR
lukso network start (this command might take a while)
lukso network start validator
```

## **Service**

### 1. Logs:

```
lukso network log consensus -f
lukso network log execution -f
lukso network log validator -f
```

### 2. Explorer:
https://stats.execution.beta.l16.lukso.network/

https://stats.consensus.beta.l16.lukso.network/


### 3. Restarting the node and validator:

```
lukso network stop validator
lukso network stop
lukso network clear
lukso network start
lukso network start validator
```
### 4. Bootnodes:

#### Bootnode 0
```
enr:-MK4QD8FkG2sIP1TGscBlgHyscwDySAN8EoT4m5IKBFNY04nQOTZMvGpAqYhXrtT0wqhngnpkS2E1C_qO54JbdSajsSGAYC4KBLkh2F0dG5ldHOIAAAAAAAAAACEZXRoMpCBQMXLYgAAcf__________gmlkgnY0gmlwhCJaVcaJc2VjcDI1NmsxoQO_eYR-N9Z-Ad_ki-PLYAyUMQ-s1od8zUo59osUBEOwmIhzeW5jbmV0cwCDdGNwgjLIg3VkcIIu4A
```
```
enode://8f2d010f18233b205609c8f2e78c56a1911e04f9113aa2dce5ccb61cfbb09956c7dbce47a93ca9c35b82f2493398447fdbae1db097bd26476220454b7cea392d@34.90.85.198:30303
```
#### Bootnode 1
```
enr:-MK4QJNG7HrFhqNTLZOOjNS_O4X2c6B4xjMG921TjtLsdHGfOLPaJBh-TKQCM-h_s6K3XEIdB_PEFDAHrriLxSDja92GAYCyCocIh2F0dG5ldHOIAAAAAAAAAACEZXRoMpCBQMXLYgAAcf__________gmlkgnY0gmlwhCJbpv2Jc2VjcDI1NmsxoQIxYwcPw9L7_25e53hmJssY53fAyr48vZCxzdgD5CgNqIhzeW5jbmV0cwCDdGNwgjLIg3VkcIIu4A
```
```
enode://e4dd2a73cd9d2c46d429e8205537f168c3a890e3b9ad073c45c40deb062bc738a5248c654d72c6a3115a9fa68b9140842de1dc6a8a269ece118fb954626ab104@34.91.166.253:30303
```
#### Bootnode 2
```
enr:-MK4QP2gSGuSB1YtRLhNRdx9PT3ttKih6TtXPuKUILE79jN4Ti1-FT54KcHH3MrvF7KTfgzN0BG062RCKmlk9xkVa7WGAYC4I9iVh2F0dG5ldHOIAAAAAAAAAACEZXRoMpCBQMXLYgAAcf__________gmlkgnY0gmlwhCJbMxaJc2VjcDI1NmsxoQNiwNrzDkDeTLxEF36JAwE5icN5FCK0WrgFEitt1mBDmIhzeW5jbmV0cwCDdGNwgjLIg3VkcIIu4A
```
```
enode://5f894de652134f2cf4be259b6bb8d74352913e3854cd5b5efb326474f94b4c6366a402e9493e26922e5ffc53a7f6ff6e34e76e99161c2999c9e3e1eaa34751e6@34.91.51.22:30303
```

## **Change Log**
[0.0.4] - 2022-05-13

This update mainly improves existing commands and adds the possibility to

### Added


* command `lukso network describe` that will give information on the state of the network

* command `lukso network validator deposit` that will deposit new validators

### Changed

* command `lukso network validator describe` that will show additional information

* the installer script `cli_downloader.sh` will copy the cli binary to /usr/local/bin

### Fixed

* command `lukso network validator setup` it will create a local transaction wallet.
