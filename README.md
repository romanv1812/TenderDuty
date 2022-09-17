## TenderDuty v2 - monitoring and alerting
<img width="1999" alt="image" src="https://user-images.githubusercontent.com/83868103/190860486-4dc9c7ac-8884-4e85-a643-7c9777e29536.png">

##

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
wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/romanv1812/TenderDuty/main/config.yml"
nano $HOME/tenderduty/config.yml
```
## Настройка мониторинга:
#### Команды выполняются на сервере с нодой
### Ввод переменных:
```bash
NODE_FOLDER=.haqq
TIKER=haqqd
WALLET=wallet
```
### Получение данных для редактирования config.yml
```bash
echo -e "\033[0;32mprometheus_listen_port$(grep -A 8 "\[instrumentation\]" ~/$NODE_FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"
echo valoper_address:$($TIKER keys show $WALLET --bech val -a)
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me)$(grep -A 3 "\[rpc\]" ~/$NODE_FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"
```
### На Сервере с TenderDuty нужно внести свои данные в шаблон:
```bash
nano $HOME/tenderduty/config.yml # Или открыть через SFTP config.yml
```
### После настройки конфига запускаем
```bash
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
```
### Смотрим логи
```bash
docker logs -f --tail 20 tenderduty
```
<img width="873" alt="image" src="https://user-images.githubusercontent.com/83868103/190863293-654a89e0-c092-4b97-850b-599777289ad9.png">



### Получить ссылку панели мониторинга
```bash
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"
```
