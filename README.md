## TenderDuty v2 - monitoring and alerting
<img width="1999" alt="image" src="https://user-images.githubusercontent.com/83868103/190860486-4dc9c7ac-8884-4e85-a643-7c9777e29536.png">

### Подготовка сервера

```bash 
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
### Установка docker
```bash
. <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/docker.sh)
```
### Устанавливаем tenderduty
```bash
mkdir tenderduty && cd tenderduty
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
```
### Скачать шаблон конфига
```bash
wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/romanv1812/guides/main/config.yml"
nano $HOME/tenderduty/config.yml
```
