# praktikum_new_diplom
# Проект Foodgram
Сайт Foodgram, «Продуктовый помощник».
Онлайн-сервис и API для него. На этом сервисе пользователи после регистрации могут публиковать рецепты, подписываться на публикации других пользователей, добавлять понравившиеся рецепты в список «Избранное», а перед походом в магазин скачивать сводный список продуктов, необходимых для приготовления одного или нескольких выбранных блюд.

![example workflow](https://github.com/baciroff/foodgram-project-react/.github/workflows/foodgram.yml/badge.svg)

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

*http://fooodgram.hopto.org/api/docs/*
