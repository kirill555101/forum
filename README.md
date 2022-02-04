# Техническое задание
В качестве домашнего задания предлагается выполнить проект «Вопросы и Ответы». Этот сервис позволит пользователям Интернета задавать вопросы и получать на них ответы. Возможности комментирования и голосования формируют сообщество и позволяет пользователям активно помогать другим. В качестве образца реализации рекомендуется использовать [Stack Overflow](https://stackoverflow.com).

## Используемые технологии
- Код приложения пишется на **Python + Django**.
- Приложение запускается под управлением сервера **Gunicorn**.
- База данных – **MySQL**.
- Для отдачи статики используется **nginx**.
- Для доставки real-time сообщений **nginx + mod_push**.
- Для кеширования данных – **memcached**.
- Верстка выполняется с использованием **Twitter Bootstrap**.
- Взаимодействие интерфейса с пользователем обеспечивается **JavaScript/jQuery**.
- Для авторизации и хранения пользователей можно использовать приложение **django.contrib.auth**.

## Основные сущности
- Пользователь – электронная почта, никнейм, пароль, аватарка, дата регистрации, рейтинг.
- Вопрос – заголовок, содержание, автор, дата создания, теги, рейтинг.
- Ответ – содержание, автор, дата написания, флаг правильного ответа, рейтинг.
- Тег – слово тега.

## Основные страницы и формы
1. **Листинг вопросов** с пагинацией по 20 вопросов на странице. Необходимо реализовать сортировку по дате добавления и рейтингу (2 вида сортировки). В шапке сайта находятся: логотип, поисковая строка (для быстрого поиска по заголовку и содержимому вопроса), кнопка задать вопрос (доступна только авторизованным). В правой части шапки - юзерблок. Для авторизованного пользователя юзерблок содержит его ник, аватарку, ссылки на “выход” и на страницу с его профилем. Для неавторизованных - ссылки “войти” и “регистрация”. В правой колонке - информационные блоки “Популярные тэги” и “Лучшие пользователи” (описание ниже). Во всех листингах присутствуют кнопки “лайк/дизлайк”, позволяющие менять рейтинг вопроса.

  ![Листинг вопросов]

2. **Страница добавления вопроса** (можно сделать оверлеем). Доступна только для авторизованных пользователей. В форма вводится заголовок, текст вопроса и теги, через запятую. С вопросом может быть связано не более 3 тегов. Для подсказки при выборе тега можно использовать готовый jquery плагин. Готовые django приложения для тегов использовать запрещается. При обработке формы обязательно проверка валидности данных. Если вопрос успешно добавлен - пользователя перебрасывает на страницу вопроса, если возникли ошибки - их нужно отобразить в форме.

  ![Страница добавления вопроса]

3. **Страница вопроса со списком ответов**. На странице вопроса можно добавить ответ. Ответы сортируются по рейтингу и дате добавления при равном рейтинге. Ответы разбиваются по 30 штук на странице. Форма добавления ответа находится на странице вопроса. Отображается только для авторизованных пользователей. После добавления ответа, автор вопроса должен получить email с уведомление от новом ответе. В этом письме должна быть ссылка для перехода на страницу вопроса. Автор вопроса может пометить один из ответов как правильный. Пользователи могут голосовать за вопросы и ответы с помощью лайков «+» или «–». Один пользователь может голосовать за 1 вопрос и ответ только 1 раз, однако может отменить свой выбор или переголосовать неограниченное число раз.

  ![Страница вопроса со списком ответов]

4. **Листинг вопросов по тегу**. На этой странице выводятся все вопросы содержащие некоторый тег. Сортировка по рейтингу вопроса. Пагинация по 20 вопросов. Пользователи попадают на эту страницу кликая по одному из тегов в описании вопроса.

  ![Листинг вопросов по тэгу]

5. **Страница пользователя** содержит его настройки - email, nick и аватарку. Каждый пользователь может смотреть только свою страницу. У пользователя должна быть возможность изменить email, nick и аватарку.

  ![Страница пользователя]

6. **Форма авторизации**. Состоит из поля логин и пароль. Дополнительно есть ссылка на форму регистрации. При успешной авторизации пользователь перебрасывается на исходную страницу, при неуспешной авторизации в форме выводятся сообщения об ошибках формы. Для авторизованных пользователей вместо этой формы должна показываться кнопка “Выйти”.

  ![Форма авторизации]

7. **Страница регистрации**. Любой пользователь может зарегистрироваться на сайте, заполнив форму с электронной почтой, никнеймом, аватаркой и паролем. Аватарка загружается на сервер и отображается рядом с вопросами и ответами пользователя. При неудачной регистрации в форме необходимо выводить сообщения об ошибках.

  ![Страница регистрации]

8. **Блок популярных тегов**. В правой колонке сайте находится облако из 20 наиболее популярных тегов. Популярными считаются те теги, которые были использованы в наибольшем количестве вопросов. Генерация этого блока занимает много времени, поэтому этот блок необходимо генерировать в фоне, с помощью cron скрипта.

9. **Блок лучших пользователи** (недели). В блок лучшие пользователи попадают 10 авторов задавших самые популярные вопросы или ответы за последнюю неделю. Т.е. вопросы и ответы, созданные за последнюю неделю сортируем по рейтингу. Выбираем топ N, их авторы и будут “лучшими”. Генерация этого блока занимает много времени, поэтому блок нужно подготавливать в фоне, с помощью cron скрипта.

## Требования к проекту
1. Структура проекта должна быть понятна пользователям. Переходы по страницам осуществляются по ссылкам. Обработка форм должна осуществляться с редиректом.
2. Код проекта должен быть аккуратным и без дублирования. Наличие больших повторяющихся фрагментов кода или шаблонов могут быть причиной снижения баллов.
3. Верстка проекта должна быть выполнена с помощью css фреймворка Twitter Bootstrap.
4. Код приложения должен быть чувствителен к входным данным и выдавать соответствующие коды и тексты ошибок. Сообщение пользователям «Вопрос добавлен»,
«Вопрос не был добавлен, потому что» выводятся в оверлее. Ответ сервера с кодом 500 может быть причиной снижения баллов.
5. Время генерации страницы не должно зависеть от объема данных в базе.
6. Страницы проекты не должны отдаваться более 1 секунды.

## Требования к объему данных
- Пользователи > 10 000.
- Вопросы > 100 000.
- Ответы > 1 000 000.
- Тэги > 10 000.
- Оценки пользователей > 2 000 000.

## Дополнительные задания.
1. Real-time нотификации, уведомление о новых ответах при просмотре вопроса (20 баллов). С использованием long-polling или web sockets.

# Оценка и разбалловка
## Пороговые баллы
- 0-39 — неудовлетворительно.
- 40-59 — удовлетворительно.
- 60-79 — хорошо.
- 80-100 — отлично.

## ДЗ 1 “Статическая верстка” - 16 баллов
Верстка общего вида (layout) страницы - 4:

- общий вид: 2 колонки, header, footer - 1;
- правая колонка - 1;
- блок авторизованного юзера - 1;
- поисковая строка и логотип - 1.

Верстка листинга вопросов - 3:

- общий вид (паддинги, аватарка) - 1;
- кнопки лайков - 1;
- теги, счетчики ответов, остальное - 1.

Верстка страницы вопроса - 3:

- общий вид - 1;
- чекбокс “правильный ответ”, кнопки лайков - 1;
- форма ответа - 1.

Верстка формы добавления вопроса - 3:

- общий вид - 2;
- сообщения об ошибках - 1.

Верстка форм логина и регистрации - 3:

- общий вид - 2;
- аватарка и сообщения об ошибках - 1.

## ДЗ 2 “Обработка HTTP запросов” - 14 баллов
Создать views и шаблоны для основных страниц - 6:

- главная (список новых вопросов) - 1;
- страница вопроса (список ответов) - 1;
- страница добавления вопроса - 1;
- форма регистрации - 1;
- форма входа - 1;
- форма добавления вопроса - 1.

Создать urls.py для всех страниц - 4:

- Собственно urls.py - 2;
- Именованные маршруты (во всех шаблонах) - 2.

Постраничное отображение - 4:

- функция пагинации - 1;
- шаблон для отрисовки пагинатора - 2;
- корректная обработка “неправильных” параметров - 1.

## ДЗ 3 “Работа с базой данных” - 14 баллов
Проектирование модели - 5:

- правильные адекватные типы данных и внешние ключи - 1;
- своя модель пользователя - 1;
- таблицы тегов, лайков - 1;
- query-set'ы для типовых выборок: новые вопросы, популярные, по тегу - 2.

Наполнение базы тестовыми данными - 3:

- скрипт для наполнения данными - 1;
- использование django management commands - 1;
- соблюдение требований по объему данных - 1.

Отображение списка вопросов - 3:

- список новых вопросов - 1;
- список популярных - 1;
- список вопросов по тегу - 1.

Отображение страницы вопроса - 3:

- общее - 3.

## ДЗ 4 “Авторизация и обработка форм” - 15 баллов
Вход на сайт - 3:

- общее - 1;
- возврат на исходную страницу - 1;
- отображение ошибок - 1.

Регистрация на сайте - 3:

- общее - 2;
- отображение ошибок - 1.

Выход с сайта - 1:

- Отображение текущего пользователя в шапке - 1.

Добавление вопроса - 4:

- общее - 1;
- добавление тегов - 1;
- отображение ошибок - 1;
- редирект на страницу вопроса - 1.

Добавление ответа - 2:

- общее - 1;
- редирект на добавленный ответ - 1.

Проверка метода запроса 1.

## ДЗ 5 “Изображения и обработка AJAX запросов” - 11 баллов
Загрузка и отображение аватарок пользователей - 4:

- заливка картинок в uploads - 2;
- отображение картинок из uploads - 2.

Страница редактирования профиля - 2:

- общее - 1;
- отображение ошибок - 1.

Лайки вопросов и ответов - 2:

- общее - 1;
- AJAX - 1.

Отметка “правильный ответ” - 2:

- общее - 1;
- AJAX - 1.

Проверка авторизации, csrf, метода запроса, авторства - 1.

## ДЗ 6 “Настройка серверов” - 14 баллов
Настройка nginx для отдачи статического контента - 5:

- общее - 3;
- локейшен /uploads/ приоритетнее статики - 1;
- проксирование - 1.

Настройка gunicorn для запуска wsgi приложений - 2:

- общее - 2.

Создание WSGI-приложения - 2:

- наличие запускаемого скрипта - 1
- правильное отображение списка GET и POST параметров - 1.

Оценка производительности nginx и gunicorn - 5:

- статический документ напрямую через nginx и напрямую через gunicorn - 2;
- динамический документ напрямую через gunicorn - 1;
- динамический документ через nginx с проксирование на gunicorn, с кэшом и без кэша (proxy_cache) - 2;

## ДЗ 7 “Дополнительные функции” - 20 баллов
Real-time сообщения - 8:

- настройка nginx+mod_push - 2;
- подключение к mod_push c помощью jQuery - 3;
- отправка сообщений в mod_push - 3.

Блок популярные теги - 4:

- правильный расчет тегов - 2;
- предварительный расчет по cron - 2 (просто кеширование - 1).

Блок лучшие пользователи - 4:

- правильный расчет пользователей - 2;
- предварительный расчет по cron - 2 (просто кеширование - 1).

Поиск по заголовку и содержимому вопроса - 4:

- корректная работа поиска - 1;
- поисковые подсказки - 1;
- отправка данных на сервер по мере ввода - 1;
- ожидание ввода всех символов пользователем - 1.
