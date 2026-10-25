## Задача 0
- Проверено отсутствие `docker-compose` (v1): `command not found`.
- Проверено наличие `docker compose` (v2).
<img width="557" height="39" alt="0" src="https://github.com/user-attachments/assets/1948194d-4d8b-4804-a973-1b8529ee3091" />


## Задача 1
- Создан `Dockerfile.python` с multistage-сборкой.
- Созданы `.dockerignore` и `.gitignore`.
- Использованы `COPY . .` и `CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"]`.
<img width="709" height="356" alt="1" src="https://github.com/user-attachments/assets/3e0d9ecf-a296-403c-94f9-e39adb8f9e4b" />



## Задача 2 (*)
*Задача со звездочкой не выполнялась.*

## Задача 3
Код `compose.yaml`:
```yaml
include:
  - proxy.yaml

services:
  web:
    build:
      context: .
      dockerfile: Dockerfile.python
    container_name: web
    networks:
      backend:
        ipv4_address: 172.20.0.5
    restart: always
    environment:
      DB_HOST: db
      DB_USER: ${MYSQL_USER}
      DB_PASSWORD: ${MYSQL_PASSWORD}
      DB_NAME: ${MYSQL_DATABASE}
    depends_on:
      - db

  db:
    image: mysql:8
    container_name: db
    networks:
      backend:
        ipv4_address: 172.20.0.10
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql

networks:
  backend:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16

volumes:
  db_data:
```
<img width="1306" height="150" alt="3" src="https://github.com/user-attachments/assets/bcaa7479-b0be-4a95-b894-e99f30e770ca" />



## Задача 4
Bash-скрипт /opt/deploy.sh:
```bash
#!/bin/bash
set -e

if ! command -v git &> /dev/null; then
    sudo apt-get update && sudo apt-get install -y git
fi

cd /opt
if [ ! -d "netology-app" ]; then
    git clone https://github.com/lexremens/shvirtd-example-python.git netology-app
fi

cd netology-app
docker compose up -d --build
```
<img width="1406" height="1125" alt="image" src="https://github.com/user-attachments/assets/7dbfa3dc-7eb9-4010-b468-1a9fae02e6fd" />
<img width="922" height="1037" alt="image" src="https://github.com/user-attachments/assets/2135687e-a8a9-480a-802c-cf376bae1a68" />



## Задача 5 (*)
*Задача со звездочкой не выполнялась.*

## Задача 6
```bash
sudo docker pull hashicorp/terraform:latest
sudo docker save hashicorp/terraform:latest -o terraform.tar
dive hashicorp/terraform:latest

# Извлечение и проверка бинарника
sudo docker create --name tf_temp hashicorp/terraform:latest
sudo docker cp tf_temp:/bin/terraform ./terraform
sudo docker rm tf_temp
chmod +x ./terraform
./terraform version
```
<img width="974" height="634" alt="image" src="https://github.com/user-attachments/assets/e751f468-eb61-4e22-aa18-c14cb193b810" />
<img width="373" height="122" alt="image" src="https://github.com/user-attachments/assets/7a18e778-cf32-4104-b685-d732643501a7" />

