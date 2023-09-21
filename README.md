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
#### Тесты: 
Запускаются тесты для вашего проекта.
#### Сборка образов: 
Git Action создает Docker-образы вашего приложения.
#### Деплой: 
Образы отправляются на ваш репозиторий DockerHub(https://hub.docker.com/), проект деплоится на сервер. 
#### Уведомление: 
В случае успеха вы получите уведомление в Telegram.(Сообщение следующего рода: Деплой Kittygram успешно выполнен!)

Перейдите в GitHub в настройки репозитория Kittygram — Settings, найдите на панели слева пункт Security -->  Secrets and Variables, перейдите в Actions, нажмите на кнопку "New repository secret".
Создайте следующие ключи:
```
DOCKER_PASSWORD (Ваш пароль в DockerHub https://hub.docker.com/)
DOCKER_USERNAME (Ваш логин в DockerHub https://hub.docker.com/)
USER (Логин вашего удалённого сервера)
HOST (IP адресс вашего удалённого сервера)
SSH_KEY (SSH ключ вашего удалённого сервера)
SSH_PASSPHRASE (Пароль вашего удалённого сервера)
TELEGRAM_TO (Ваш ID пользователя в Telegram, он же CHAT_ID)
TELEGRAM_TOKEN (Токен вашего создангого бота в Telegram)
```
Подготовьте свой удалённый сервер к публикации проекта Kittygram. Очистите диск сервера от лишних данных:
удалите кеш npm, удалите кеш APT, удалите старые системные логи, выполните поочередно следующие команды:
```
npm cache clean --force;
sudo apt-get clean;
sudo journalctl --vacuum-time=1d
```
Подключитесь к вашему удалённому серверу любым удобным способом. Создайте директорию kittygram/ в домашней директории сервера:
```
mkdir kittygram
```
Перейдите в созданную директорию cd kittygram и создайте в ней файл .env, затем добавьте строки, содержащиеся в файле .env.example и подставьте свои значения.
```
nano .env
```
Пример из .env файла:
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
При подключении к вашему удалённому серверу воркер GitHub Actions создаст БД и запустит контейнер с backend'ом, используя эти константы.

## Настройка и запуск deploy.
В локальном проекте замените в файле docker-compose.production.yml названия образов в соответствии с вашим логином на DockerHub в нижнем регистре (Например your_name/kittygram_backend).
Также в директории /.github/workflows/ файл main.yml необходимо изменить названия образов.
Сделайте коммит с файлом main.yml и со всеми остальными изменёнными файлами. Как только вы инициируете коммит и отправите изменения на GitHub, GitHub Action начнет свою работу:
```
git add .
```
```
git commit -m 'Add Actions'
```
```
git push
```
После пуша workflow должен сработать. Доверяй, но проверяй: надо удостовериться, что всё прошло по плану. В своём репозитории на GitHub(https://github.com/) во вкладку Actions. После окончания работы воркера в ваш Telegram придёт сообщение от бота:

Деплой Kittygram успешно выполнен!

После этого можно вернуться на Ваш удалённый сервер, в директорию kittygram и создать суперпользователя:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```
В конце вы можете получить доступ к сайту по его доменному имени. Все работает, браво!

## Автор проекта: 
## Павленко Дмитрий
- [Dmitry Pavlenko](https://github.com/DPavlen)
