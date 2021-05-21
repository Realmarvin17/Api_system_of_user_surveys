# Api_system_of_user_surveys

API для системы опросов пользователей 

Документация API (автодокументирование на swagger (drf-yasg) доступно по адресу http://127.0.0.1:8000/swagger/ )
Описание ТЗ:

Функционал для администратора системы:
•	авторизация в системе (регистрация не нужна)
•	добавление/изменение/удаление опросов. Атрибуты опроса: название, дата старта, дата окончания, описание. После создания поле "дата старта" у опроса менять нельзя
•	добавление/изменение/удаление вопросов в опросе. Атрибуты вопросов: текст вопроса, тип вопроса (ответ текстом, ответ с выбором одного варианта, ответ с выбором нескольких вариантов)

Функционал для пользователей системы:
•	получение списка активных опросов
•	прохождение опроса: опросы можно проходить анонимно, в качестве идентификатора пользователя в API передаётся числовой ID, по которому сохраняются ответы пользователя на вопросы; один пользователь может участвовать в любом количестве опросов
•	получение пройденных пользователем опросов с детализацией по ответам (что выбрано) по ID уникальному пользователя

Окружение проекта:
•	python 3.8
•	Django 2.2.10
•	djangorestframework

Создать и активировать виртуальное окружение Python.

Установить зависимости из файла requirements.txt:
pip install -r requirements.txt

Выполнить следующие команды:
•	Команда для создания миграций приложения для базы данных

python manage.py makemigrations
python manage.py migrate

•	Создание суперпользователя

python manage.py createsuperuser

Username (leave blank to use 'admin'): admin
Email address: admin@admin.com
Password: ********
Password (again): ********
Superuser created successfully.

•	Команда для запуска приложения
python manage.py runserver

•	Приложение будет доступно по адресу: http://127.0.0.1:8000/
Документация API (создал автодокументирование API на swagger доступно по адресу http://127.0.0.1:8000/swagger/ )

Чтобы получить токен пользователя:
•	Request method: GET
•	URL: http://localhost:8000/api/login/
•	Body:
o	username:
o	password:

•	Example:
curl --location --request GET 'http://localhost:8000/api/login/' \
--form 'username=%username' \
--form 'password=%password'

Чтобы создать опрос:
•	Request method: POST
•	URL: http://localhost:8000/api/surveysApp/create/
•	Header:
o	Authorization: Token userToken
•	Body:
o	survey_name: name of survey
o	pub_date: publication date can be set only when survey is created, format: YYYY-MM-DD HH:MM:SS
o	end_date: survey end date, format: YYYY-MM-DD HH:MM:SS
o	survey_description: description of survey

•	Example:
curl --location --request POST 'http://localhost:8000/api/surveysApp/create/' \
--header 'Authorization: Token %userToken' \
--form 'survey_name=%survey_name' \
--form 'pub_date=%pub_date' \
--form 'end_date=%end_date \
--form 'survey_description=%survey_description'

Обновить опрос:
•	Request method: PATCH
•	URL: http://localhost:8000/api/surveysApp/update/[survey_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	survey_id
•	Body:
o	survey_name: name of survey
o	end_date: survey end date, format: YYYY-MM-DD HH:MM:SS
o	survey_description: description of survey

•	Example:
curl --location --request PATCH 'http://localhost:8000/api/surveysApp/update/[survey_id]/' \
--header 'Authorization: Token %userToken' \
--form 'survey_name=%survey_name' \
--form 'end_date=%end_date \
--form 'survey_description=%survey_description'

Удалить опрос:
•	Request method: DELETE
•	URL: http://localhost:8000/api/surveysApp/update/[survey_id]
•	Header:
o	Authorization: Token userToken
•	Param:
o	survey_id Example:
curl --location --request DELETE 'http://localhost:8000/api/surveysApp/update/[survey_id]/' \
--header 'Authorization: Token %userToken'

Посмотреть все опросы:
•	Request method: GET
•	URL: http://localhost:8000/api/surveysApp/view/
•	Header:
o	Authorization: Token userToken

•	Example:
curl --location --request GET 'http://localhost:8000/api/surveysApp/view/' \
--header 'Authorization: Token %userToken'

Просмотр текущих активных опросов:
•	Request method: GET
•	URL: http://localhost:8000/api/surveysApp/view/active/
•	Header:
o	Authorization: Token userToken

•	Example:
curl --location --request GET 'http://localhost:8000/api/surveysApp/view/active/' \
--header 'Authorization: Token %userToken'

Создаем вопрос:
•	Request method: POST
•	URL: http://localhost:8000/api/question/create/
•	Header:
o	Authorization: Token userToken
•	Body:
o	survey: id of survey
o	question_text:
o	question_type: can be only one, multiple or text

•	Example:
curl --location --request POST 'http://localhost:8000/api/question/create/' \
--header 'Authorization: Token %userToken' \
--form 'survey=%survey' \
--form 'question_text=%question_text' \
--form 'question_type=%question_type \

Обновляем вопрос:
•	Request method: PATCH
•	URL: http://localhost:8000/api/question/update/[question_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	question_id
•	Body:
o	survey: id of survey
o	question_text: question
o	question_type: can be only one, multiple or text

•	Example:
curl --location --request PATCH 'http://localhost:8000/api/question/update/[question_id]/' \
--header 'Authorization: Token %userToken' \
--form 'survey=%survey' \
--form 'question_text=%question_text' \
--form 'question_type=%question_type \

Удаляем вопрос:
•	Request method: DELETE
•	URL: http://localhost:8000/api/question/update/[question_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	question_id

•	Example:
curl --location --request DELETE 'http://localhost:8000/api/question/update/[question_id]/' \
--header 'Authorization: Token %userToken' \
--form 'survey=%survey' \
--form 'question_text=%question_text' \
--form 'question_type=%question_type \

Создаем выбор:
•	Request method: POST
•	URL: http://localhost:8000/api/choice/create/
•	Header:
o	Authorization: Token userToken
•	Body:
o	question: id of question
o	choice_text: choice

•	Example:
curl --location --request POST 'http://localhost:8000/api/choice/create/' \
--header 'Authorization: Token %userToken' \
--form 'question=%question' \
--form 'choice_text=%choice_text'
Обновляем выбор:
•	Request method: PATCH
•	URL: http://localhost:8000/api/choice/update/[choice_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	choice_id
•	Body:
o	question: id of question
o	choice_text: choice
•	Example:
curl --location --request PATCH 'http://localhost:8000/api/choice/update/[choice_id]/' \
--header 'Authorization: Token %userToken' \
--form 'question=%question' \
--form 'choice_text=%choice_text'
Обновляем выбор:
•	Request method: DELETE
•	URL: http://localhost:8000/api/choice/update/[choice_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	choice_id
•	Example:
curl --location --request DELETE 'http://localhost:8000/api/choice/update/[choice_id]/' \
--header 'Authorization: Token %userToken' \
--form 'question=%question' \
--form 'choice_text=%choice_text'
Создаем ответ:
•	Request method: POST
•	URL: http://localhost:8000/api/answer/create/
•	Header:
o	Authorization: Token userToken
•	Body:
o	survey: id of survey
o	question: id of question
o	choice: if question type is one or multiple then it’s id of choice else null
o	choice_text: if question type is text then it’s text based answer else null
•	Example:
curl --location --request POST 'http://localhost:8000/api/answer/create/' \
--header 'Authorization: Token %userToken' \
--form 'survey=%survey' \
--form 'question=%question' \
--form 'choice=%choice \
--form 'choice_text=%choice_text'
Обновляем ответ:
•	Request method: PATCH
•	URL: http://localhost:8000/api/answer/update/[answer_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	answer_id
•	Body:
o	survey: id of survey
o	question: id of question
o	choice: if question type is one or multiple then it’s id of choice else null
o	choice_text: if question type is text then it’s text based answer else null
•	Example:
curl --location --request PATCH 'http://localhost:8000/api/answer/update/[answer_id]' \
--header 'Authorization: Token %userToken' \
--form 'survey=%survey' \
--form 'question=%question' \
--form 'choice=%choice \
--form 'choice_text=%choice_text'
Удаляем ответ:
•	Request method: DELETE
•	URL: http://localhost:8000/api/answer/update/[answer_id]/
•	Header:
o	Authorization: Token userToken
•	Param:
o	answer_id
•	Example:
curl --location --request DELETE 'http://localhost:8000/api/answer/update/[answer_id]' \
--header 'Authorization: Token %userToken'
Просматриваем ответы пользователя:
•	Request method: GET
•	URL: http://localhost:8000/api/answer/view/[user_id]/
•	Param:
o	user_id
•	Header:
o	Authorization: Token userToken
•	Example:
curl --location --request GET 'http://localhost:8000/api/answer/view/[user_id]' \
--header 'Authorization: Token %userToken'

