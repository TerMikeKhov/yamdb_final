# Проект yamdb_final  ![workflow](https://github.com/TerMikeKhov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Описание

### Пользовательский функционал проекта:

* Проект yamdb_final собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
* Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
* В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.
* Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.
* Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

### CI/CD

Для приложения настроено Continuous Integration и Continuous Deployment (CI/CD) на GitHub Actions.

* Настроено тестирование. При push на git проект проходит тесты pytest и проверку на соответствие PEP8 с помощью flake8. 
* После прохождения тестирования GitHub Action формируется Docker образ приложения и этот образ "пушится" на Docker Hub.
* Проект деплоится из образа с Docker Hub на удаленном сервере Yandex Cloud.
* После деплоя ответственному лицу отправляется сообщение о удачном деплое через telegram бот.

* Проект yamdb_final 

* Проект infra_sp2 упаковывает приложение api_yamdb в контейнеры Docker и настраивает взаимодействие контейнеров в Docker Compose. Так же в проекте заменена база данных на PostgreSQL и сервер разработчика заменен на взаимодействие двух серверов NGINX и Gunicorn. NGINX отвечает за раздачу статики и первичном приеме пользователей, а Gunicorn обеспечивает взаимодействие NGINX и Django. 

## Использованные технологии

* Python 3.7
* Django 2.2.16
* Django Rest Framework 3.12.4
* Docker
* Docker Compose
* Nginx
* Gunicorn
* Yandex Cloud
* GitHub Action
* Docker Hub
* PostgreSQL
* Simple-JWT 4.7.2


### Запуск/обновление проекта на удаленном сервере

Для запуска проета необходимо произвести push в ветку main.
При пуше в ветку main код автоматически проверяется, тестируется, деплоится на сервер.

### Доступ к проекту развернутому на удаленном сервере

http://178.154.221.177

### Запуск проекта локально

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/TerMikeKhov/ls
```

```
cd yamdb_final/infra/
```

Запустить рзвертывание проета в нескольких контенерах Docker путем запуска инструкций файла docker-compose.yaml

```
docker-compose up -d --build
```

Выполнить миграции
```
docker-compose exec web python manage.py migrate
```

```
docker-compose exec web python manage.py collectstatic --no-input 
```


Теперь проект доступен по адресу http://localhost/

Для остановки работы контейнеров и их удаления выполнить в командной строке:
```
docker-compose down -v
```

## Примеры

### Примеры запросов к API:

### Регистрация нового пользователя

POST запрос на адрес
http:/178.154.221.177/api/v1/auth/signup/

```
{

    "email": "string",
    "username": "string"

}
```

### Получение JWT-токена

POST запрос на адрес
http:/178.154.221.177/api/v1/auth/token/

```
{

    "username": "string",
    "confirmation_code": "string"

}
```


### Получение списка всех произведений

GET запрос на адрес
http://178.154.221.177/api/v1/titles/


Ответ API:

```
[

{

    "count": 0,
    "next": "string",
    "previous": "string",
    "results":
                        [
                        ......
                        ]
    }

]
```

### Добавление произведения

POST запрос на адрес
http://178.154.221.177/api/v1/titles/

Текст запроса:

```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```



### Более подробная документация API с примерами размещена по адресу:
http://178.154.221.177/redoc/

## Автор

* Терехов Михаил
