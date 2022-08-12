## Установка ноды Sui

   системные требования
 
  (VPS/VDS/DS) 
  
   Ubuntu 20.04
   
   - CPUs: 2
   - RAM: 8GB
   - Storage: 50GB
  
    Обновить пакеты и систему

     ```
    sudo apt update && sudo apt upgrade -y
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
     
  Форкните репозиторий Sui https://github.com/MystenLabs/sui [репозиторий Sui](#https://github.com/MystenLabs/sui)
  [![image.png](https://i.postimg.cc/gkMwLbjP/image.png)](https://postimg.cc/crn1PjfD)

   


    
   
