# unik_2
# Лабораторные работы по программированию: структура и реализация
 
 ## Темы лабораторных работ
 
 1. Задача коммивояжёра
 2. Шифрование данных
 3. Нечеткий поиск
 4. Бинаризация изображений
 5. Брутфорс (метод перебора)
 6. Парсинг сайта и построение графа
 
 ## Этапы реализации каждой лабораторной работы
 
 2. Базовая реализация с использованием HTTP-протоколов (GET, POST)
 3. Интеграция веб-сокетов для real-time взаимодействия
 4. Внедрение Redis и Celery для асинхронного выполнения задач
 
 ## Общие требования к проектам
 
 - Использование FastAPI в качестве веб-фреймворка
 - SQLite как база данных
 - SQLAlchemy для ORM (Object-Relational Mapping)
 - Реализация авторизации по email и паролю
 - Аутентификация с использованием JWT (JSON Web Tokens)
 
 ## Структура проекта
 
 project/<br>
 ├── app/<br>
 │   ├── api/           # эндпоинты<br>
 │   ├── core/          # config <br>
 │   ├── db/            # файл базы данных и сессия подключения <br>
 │   ├── models/        # модели для базы данных<br>
 │   ├── cruds/         # ORM CRUD операции<br>
 │   ├── schemas/       # схемы запросов <br>
 │   └── services/      # доп. сервисы, в нашем случае тут будет лежать логика <br>
 ├── main.py<br>
 ├── alembic/<br>
 └── .env<br>
 
 ## Базовые эндпоинты
 
 ### Регистрация нового пользователя
 @router.post("/sign-up/")
 Проверяет, не зарегистрирован ли уже пользователь с таким email.
 Если нет, создает нового пользователя и генерирует для него токен.
 Возвращает данные созданного пользователя.
 
 Пример запроса:<br>
 {<br>
 "email": "user@example.com",<br>
 "password": "securepassword123"<br>
 }<br>
 Пример ответа:<br>
 {<br>
 "id": 1,<br>
 "email": "user@example.com",<br>
 "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."<br>
 }
 
 ### Вход в систему
 @router.post("/login/")
 Проверяет существование пользователя с указанным email.
 Проверяет правильность введенного пароля.
 Если все верно, генерирует новый токен для пользователя.
 Возвращает данные пользователя с новым токеном.
 
 Пример запроса:<br>
 {<br>
 "email": "user@example.com",<br>
 "password": "securepassword123"<br>
 }<br>
 Пример ответа:<br>
 {<br>
 "id": 1,<br>
 "email": "user@example.com",<br>
 "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."<br>
 }
 
 
 ### Получение информации о текущем пользователе
 @router.get("/users/me/")
 Возвращает данные авторизованного пользователя.
 
 Пример ответа:<br>
 {<br>
 "id": 1,<br>
 "email": "user@example.com"<br>
 }
 
 Данные о пользователе должны сохраняться в базу данных для чего вам надо написать CRUD'ы.
 Необходимо пользоваться alembic для миграции базы данных.

 ## Вариант 3. Нечеткий поиск (Этап 1)
 Алгоритмы нечеткого поиска (также известного как поиск по сходству или fuzzy string search) являются основой систем проверки орфографии и полноценных поисковых систем вроде Google или Yandex. Например, такие алгоритмы используются для функций наподобие «Возможно вы имели в виду …» в тех же поисковых системах.
 [К ознакомлению](https://habr.com/ru/articles/114997/)
 
 Выберите и реализуйте 2 алгоритма нечеткого поиска из списка:
 - Расстояние Левенштейна
 - Расстояние Дамерау-Левенштейна
 - Алгоритм Bitap с модификациями от Wu и Manber
 - Алгоритм расширения выборки
 - Метод N-грамм
 - Хеширование по сигнатуре
 - BK-деревья
 
 ### Эндпоинты:
 
 @app.post("/upload_corpus")<br>
 Загружает корпус текста для индексации и поиска.
 
 Пример запроса:<br>
 {<br>
 "corpus_name": "example_corpus",<br>
 "text": "This is a sample text for the corpus."<br>
 }<br>
 Пример ответа:<br>
 {<br>
 "corpus_id": 1,<br>
 "message": "Corpus uploaded successfully"<br>
 }
 
 
 @app.get("/corpuses")<br>
 Возвращает список корпусов c идентификаторами.
 
 Пример ответа:<br>
 {<br>
 "corpuses": [<br>
   {"id": 1, "name": "example_corpus"},<br>
   {"id": 2, "name": "another_corpus"}<br>
 ]<br>
 }<br>
 
 @app.post("/search_algorithm")<br>
 Позволяет указать слово (для поиска), тип алгоритма (которым можно искать), корпус (который можно использовать) и возвращает время работы алгоритма + результат поиска.
 
 Пример запроса:<br>
 {<br>
 "word": "example",<br>
 "algorithm": "levenshtein",<br>
 "corpus_id": 1<br>
 }<br>
 Пример ответа:<br>
 {<br>
 "execution_time": 0.0023,<br>
 "results": [<br>
   {"word": "example", "distance": 0},<br>
   {"word": "sample", "distance": 2}<br>
 ]<br>
 }
