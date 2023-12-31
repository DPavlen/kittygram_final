#  Kittygram - социальная сеть для обмена фотографиями любимых питомцев.

## Технологии:
***
**-Nginx/1.18.0**

**-Django==3.2.3**

**-djangorestframework==3.12.4**

**-djoser==2.1.0**

**-gunicorn==20.1.0**

**-psycopg2-binary==2.9.3**

**-Pillow==9.0.0**

**-pytest==6.2.4**

**-pytest-django==4.4.0**

**-pytest-pythonpath==0.7.3**

**-PyYAML==6.0** 

**-webcolors==1.11.1**

**-python-dotenv==1.0.0**
***


## Функционал проекта:
***
Социальная сеть Kittygram разрешает пользователей делиться своими фотографиями котиков, а также просматривать фотографии котиков других пользователей.

При загрузке фото котика пользователь должен ввести в специальные поля: "Имя кота", "Год рождения" и необязательное поле: "Достижения".

Добавлять и просматривать фотографии могут только авторизованные и зарегистрированные пользователи.

Доступность только авторам изменять свои фотографии и описание.
***

## Запуск проекта в Docker контейнерах с помощью Docker Compose

Склонируйте проект из репозитория:
```
git clone git@github.com:DPavlen/kittygram_final.git
```
Перейдите в директорию проекта kittygram_final:
```
cd kittygram_final/
```
Создайте файл .env для PostgreSQL в корне проекта и контейнера backend, впишите в него переменные для инициализации БД и связи с ней. Затем добавьте строки, содержащиеся в файле .env.example и подставьте свои значения.
Пример из файла с расширением .env:
```
# Мы используем СУБД PostgreSQL, необходимо заполнить следующие константы.
POSTGRES_USER=your_django_user
POSTGRES_PASSWORD=your_password
POSTGRES_DB=db_name
# Добавляем переменные для Django-проекта:
DB_HOST=db
DB_PORT=port_for_db  # Default is 5432
# Настройки настройки переменных settings
SECRET_KEY=DJANGO_SECRET_KEY  # Your django secret key 'django-insecure......'
DEBUG=True # Set to True if you do need Debug.
ALLOWED_HOSTS=127.0.0.1 # localhost by default if DEBUG=False
```
Запустите Docker Compose с этой конфигурацией на своём компьютере. Название файла конфигурации надо указать явным образом, ведь оно отличается от дефолтного. Имя файла указывается после ключа -f:
```
docker compose -f docker-compose.production.yml up
```
Команда описанная выше, сбилдит Docker образы и запустит backend, frontend, СУБД и Nginx в отдельных Docker контей.
Выполните миграции в контейнере с backend и необходимо собрать статику backend'a, поочередно выполните 2 команды:
```
sudo docker compose -f docker-compose.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.yml exec backend python manage.py collectstatic
```
Переместите собранную статику в volume(Данные можно сохранить отдельно от контейнера: для этого придумали Docker volume), 
созданный Docker Compose для хранения статики:
```
sudo docker compose -f docker-compose.yml exec backend cp -r /app/collected_static/. /static/static/
```
По завершении всех операции проект будет запущен и доступен по адресу:
```
http://127.0.0.1/
```
Останавливает все сервисы, связанные с определённой конфигурацией Docker Compose. 
Для остановки Docker контейнеров выполните следующую команду в корне проекта:
```
sudo docker compose -f docker-compose.yml down
```

## Автор проекта: 
**Павленко Дмитрий**
- Ссылка на мой профиль в GitHub [Dmitry Pavlenko](https://github.com/DPavlen)
