## Установка ноды Sui

  системные требования
 
  (VPS/VDS/DS) 
  
   Ubuntu 20.04
   
   - CPUs: 2
   - RAM: 8GB
   - Storage: 50GB
   
   
##Подгодовка Сервера
  
   Обновить пакеты и систему

    ```
    sudo apt update 
  
    ```
    
    ```
    sudo apt upgrade -y
    
    ```

   установите дополнительные зависимости

   ```
    apt-get update \
    && apt-get install -y --no-install-recommends \
    tzdata \
    git \
    ca-certificates \
    curl \
    build-essential \
    libssl-dev \
    pkg-config \
    libclang-dev \
    cmake 
  ```
    
    Установите ```Rust```
    
    ```
    . <(wget -qO- https://raw.githubusercontent.com/SecorD0/utils/main/installers/rust.sh)
    ```
    
     
  Форкните [Репозиторий Sui](https://github.com/MystenLabs/sui)
  
  
  [![image.png](https://i.postimg.cc/gkMwLbjP/image.png)](https://postimg.cc/crn1PjfD)
  
  
Клонируйте свой личный форк репозитория Sui на свой пк 
Вместо ```YOUR-GITHUB-USERNAME``` Вставьте свой ник из Гитхаба
  
 ```
 git clone https://github.com/<YOUR-GITHUB-USERNAME>/sui.git
 ```
 
 Войдите в свой репозиторий
 
 ```
 cd sui
 ```
 
 Создайте ветку с исходным репозиторием
 
 ```
 git remote add upstream https://github.com/MystenLabs/sui
 ```
 
 Синхронизируйте свой форк
 
 ```
 git fetch upstream
 ```
 
 Переключитесь на ```devnet```
 
 ```
 git checkout --track upstream/devnet
 ```
 
 Скопируйте [конфиг](https://github.com/MystenLabs/sui/blob/main/crates/sui-config/data/fullnode-template.yaml) 
  
 ```
 cp crates/sui-config/data/fullnode-template.yaml fullnode.yaml
 ```
 
 Скачайте  ```genesis``` фаил
 
 ```
 curl -fLJO https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
 ```
 
запускаем ноду 
cargo run --release --bin sui-node -- --config-path fullnode.yaml

Открыть Nano
nano /root/sui/fullnode.yaml
 
 и путь к genesis файлу поправь

/root/sui/genesis.blob

поменяй ip на 0.0.0.0

Создайте сервисный фаил

```
sudo tee /etc/systemd/system/sui-node.service > /dev/null <<EOF 
[Unit] 
Description=sui-node 
After=network-online.target 
[Service] 
User=$USER
ExecStart=/root/sui/target/release/sui-node --config-path /root/sui/fullnode.yaml
Restart=on-failure 
RestartSec=10 
LimitNOFILE=65535
[Install] 
WantedBy=multi-user.target 
EOF
```
Рестартнуть Ноду

```
sudo systemctl enable sui-node
```
```
sudo systemctl daemon-reload 
```
```
sudo systemctl restart sui-node

```
```
journalctl -u sui-node.service -f
```













 


 


   


    
   
