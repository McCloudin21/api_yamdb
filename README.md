# Api_YaMDb
в разработке проекта принимали участие:
[Ксения Анемподистова](https://github.com/Brittmary)  

[Сергей Андреев](https://github.com/McCloudin21)  

[Сергей Бразгун](https://github.com/sergeybrazgun/)

### Применены следующие технологии:
- Python 3.9
- requests
- Django
- djangorestframework
- PyJWT
- djangorestframework-simplejwt
- Python-dotenv
- SQLite
- Pytest

# Как запустить проект в dev-режиме
### Клонировать репозиторий и перейти в него в командной строке:
`git clone https://github.com/sergeybrazgun/api_yamdb`  

`cd api_yamdb`

### Создать и активировать виртуальное окружение
для Linux или MacOS:
`python3 -m venv venv`  

`source venv/bin/activate`

для Windows:
`python -m venv venv`  

`venv/Scripts/activate.bat`

Установить зависимости из файла requirements.txt:
`pip install -r requirements.txt`
Перейти в папку api_yamdb, и выполнить миграции:
`cd api_yamdb`  

`python manage.py migrate`
Запустить проект:
`python manage.py runserver`

после запуска проекта, по адресу http://127.0.0.1:8000/redoc/ будет доступна документация для API Yatube.В ней описаны возможные запросы к API и структура ожидаемых ответов. 


# Описание проекта
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором.

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку. В каждой категории есть произведения: книги, фильмы или музыка.

Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

Пользовательские роли
Аноним — может просматривать описания произведений, читать отзывы и комментарии.

Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.

Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.

Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

Самостоятельная регистрация новых пользователей
Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).
Создание пользователя администратором
Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации).
В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.
