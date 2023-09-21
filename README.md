#  Kittygram - социальная сеть для обмена фотографиями любимых питомцев.

##Технологии:
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


##Функционал проекта:
***
**Социальная сеть Kittygram разрешает пользователей делиться своими фотографиями котиков, а также просматривать фотографии котиков других пользователей.**
**При загрузке фото котика пользователь должен ввести в специальные поля: "Имя кота", "Год рождения" и необязательное поле: "Достижения".**
**Добавлять и просматривать фотографии могут только авторизованные и зарегистрированные пользователи.**
**Доступность только авторам изменять свои фотографии и описание.**
***

Настроить запуск проекта Kittygram в контейнерах и CI/CD с помощью GitHub Actions

## Как проверить работу с помощью автотестов

В корне репозитория создайте файл tests.yml со следующим содержимым:
```yaml
repo_owner: ваш_логин_на_гитхабе
kittygram_domain: полная ссылка (https://доменное_имя) на ваш проект Kittygram
taski_domain: полная ссылка (https://доменное_имя) на ваш проект Taski
dockerhub_username: ваш_логин_на_докерхабе
```

Скопируйте содержимое файла `.github/workflows/main.yml` в файл `kittygram_workflow.yml` в корневой директории проекта.

Для локального запуска тестов создайте виртуальное окружение, установите в него зависимости из backend/requirements.txt и запустите в корневой директории проекта `pytest`.

## Чек-лист для проверки перед отправкой задания

- Проект Taski доступен по доменному имени, указанному в `tests.yml`.
- Проект Kittygram доступен по доменному имени, указанному в `tests.yml`.
- Пуш в ветку main запускает тестирование и деплой Kittygram, а после успешного деплоя вам приходит сообщение в телеграм.
- В корне проекта есть файл `kittygram_workflow.yml`.
