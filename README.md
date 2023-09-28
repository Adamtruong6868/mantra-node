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
