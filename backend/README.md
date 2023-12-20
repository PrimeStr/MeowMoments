## Запуск backend сервера

Перейдите в директорию проекта:

```shell
cd MeowMoments/backend/
```

Создайте виртуальное окружение в папке **backend**:

>###### Если у вас OS Linux или MacOS
>```shell
>python3 -m venv venv
>```
>###### Если у вас OS Windows
>```shell
>python -m venv venv
>```

Активируйте виртуальное окружение:  

>#### `source` можно заменить на `.`
>   
>###### Если у вас OS Linux или MacOS
>```shell
>source venv/bin/activate
>```
>###### Если у вас OS Windows и вы используете cmd или PowerShell
>```shell
>.\venv\Scripts\activate.ps1
>```
>###### Если у вас OS Windows и вы используете Bash
>```shell
>source venv/Scripts/activate
>```

Установите зависимости:

```shell
pip install -r requirements.txt
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

***

>Примечание. Если вы хотите использовать SQLite вместо PostgreSQL - измените
следующие настройки подключения к БД в файле **settings.py**:

Данный код в файле:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('POSTGRES_DB', 'django'),
        'USER': os.getenv('POSTGRES_USER', 'django_user'),
        'PASSWORD': os.getenv('POSTGRES_PASSWORD', ''),
        'HOST': os.getenv('DB_HOST', 'db'),
        'PORT': os.getenv('DB_PORT', '5432')
    }
}
```

Замените на следующий:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

***

Удостоверьтесь что СУБД запущена **(при использовании PostgreSQL)**
и выполните миграции на уровне проекта:

```shell
python manage.py migrate
```

Создайте суперпользователя:  

```shell
python manage.py createsuperuser
```

Запустите backend локально:

```shell
python manage.py runserver
```

### Сервер будет доступен по адресу:
http://127.0.0.1:8000
