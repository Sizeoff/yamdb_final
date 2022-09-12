# Описание
## Проект "YaMDb"


![example workflow](https://github.com/Sizeoff/yamdb_final/actions/workflows/yamdb_workflow.yml.yml/badge.svg)

Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории: «Книги», «Фильмы»,
«Музыка». Список категорий может быть расширен администратором (например, можно добавить категорию 
«Изобразительное искусство» или «Ювелирка»).

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть
произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча»
группы «Насекомые» и вторая сюита Баха.

Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»).
Новые жанры может создавать только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку
в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка
произведения — рейтинг (целое число).  На одно произведение пользователь может оставить только один
отзыв.

## Пользовательские роли:
- Аноним — может просматривать описания произведений, читать отзывы и комментарии.

- Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить
оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои
отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому 
новому пользователю.

- Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и 
редактировать любые отзывы и комментарии.

- Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять
произведения, категории и жанры. Может назначать роли пользователям.

## Алгоритм регистрации пользователей:

### Самостоятельная регистрация новых пользователей:
1. Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
2. Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/,
в ответе на запрос ему приходит token (JWT-токен).
4. При утере confirmation_code пользователь может отправить POST-запрос с параметрами username и email на 
эндпоинт /api/v1/auth/code/.

После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/
и заполнить поля в своём профайле.

### Создание пользователя администратором
Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт
api/v1/users/. В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.

После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/,
в ответ ему должно прийти письмо с кодом подтверждения.

Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт
/api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

# Установка и запуск проекта:
Клонировать репозиторий и перейти в него в командной строке:
```bash
https://github.com/Sizeoff/api_yamdb.git
```
```bash
cd api_yamdb
```
Создать и активировать виртуальное окружение:
```bash
python -m venv venv
```
```bash
source venv/Scripts/activate
```
Установить зависимости из файла requirements.txt
```bash
python -m pip install --upgrade pip
```
```bash
pip install -r requirements.txt
```
Выполнить миграции (из директории с файлом manage.py):
```bash
python manage.py migrate
```
Запустить проект:
```bash
python manage.py runserver
```

# Примеры:

GET-Запрос на
```
http://127.0.0.1:8000/api/v1/
```
Ответ:
```json
{
    "users": "http://127.0.0.1:8000/api/v1/users/",
    "categories": "http://127.0.0.1:8000/api/v1/categories/",
    "genres": "http://127.0.0.1:8000/api/v1/genres/",
    "titles": "http://127.0.0.1:8000/api/v1/titles/"
}
```
GET-Запрос на
```
http://127.0.0.1:8000/api/v1/users/
```
Ответ:
```json
{
    "count": 123,
    "next": "http://127.0.0.1:8000/api/v1/users/?page=21",
    "previous": "http://127.0.0.1:8000/api/v1/users/?page=19",
    "results": [
        {
            "username": "string",
            "email": "user@example.ru",
            "first_name": "string",
            "last_name": "string",
            "bio": "string",
            "role": "user"
        }
    ]
}
```
POST-Запрос на 
```
http://127.0.0.1:8000/api/v1/users/
```
```json
{
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string",
    "role": "user"
}
```
Ответ:
```json
{
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string",
    "role": "user"
}
```
GET-Запрос на 
```
http://127.0.0.1:8000/api/v1/users/{username}/
```
Ответ:
```json
{
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string",
    "role": "user"
}
```
PATCH-Запрос на 
```
http://127.0.0.1:8000/api/v1/users/me/
```
```json
{
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string"
}
```
Ответ:
```json
{
    "username": "string",
    "email": "user@example.com",
    "first_name": "string",
    "last_name": "string",
    "bio": "string",
    "role": "user"
}
```


### Шаблон заполнения окружения .env
Файл .env выглядит состоит из следующих полей:

DB_ENGINE=<...> # указываем, что работаем с postgresql, в данном случае django.db.backends.postgresql
DB_NAME=<...> # имя базы данных, например "postgres"
POSTGRES_USER=<...> # логин для подключения к базе данных, например "postgres"
POSTGRES_PASSWORD=<...> # пароль для подключения к БД (установите свой), например "postgres"
DB_HOST=<...> # название сервиса (контейнера), например db
DB_PORT=<...> # порт для подключения к БД, например 5432

# Установка и запуск проекта:
Клонировать репозиторий: https://github.com/Sizeoff/infra_sp2.git


Если у вас не пустая БД и вы хотите перенести эти данные в контейнер, перейдите в 
директорию с файлом manage.py и выполните команду: python manage.py dumpdata > fixtures.json 


у вас появится файл резервной копии БД.

Его необходимо поместить в директорию, где расположен файл Dockerfile. В данном случае это .\infra_sp2\api_yamdb
 

В папке, где расположен файл docker-compose выполнить команду:
docker-compose up -d --build 



Запустите контейнеры.


python manage.py migrate


python manage.py createsuperuser


python manage.py collectstatic --no-input 


python manage.py loaddata fixtures.json 


Проект готов к работе.


Автор: Владислав Сизов