# MANTRA - NODE
### Setup Server

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
