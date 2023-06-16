[![foodgram_workflow](https://github.com/baciroff/foodgram-project-react/actions/workflows/foodgram.yml/badge.svg)](https://github.com/baciroff/foodgram-project-react/actions/workflows/foodgram.yml)
# "Продуктовый помощник" (Foodgram)

## 1. [Описание](#1)
## 2. [Установка Docker (на платформе Ubuntu)](#2)
## 3. [База данных и переменные окружения](#3)
## 4. [Команды для запуска](#4)
## 5. [Заполнение базы данных](#5)
## 6. [Техническая информация](#6)

---
## 1. Описание <a id=1></a>

Проект "Продуктовый помошник" (Foodgram) предоставляет пользователям следующие возможности:
  - регистрироваться
  - создавать свои рецепты и управлять ими (корректировать\удалять)
  - просматривать рецепты других пользователей
  - добавлять рецепты других пользователей в "Избранное" и в "Корзину"
  - подписываться на других пользователей
  - скачать список ингредиентов для рецептов, добавленных в "Корзину"

---
## 2. Установка Docker (на платформе Ubuntu) <a id=2></a>

Проект поставляется в четырех контейнерах Docker (db, frontend, backend, nginx).  
Для запуска необходимо установить Docker и Docker Compose.  
Подробнее об установке на других платформах можно узнать на [официальном сайте](https://docs.docker.com/engine/install/).

Для начала необходимо скачать и выполнить официальный скрипт:
```bash
apt install curl
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

При необходимости удалить старые версии Docker:
```bash
apt remove docker docker-engine docker.io containerd runc 
```

Установить пакеты для работы через протокол https:
```bash
apt update
```
```bash
apt install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common -y 
```

Добавить ключ GPG для подтверждения подлинности в процессе установки:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Добавить репозиторий Docker в пакеты apt и обновить индекс пакетов:
```bash
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
```
```bash
apt update
```

Установить Docker(CE) и Docker Compose:
```bash
apt install docker-ce docker-compose -y
```

Проверить что  Docker работает можно командой:
```bash
systemctl status docker
```

Подробнее об установке можно узнать по [ссылке](https://docs.docker.com/engine/install/ubuntu/).

---
## 3. База данных и переменные окружения <a id=3></a>

Проект использует базу данных PostgreSQL.  
Для подключения и выполненя запросов к базе данных необходимо создать и заполнить файл ".env" с переменными окружения в папке "./infra/".

Шаблон для заполнения файла ".env":
```python
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY='Здесь указать секретный ключ'
ALLOWED_HOSTS='Здесь указать имя или IP хоста' (Для локального запуска - 127.0.0.1)
```

---
## 4. Команды для запуска <a id=4></a>

Перед запуском необходимо склонировать проект:
```bash
HTTPS: git clone https://github.com/baciroff/foodgram-project-react.git
SSH: git clone git@github.com:baciroff/foodgram-project-react.git
```

Cоздать и активировать виртуальное окружение:
```bash
python -m venv venv
```
```bash
Linux: source venv/bin/activate
Windows: source venv/Scripts/activate
```

И установить зависимости из файла requirements.txt:
```bash
python3 -m pip install --upgrade pip
```
```bash
pip install -r requirements.txt
```

Далее необходимо собрать образы для фронтенда и бэкенда.  
Из папки "./backend/foodgram/" выполнить команду:
```bash
docker build -t baciroff/foodgram_backend .
```

Из папки "./frontend/" выполнить команду:
```bash
docker build -t baciroff/foodgram_frontend .
```

После создания образов можно создавать и запускать контейнеры.  
Из папки "./infra/" выполнить команду:
```bash
docker-compose up -d
```

После успешного запуска контейнеров выполнить миграции:
```bash
docker-compose exec backend python manage.py migrate
```

Создать суперюзера (Администратора):
```bash
docker-compose exec backend python manage.py createsuperuser
```

Собрать статику:
```bash
docker-compose exec backend python manage.py collectstatic --no-input
```

Теперь доступность проекта можно проверить по адресу [http://localhost/](http://localhost/)

---
## 5. Заполнение базы данных <a id=5></a>

С проектом поставляются данные об ингредиентах.  
Заполнить базу данных ингредиентами можно выполнив следующую команду из папки "./infra/":
```bash
docker-compose exec backend python manage.py fill_ingredients_from_csv --path data/
```

Также необходимо заполнить базу данных тегами (или другими данными).  
Для этого требуется войти в [админ-зону](http://localhost/admin/)
проекта под логином и паролем администратора (пользователя, созданного командой createsuperuser).

---
## 6. Техническая информация <a id=6></a>

Стек технологий: Python 3, Django, Django Rest, React, Docker, PostgreSQL, nginx, gunicorn, Djoser.

Веб-сервер: nginx (контейнер nginx)  
Frontend фреймворк: React (контейнер frontend)  
Backend фреймворк: Django (контейнер backend)  
API фреймворк: Django REST (контейнер backend)  
База данных: PostgreSQL (контейнер db)

Веб-сервер nginx перенаправляет запросы клиентов к контейнерам frontend и backend, либо к хранилищам (volume) статики и файлов.  
Контейнер nginx взаимодействует с контейнером backend через gunicorn.  
Контейнер frontend взаимодействует с контейнером backend посредством API-запросов.









<!-- # praktikum_new_diplom
# Проект Foodgram
Сайт Foodgram, «Продуктовый помощник».
Онлайн-сервис и API для него. На этом сервисе пользователи после регистрации могут публиковать рецепты, подписываться на публикации других пользователей, добавлять понравившиеся рецепты в список «Избранное», а перед походом в магазин скачивать сводный список продуктов, необходимых для приготовления одного или нескольких выбранных блюд.

![example workflow](https://github.com/baciroff/foodgram-project-react/actions/workflows/foodgram.yml/badge.svg)

## Используемые технологии:

Django 3.2.13

Python 3.10.4

Django REST Framework 3.12.4

PostgreSQL 13.0-alpine

Nginx 1.19.3

Gunicorn 20.1.0

Docker 20.10.17, build 100c701

Docker-compose 3.8

GitHub Actions

## Проект доступен по адресу:
```
http://fooodgram.hopto.org
http://84.201.159.106
```

## Аминистратор:

- Имя пользователя - baciroff
- email - baciroff.ruslan@yandex.ru
- Пароль - 3721932A

### Как запустить проект:

- Для развёртывания проекта необходимо скачать его в нужную вам директорию, например:

```git clone git@github.com:sapphirehead/foodgram-project-react.git```

*Нужно установить docker и docker-compose. Настроить Dockerfile, docker-compose.yaml, foodgram.yml согласно вашим данным.*
*После настроек и push на GitHub проект проверятся тестами и линтером flake8, загружает образ на Docker Hub, разворачивает образ на сервере.*

- Выполните вход на свой удаленный сервер:

```
ssh <YOUR_USERNAME>@<IP_ADDRESS>
```
- Установите docker на сервер:

```sudo apt install docker.io```

- Установите docker-compose на сервер:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```sudo chmod +x /usr/local/bin/docker-compose```

- отредактируйте файл infra/nginx.conf, в строке server_name впишите свой IP.

- Скопируйте файлы из каталога infra: infra/docker-compose.yml и infra/nginx.conf из вашего проекта на сервер в home/<ваш_username>/docker-compose.yml и home/<ваш_username>/nginx.conf соответственно. Введите команду из корневой папки проекта:

```
scp docker-compose.yml <username>@<host>:/home/<username>/docker-compose.yml
```
```
scp nginx.conf <username>@<host>:/home/<username>/nginx.conf
```

- В директории infra создайте файл .env с переменными окружения для работы с базой данных, например такой:

```
DJANGO_KEY='your Django secret key'
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя вашей базы данных
POSTGRES_USER=postgres # ваш логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # ваш пароль для подключения к БД (установите свой)
DB_HOST=db # ваше название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

- Для запуска необходимо выполнить из директории с проектом команду:

```sudo docker-compose up -d```

_Для пересборки команда up выполняется с параметром --build_

```sudo docker-compose up -d --build```

### На сервере.

- Сделать миграции:

```sudo docker-compose exec backend python manage.py migrate```

- Создать суперпользователя:

```sudo docker-compose exec backend python manage.py createsuperuser```

- Собрать статику:

```sudo docker-compose exec backend python manage.py collectstatic --no-input```

- Вы также можете создать дамп (резервную копию) базы:

```sudo docker-compose exec backend python manage.py dumpdata > fixtures.json```

- или, разместив, например, файл fixtures.json в папке с Dockerfile, загрузить в базу данные из дампа:

```sudo docker-compose exec backend python manage.py loaddata fixtures.json```

- Но для данного проекта можно просто запустить скрипт загрузки данных:

```sudo docker-compose exec backend python manage.py loader```

### Некоторые полезные команды:

- Локально создать образ с нужным названием и тегом:

```docker build -t <username>/<imagename>:<tag> .```

- Авторизоваться через консоль:
```sudo docker login```
- А можно сразу указать имя пользователя
```sudo docker login -u <username>```
- Загрузить образ на DockerHub:
```sudo docker push <username>/<imagename>:<tag>```

- Проверить файлы в корне проекта:

```sudo docker-compose exec backend ls -a```

- Остановка всех контейнеров:

```sudo docker-compose down```

- Мониторинг запущенных контейнеров:

```sudo docker stats```

- Команда покажет, сколько места на диске занимают образы, контейнеры, тома и билд-кеш.

```sudo docker system df```


- Останавливаем и удаляем контейнеры, сети, тома вместе со всеми зависимостями. Осталются только образы:

```sudo docker-compose down -v```

- Остановить проект сохранив данные в БД:

```sudo docker-compose down```

- Остановить проект удалив данные в БД:

```sudo docker-compose down --volumes```

- Все неактивные (остановленные) контейнеры удаляются командой:

```docker container prune```

- Можно удалить образы, какие использовались как промежуточные для сборки других образов, но на которые не ссылается ни один контейнер:

```docker image prune```

- Удалить всё, что не используется (неиспользуемые образы, остановленные контейнеры, тома, которые не использует ни один контейнер, билд-кеш)

```sudo docker system prune```

```sudo docker system prune -a```

```sudo docker system prune -a -f```

- Проверить логи контейнера, если возникли проблемы:

```
sudo docker logs --tail 50 --follow --timestamps <your_container_name>
```

- Логи можно сохранить в файл командой: 

```sudo docker logs <container_name> > docker.log```
- или найти в них нужную информацию: 

```grep <поисковый-запрос>```

- Зайти внутрь контейнера:

```sudo docker exec -it <your_container_name> bash```

_Документация: Примеры обращений к эндпоинтам находятся по адресу:_

*http://fooodgram.hopto.org/api/docs/* -->
