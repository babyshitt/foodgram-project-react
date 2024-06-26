# Сайт Foodgram "Продуктовый помощник"

![example workflow](https://github.com/babyshitt/foodgram-project-react/actions/workflows/foodgram_workflow.yml/badge.svg)

## _Онлайн-сервис для публикации рецептов_

### Описание проекта Foodgram

На этом сервисе пользователи смогут публиковать рецепты,
подписываться на публикации других пользователей,
добавлять понравившиеся рецепты в список «Избранное»,
а перед походом в магазин скачивать сводный список продуктов,
необходимых для приготовления одного или нескольких выбранных блюд.
На сайте доступна система регистрации и авторизации пользователей. 
Неавторизованным пользователям доступен просмотр рецептов на главной странице 
с фильтрацией по тегам, страниц отдельных рецептов и страниц других 
пользователей.
Фронтенд и бекенд взаимодействуют через API.
Проект запускается в трёх Docker-контейнерах (nginx, PostgreSQL и Django) 
через docker-compose. Четвертый контейнер (frontend) используется лишь для 
подготовки файлов.

### Технологии:

Python, Django, Django Rest Framework, Docker, Gunicorn, NGINX, PostgreSQL, Yandex Cloud, Continuous Integration, Continuous Deployment

- Клонировать репозиторий:
```
https://github.com/babyshitt/foodgram-project-react.git
```

- Установить на сервере Docker, Docker Compose:

```
sudo apt install curl                                   # установка утилиты для скачивания файлов
curl -fsSL https://get.docker.com -o get-docker.sh      # скачать скрипт для установки
sh get-docker.sh                                        # запуск скрипта
sudo apt-get install docker-compose-plugin              # последняя версия docker compose
```

- Скопировать на сервер файлы docker-compose.yml, nginx.conf из папки infra (команды выполнять находясь в папке infra):

```
scp docker-compose.yml nginx.conf username@IP:/home/username/   # username - имя пользователя на сервере
                                                                # IP - публичный IP сервера
```

- Для работы с GitHub Actions необходимо в репозитории в разделе Secrets > Actions создать переменные окружения:
```
SECRET_KEY              # секретный ключ Django проекта
DOCKER_PASSWORD         # пароль от Docker Hub
DOCKER_USERNAME         # логин Docker Hub
HOST                    # публичный IP сервера
USER                    # имя пользователя на сервере
PASSPHRASE              # *если ssh-ключ защищен паролем
SSH_KEY                 # приватный ssh-ключ
TELEGRAM_TO             # ID телеграм-аккаунта для посылки сообщения
TELEGRAM_TOKEN          # токен бота, посылающего сообщение

DB_ENGINE               # django.db.backends.postgresql
DB_NAME                 # postgres
POSTGRES_USER           # postgres
POSTGRES_PASSWORD       # postgres
DB_HOST                 # db
DB_PORT                 # 5432 (порт по умолчанию)
```

- Создать и запустить контейнеры Docker, выполнить команду на сервере
*(версии команд "docker compose" или "docker-compose" отличаются в зависимости от установленной версии Docker Compose):*
```
sudo docker compose up -d
```

- После успешной сборки выполнить миграции:
```
sudo docker-compose exec backend python manage.py migrate
```

- Создать суперпользователя:
```
sudo docker-compose exec backend python manage.py createsuperuser
```

- Собрать статику:
```
sudo docker-compose exec backend python manage.py collectstatic --noinput
```

- Наполнить базу данных содержимым из файла ingredients.json:
```
sudo docker-compose exec backend python manage.py loaddata database.json
```

- Для остановки контейнеров Docker:
```
sudo docker-compose down -v      # с их удалением
sudo docker-compose stop         # без удаления
```

### После каждого обновления репозитория (push в ветку master) будет происходить:

1. Проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8)
2. Сборка и доставка докер-образов frontend и backend на Docker Hub
3. Разворачивание проекта на удаленном сервере
4. Отправка сообщения в Telegram в случае успеха

### Запуск проекта на локальной машине:

- Клонировать репозиторий:
```
https://github.com/babyshitt/foodgram-project-react.git
```

- В директории infra файл example.env переименовать в .env и заполнить своими данными:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY='секретный ключ Django'
```

- Создать и запустить контейнеры Docker, последовательно выполнить команды по созданию миграций, сбору статики, 
созданию суперпользователя, как указано выше.
```
sudo docker-compose -f docker-compose-local.yml up -d


- После запуска проект будут доступен по адресу: [http://localhost/](http://localhost/)


- Документация будет доступна по адресу: [http://localhost/api/docs/](http://localhost/api/docs/)


### Автор backend'а: [Андрей Тарасов ](https://github.com/babyshitt)

# Проект можно посмотреть по ссылке
http://51.250.80.224/
