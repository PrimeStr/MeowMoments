# Kittygram - простой проект, чтобы делиться фотографиями котиков
***
***


## Технологии:

[![Python](https://img.shields.io/badge/Python-%203.10-blue?style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/Django-%203.2.3-blue?style=flat-square&logo=django)](https://www.djangoproject.com/)
[![DRF](https://img.shields.io/badge/DjangoRESTFramework-%20-blue?style=flat-square&logo=django)](https://www.django-rest-framework.org/)
[![Docker](https://img.shields.io/badge/Docker-%2024.0.5-blue?style=flat-square&logo=docker)](https://www.docker.com/)
[![DockerCompose](https://img.shields.io/badge/Docker_Compose-%202.21.0-blue?style=flat-square&logo=docsdotrs)](https://docs.docker.com/compose/)
[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-%20-blue?style=flat-square&logo=githubactions)](https://github.com/features/actions)
[![Gunicorn](https://img.shields.io/badge/Gunicorn-%2020.1.0-blue?style=flat-square&logo=gunicorn)](https://gunicorn.org/)
[![Nginx](https://img.shields.io/badge/Nginx-%20-blue?style=flat-square&logo=nginx)](https://www.nginx.com/)
[![React](https://img.shields.io/badge/React-%20-blue?style=flat-square&logo=react)](https://react.dev/)
[![React](https://img.shields.io/badge/certbot-%20-blue?style=flat-square&logo=letsencrypt)](https://certbot.eff.org/)

***

## Функционал:

- Проект Kittygram позволяет пользователям делиться своими фотографиями
котиков и просматривать фотографии котиков других пользователей.
- При загрузке фото котика пользователь должен ввести в специальные поля имя
котика, год его рождения и, при желании, может добавить ему достижения.
- Добавлять и просматривать фотографии могут только зарегистрированные и
авторизованные пользователи.
- Только авторы могут изменять свои фотографии и описание.

***

## Технические особенности:

Репозиторий включает в себя два файла **docker-compose.yml** и 
**docker-compose.production.yml**, что позволяет развернуть проект как на
локальном или удалённом серверах.

Данная инструкция подразумевает, что на вашем локальном сервере уже установлен Git, Python 3.9, пакетные менеджеры npm и pip, Docker, Docker Compose, утилита виртуального окружения python3-venv.

Для ручного локального запуска в ручном режиме настроена и запущена СУБД PostgreSQL.

С подробными инструкциями запуска вы можете ознакомиться ниже.

***

## Запуск проекта локально в ручном режиме

Полные инструкции по запуску в ручном режиме серверов **frontend** и
**backend** вы можете найти в соответствующих папках репозитория в 
файлах README.md

*kittygram_final/frontend/README.md* \
*kittygram_final/backend/README.md*

## Запуск проекта в Docker контейнерах с помощью Docker Compose

Склонируйте проект из репозитория:

```shell
git clone https://github.com/PrimeStr/kittygram_final.git
```


Перейдите в директорию проекта:

```shell
cd kittygram_final/
```

Создайте файл **.env** в корне проекта, затем добавьте строки,
содержащиеся в файле **.env.example** и подставьте свои значения.

Пример из .env файла:

```dotenv
# Если вы будете использовать СУБД PostgreSQL - заполните следующие константы.
POSTGRES_USER=your_username        # Пользователь Postgres
POSTGRES_PASSWORD=your_password    # Пароль пользователя Postgres
POSTGRES_DB=db_name                # Название БД
DB_HOST=db                         # Хост, по дефолту db
DB_PORT=port_for_db                # Порт, по дефолту 5432

# Следующие константы необходимо заполнить независимо от выбора СУБД.
SECRET_KEY=DJANGO_SECRET_KEY       # Секретный ключ Джанго
DEBUG=False                        # Выставьте True, если вам нужно включить дебаг. Оставьте пустым если не нужен.
ALLOWED_HOSTS=127.0.0.1            # 127.0.0.1 localhost по дефолту если DEBUG=False. Разделите адреса пробелами.
```

В корне проекта находится файл docker-compose.yml, с помощью которого вы можете
запустить проект локально в Docker контейнерах.

Находясь в корне проекта выполните следующую команду:
> **Примечание.** Если нужно - добавьте в конец команды флаг **-d** для запуска
> в фоновом режиме.
```shell
sudo docker compose -f docker-compose.yml up
```

Она сбилдит Docker образы и запустит backend, frontend, СУБД и Nginx в отдельных
Docker контейнерах.

Выполните миграции в контейнере с backend:

```shell
sudo docker compose -f docker-compose.yml exec backend python manage.py migrate
```

Соберите статику backend'a:

```shell
sudo docker compose -f docker-compose.yml exec backend python manage.py collectstatic
```

Переместите собранную статику в volume, созданный Docker Compose для хранения статики:

```shell
sudo docker compose -f docker-compose.yml exec backend cp -r /app/collected_static/. /static/static/
```

По завершении всех операции проект будет запущен и доступен по адресу
http://127.0.0.1/

Для остановки Docker контейнеров выполните следующую команду в корне проекта:

```shell
sudo docker compose -f docker-compose.yml down
```

Либо просто завершите работу Docker Compose в терминале, в котором вы его
запускали, сочетанием клавиш **CTRL+C**.

***

# CI/CD - Развёртка проекта на удаленном сервере

В проекте уже настроен Workflow для GitHub Actions.

Ваш GitHub Actions самостоятельно запустит:

- 🧪 Тесты: Запускаются тесты для вашего проекта.
- 🏗️ Сборка образов: Git Action создает Docker-образы вашего приложения.
- 🚀 Деплой: Образы отправляются на ваш репозиторий DockerHub, проект
деплоится на сервер.
- ✉️ Уведомление: В случае успеха вы получите уведомление в Telegram.

Перейдите в GitHub в настройки репозитория — **Settings**, найдите на панели слева пункт
**Secrets and Variables**, перейдите в **Actions**, нажмите
**New repository secret**.

Создайте следующие ключи:

```
DOCKER_USERNAME (Ваш логин в DockerHub)
DOCKER_PASSWORD (Ваш пароль в DockerHub)
HOST (IP адресс вашего удалённого сервера)
USER (Логин вашего удалённого сервера)
SSH_KEY (SSH ключ вашего удалённого сервера)
SSH_PASSPHRASE (Пароль вашего удалённого сервера)
TELEGRAM_TO (Ваш ID пользователя в Telegram)
TELEGRAM_TOKEN (Токен вашего бота в Telegram)
```

Подключитесь к вашему удалённому серверу любым удобным способом. Создайте в
домашней директории директорию с названием **kittygram**.

```shell
mkdir kittygram
```

Перейдите в созданную директорию и создайте в ней файл **.env**, затем добавьте
строки, содержащиеся в файле **.env.example** и подставьте свои значения.

```shell
nano .env
```

Пример из .env файла:

```dotenv
# По этим константам будет создана база данных для проекта.
POSTGRES_USER=your_username        # Пользователь Postgres
POSTGRES_PASSWORD=your_password    # Пароль пользователя Postgres
POSTGRES_DB=db_name                # Название БД
DB_HOST=db                         # Хост, по дефолту db
DB_PORT=port_for_db                # Порт, по дефолту 5432

SECRET_KEY=DJANGO_SECRET_KEY       # Секретный ключ Джанго
DEBUG=False                        # Выставьте True, если вам нужно включить дебаг. Оставьте пустым если не нужен.
ALLOWED_HOSTS=127.0.0.1            # 127.0.0.1 localhost по дефолту если DEBUG=False. Разделите адреса пробелами.
```

При подключении к вашему удалённому серверу воркер GitHub Actions создаст БД и
запустит контейнер с backend'ом, используя эти константы.

### **Теперь можно приступать к деплою**

В локальном проекте замените в файле **docker-compose.production.yml** названия
образов в соответствии с вашим логином на DockerHub в нижнем регистре
(Например **your_name/kittygram_backend**)

Аналогично измените названия образов и в файле **main.yml**, который находится
в директории **/.github/workflows/**.

Весь процесс автоматизирован! Как только вы инициируете коммит и отправите
изменения на GitHub, GitHub Action берет дело в свои руки:

```shell
git add .
```

```shell
git commit -m "Ваше сообщение для коммита."
```

```shell
git push
```

После успешной отправки изменений перейдите в своём репозитории на GitHub во
вкладку **Actions**. Вы увидите процесс работы Actions. После окончания работы
воркера в ваш Telegram придёт сообщение от бота:

> Деплой Kittygram успешно выполнен!

После этого можно вернуться на удалённый сервер, в директорию **kittygram** и
создать суперпользователя: 

```shell
sudo docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```

>**Внимание! :** Для полноценной работы проекта на вашем удалённом сервере должен быть
установлен, настроен и запущен **Nginx**. Удостоверьтесь что в конфиге по
вашему доменному имени настроена переадресация всех запросов на
**127.0.0.1:9000**.

### Настройка SSL шифрования через Let's Encrypt

Чтобы проект отвечал базовым стандартам безопасности необходимо настроить
SSL сертификат.

Установите **certbot**:

```shell
sudo apt install snapd
```

```shell
sudo snap install core; sudo snap refresh core
```

```shell
sudo snap install --classic certbot
```

```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

Запустите sertbot и получите свой SSL сертификат:

```shell
sudo certbot --nginx
```

После перезапустите Nginx:

```shell
sudo systemctl reload nginx
```  

```shell
sudo certbot certificates
```  

> **Примечание.** SSL-сертификаты от Let's Encrypt действительны в течение 90 дней. Их нужно постоянно обновлять.
Если вы не хотите делать это самостоятельно, вы можете настроить автоматическое обновление сертификата с помощью команды ниже.

```shell
sudo certbot renew --dry-run
```  

**Теперь вы можете получить доступ к сайту по его доменному имени.**
**Как только вы убедитесь, что все работает как надо, можете приглашать пользователей.**

***

## Автор

**Максим Головин**\
Вы можете заглянуть в другие мои репозитории в моем профиле GitHub. Нажмите [здесь](https://github.com/PrimeStr).
