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
echo chain-id:" "$($TIKER status 2>&1 | jq ."NodeInfo" | jq ."network")
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

## Настройка Discord Alerts
### Создание канала    
> Нажать на кнопку:   
> <img width="50" alt="image" src="https://user-images.githubusercontent.com/83868103/190870811-5a6f4ebe-e20e-47d4-8803-40811adfedde.png">  
> 
> Выбрать:   
> <img width="250" alt="image" src="https://user-images.githubusercontent.com/83868103/190870865-4ea927f7-3a79-4fda-b199-2730f1191f19.png">  
>    
> Выбрать:  
> <img width="250" alt="image" src="https://user-images.githubusercontent.com/83868103/190871041-638598e7-42da-40f4-9839-8a18970c32b7.png">  
>  
> Ввести название сервера:   
> <img width="250" alt="image" src="https://user-images.githubusercontent.com/83868103/190871205-5c6e474c-9619-4dd1-8227-99724e52051d.png">  
> 
> Завершить создание канала нажатием кнопки:     
> <img width="90" alt="image" src="https://user-images.githubusercontent.com/83868103/190871249-baf6bf83-c2ed-466c-958b-42313777ab3d.png"> 
> 
### Интеграция ноды с Discord
> Открыть "Настройки сервера" в созданном канале:   
> <img width="200" alt="image" src="https://user-images.githubusercontent.com/83868103/190871404-aef215df-c5e2-42f5-9733-47a415125b31.png">
>   
> Перейти во вкладку "Интеграция":    
> <img width="200" alt="image" src="https://user-images.githubusercontent.com/83868103/190871443-e37e511e-7f43-4b82-a03a-cc67a79cecdc.png">  
> 
> Создать новый "Вебхук":  
> <img width="500" alt="image" src="https://user-images.githubusercontent.com/83868103/190871554-de41e047-2c15-4f00-aaf8-b4c2550a6f55.png">  
> 
> Ввести данные и "Копировать URL вебхука":   
> <img width="500" alt="image" src="https://user-images.githubusercontent.com/83868103/190871582-5f5f1fce-2146-420f-b6f9-63bb80dd489f.png">  










 
