## Установка ноды Sui

  системные требования
 
  (VPS/VDS/DS) 
  
   Ubuntu 20.04
   
   - CPUs: 2
   - RAM: 8GB
   - Storage: 50GB
   
   
## Подгодовка Сервера
  
   Обновить пакеты и систему

  ```
  sudo apt update 
  ```
    
  ``` 
  sudo apt upgrade -y
  ```
   

   Установим дополнительные зависимости

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
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```
  
  
 ```
 source "$HOME/.cargo/env"
 ```
  
   
    
     
  Форкаем [Репозиторий Sui](https://github.com/MystenLabs/sui)
  
  
  [![image.png](https://i.postimg.cc/gkMwLbjP/image.png)](https://postimg.cc/crn1PjfD)
  
  
Клонируем свой репозиторий 

Вместо ```YOUR-GITHUB-USERNAME``` Вставляем свой ник из Гитхаба
  
 ```
 git clone https://github.com/<YOUR-GITHUB-USERNAME>/sui.git
 ```
 
 Войдем в свой репозиторий
 
 ```
 cd sui
 ```
 
 Создаем ветку с исходным репозиторием
 
 ```
 git remote add upstream https://github.com/MystenLabs/sui
 ```
 
 Синхронизируем свой форк
 
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
 
Компилируем бинарные файлы (занимает долгое время) 
 
 ```
cargo run --release --bin sui-node -- --config-path fullnode.yaml
 ```

Редактируем ```fullnode.yaml```

```
nano /root/sui/fullnode.yaml
```
 
тут же меняем путь к ```genesis``` файлу 

```/root/sui/genesis.blob```

Меняем IP на 0.0.0.0

В итоге должно получиться следующее:

[![image.png](https://i.postimg.cc/FHk8P8qt/image.png)](https://postimg.cc/yDBLxQsn) 


Создаем сервисный фаил

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

Рестартнем ноду

```
sudo systemctl enable sui-node
```
```
sudo systemctl daemon-reload 
```
```
sudo systemctl restart sui-node
```

Проверяем логи
```
journalctl -u sui-node.service -f
```


## Обновление

 Остановите ноду
 
 ```
 systemctl stop suid
 ```
 
 
 Удалите старую базу данных ```suidb```:

```
rm -rf /root/sui/suidb/
```

Удалите старый генезис и установите новый: 

```
wget -O /root/.sui/genesis.blob https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
```

Обновите папку ```sui```
```
cd sui
```
```
git fetch upstream
```
```
git stash
```
```
git checkout -B devnet --track upstream/devnet
```

скомпилируйте новый бинарник и запустите ноду
```
cargo run --release --bin sui-node -- --config-path /root/.sui/fullnode.yaml
```

Проверьте версию
```
sui --version
```












 


 


   


    
   
