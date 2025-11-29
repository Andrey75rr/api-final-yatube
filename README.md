# api_final
api final
# API для социальной сети Yatube

REST API для социальной сети Yatube, позволяющее взаимодействовать с платформой через HTTP-запросы. Проект предоставляет полный функционал для ведения блога: создание постов, комментирование, подписки на авторов и группировку контента.

## Описание проекта

Yatube API решает задачу предоставления программного интерфейса для социальной платформы, позволяя:
- Интегрировать социальные функции в другие приложения
- Создавать мобильные и веб-клиенты для платформы
- Автоматизировать публикацию и управление контентом
- Обеспечить безопасный доступ к данным через JWT-аутентификацию

**Польза проекта**: разработчики могут создавать собственные клиенты для Yatube, расширяя функциональность платформы и предоставляя пользователям альтернативные способы взаимодействия.

## Технологии

- Python 3.12+
- Django 5.1+
- Django REST Framework 3.15+
- Simple JWT для аутентификации
- SQLite (разработка) / PostgreSQL (продакшен)
- Django Filter для фильтрации данных

### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/Andrey75rr/api-final-yatube.git
```

```
cd api-final-yatube
```

Cоздать и активировать виртуальное окружение:

```
python3 -m venv venv
```

```
source venv/Scripts/activate
```

Установить зависимости из файла requirements.txt:
```
python3 -m pip install --upgrade pip
```

```
pip install -r requirements.txt
```

Выполнить миграции:

```
python3 manage.py migrate
```

Запустить проект:

```
python3 manage.py runserver

#### Примеры запросов к API

Аутентификация и получение токенов
Для работы с API необходимо сначала получить JWT-токен. Отправьте POST-запрос на эндпоинт /api/v1/jwt/create/ с вашими учетными данными:

```
POST /api/v1/jwt/create/
Content-Type: application/json

{
    "username": "ваше_имя_пользователя",
    "password": "ваш_пароль"
}
```

В ответ вы получите access и refresh токены:
```

{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

Access-токен используется для доступа к защищенным эндпоинтам, передавайте его в заголовке Authorization:
```

Authorization: Bearer ваш_access_токен
Работа с постами
Получение списка всех постов:
```

GET /api/v1/posts/
Authorization: Bearer ваш_токен
Получение конкретного поста:
```

```
GET /api/v1/posts/1/
Authorization: Bearer ваш_токен
```

Создание нового поста:
```

POST /api/v1/posts/
Authorization: Bearer ваш_токен
Content-Type: application/json
```

{
    "text": "Текст моего нового поста",
    "group": 1
}
```

Обновление поста (только автор):

```
PUT /api/v1/posts/1/
Authorization: Bearer ваш_токен
Content-Type: application/json
```

{
    "text": "Обновленный текст поста",
    "group": 2
}
```

Удаление поста (только автор):

```
DELETE /api/v1/posts/1/
Authorization: Bearer ваш_токен
```

Получение комментариев к посту:

```
GET /api/v1/posts/1/comments/
Authorization: Bearer ваш_токен
```

Добавление комментария:

```
POST /api/v1/posts/1/comments/
Authorization: Bearer ваш_токен
Content-Type: application/json

{
    "text": "Это мой комментарий к посту"
}
```

Редактирование комментария:

```
PUT /api/v1/posts/1/comments/1/
Authorization: Bearer ваш_токен
Content-Type: application/json

{
    "text": "Исправленный текст комментария"
}
```

```
Просмотр своих подписок:

```
GET /api/v1/follow/
Authorization: Bearer ваш_токен
```

Поиск по подпискам:

GET /api/v1/follow/?search=username
Authorization: Bearer ваш_токен

```
Подписка на автора:

POST /api/v1/follow/
Authorization: Bearer ваш_токен
Content-Type: application/json

{
    "following": "username_автора"
}
```

Получение списка групп:

GET /api/v1/groups/

```
Получение информации о группе:

GET /api/v1/groups/1/

```

Пагинация постов:

GET /api/v1/posts/?limit=10&offset=20
Authorization: Bearer ваш_токен

```
Фильтрация постов по группе:

GET /api/v1/posts/?group=1
Authorization: Bearer ваш_токен
```

Успешное создание (201):

{
    "id": 1,
    "text": "Текст поста",
    "author": "username",
    "pub_date": "2024-01-01T12:00:00Z",
    "group": 1,
    "image": null
}
```
Ошибка валидации (400):

{
    "text": ["Это поле обязательно."]
}
```

Ошибка доступа (403):

{
    "detail": "У вас недостаточно прав для выполнения данного действия."
}
```

Неавторизованный доступ (401):

{
    "detail": "Учетные данные не были предоставлены."
}