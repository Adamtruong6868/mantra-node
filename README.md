# MANTRA - NODE
## Setup Server

### Update Packages

```sh
sudo apt update && apt upgrade -y
sudo apt install curl git jq lz4 build-essential unzip fail2ban ufw -y
```

### Set Firewall
```sh
sudo apt install -y ufw
```
```sh
sudo ufw default allow outgoing
```
```sh
sudo ufw default deny incoming
```
```sh
sudo ufw allow ssh
```
```sh
sudo ufw allow 9100
```
```sh
sudo ufw enable
sudo ufw status numbered
```
### Install Go
```sh
ver="1.20.3"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```
### Install Cosmovisor (OPTIONAL If you are using Cosmovisor)
```sh
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```
## Install node
### Download 
```sh
cd $HOME
rm -rf entangle-blockchain/
git clone https://github.com/Entangle-Protocol/entangle-blockchain
```
### Install binary
```sh
cd entangle-blockchain/
make build
```
### Setup Cosmovisor Symlinks
```sh
mkdir -p $HOME/.entangled/cosmovisor/genesis/bin
```
```sh
mv build/entangled $HOME/.entangled/cosmovisor/genesis/bin/
```
```sh
rm -rf build
```
```sh
sudo ln -s $HOME/.entangled/cosmovisor/genesis $HOME/.entangled/cosmovisor/current
```
```sh
sudo ln -s $HOME/.entangled/cosmovisor/current/bin/entangled /usr/local/bin/entangled
```
### Set Configuration for your node
```sh
entangled config chain-id entangle_33133-1
```
```sh
entangled config keyring-backend file
```
### Init your node
#### You can change "VNBnodes" to anything yours prefer name
```sh
entangled init VNBnodes --chain-id entangle_33133-1
```

#### Add Genesis File and Addrbook
```sh
curl -Ls https://snapshots.indonode.com/mantra/genesis.json > $HOME/.mantrachain/config/genesis.json
```
```sh
curl -Ls https://snapshots.indonode.com/mantra/addrbook.json > $HOME/.mantrachain/config/addrbook.json
```
#### Configure Seeds and Peers
```sh
PEERS="$(curl -sS https://rpc.mantra-t.indonode.net/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}' | sed -z 's|\n|,|g;s|.$||')"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.mantrachain/config/config.toml
```
#### Set Pruning, Enable Prometheus, Gas Price, and Indexer
```sh
PRUNING="custom"
PRUNING_KEEP_RECENT="100"
PRUNING_INTERVAL="19"
```
```sh
sed -i -e "s/^pruning *=.*/pruning = \"$PRUNING\"/" $HOME/.mantrachain/config/app.toml
```
```sh
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \
\"$PRUNING_KEEP_RECENT\"/" $HOME/.mantrachain/config/app.toml
```
```sh
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \
\"$PRUNING_INTERVAL\"/" $HOME/.mantrachain/config/app.toml
```
```sh
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.mantrachain/config/config.toml
```
```sh
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.mantrachain/config/config.toml
```
```sh
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uaum\"/" $HOME/.mantrachain/config/app.toml
```
