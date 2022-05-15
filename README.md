# Aptos fullnode setup for Testnet
Official documents:

Run a full node

Usefull tools:

To find latest block height use Aptos Network Dashboard
To check your node health status try Aptos Node Informer
To migrate your fullnode to another machine read Migrate your fullnode to another machine

## Hardware requirements:
For running a production grade Fullnode we recommend the following:
CPU: 4 cores (Intel Xeon Skylake or newer)
Memory: 8GiB RAM

If running the Fullnode for development or testing purpose:
CPU: 2 cores
Memory: 4GiB RAM
Set up your aptos fullnode
Option 1 (automatic)
Use script below for a quick installation

wget -qO aptos.sh https://raw.githubusercontent.com/kj89/testnet_manuals/main/aptos/aptos.sh && chmod +x aptos.sh && ./aptos.sh
Option 2 (manual)
You can follow manual guide if you better prefer setting up node manually

Update Aptos Fullnode version
wget -qO update.sh https://raw.githubusercontent.com/kj89/testnet_manuals/main/aptos/tools/update.sh && chmod +x update.sh && ./update.sh
(OPTIONAL) Update configs
wget -qO update_configs.sh https://raw.githubusercontent.com/kj89/testnet_manuals/main/aptos/tools/update_configs.sh && chmod +x update_configs.sh && ./update_configs.sh
Get your node identity and upstream details
wget -qO get_identity.sh https://raw.githubusercontent.com/kj89/testnet_manuals/main/aptos/tools/get_identity.sh && chmod +x get_identity.sh && ./get_identity.sh
Useful commands
Check Aptos logs
docker logs -f aptos-fullnode-1 --tail 50
Check sync status
curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type
Restart service
docker restart aptos-fullnode-1
