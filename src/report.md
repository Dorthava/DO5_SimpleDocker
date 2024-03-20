## Part 1. Готовый докер

== Задание ==

Взять официальный докер образ с nginx и выкачать его при помощи docker pull

![Alt text](<images/Screenshot from 2023-11-19 18-09-43.png>)

Проверить наличие докер образа через docker images

![Alt text](<images/Screenshot from 2023-11-19 18-11-04.png>)

Запустить докер образ через docker run -d [image_id|repository]

![Alt text](<images/Screenshot from 2023-11-19 18-11-56.png>)

Проверить, что образ запустился через docker ps

![Alt text](<images/Screenshot from 2023-11-19 18-12-58.png>)

Посмотреть информацию о контейнере через docker inspect [container_id|container_name].
По выводу команды определить и поместить в отчёт размер контейнера, список замапленных портов и ip контейнера

Размер контейнера:
* Общий размер всех файлов в контейнере - SizeRootFs
* Размер файлаов, которыу подверглись изменению, по сравнению с его прошлым образом - SizeRw

![Alt text](<images/Screenshot from 2023-11-19 18-31-47.png>)

* IP-адрес контейнера: 172.17.0.2

![Alt text](<images/Screenshot from 2023-11-19 18-32-50.png>)

* Замапленный порт: 80 

![Alt text](<images/Screenshot from 2023-11-19 18-33-36.png>)

Остановить докер образ через docker stop [container_id|container_name]

![Alt text](<images/Screenshot from 2023-11-19 18-37-11.png>)

Проверить, что образ остановился через docker ps

![Alt text](<images/Screenshot from 2023-11-19 18-39-15.png>)

Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду run

![Alt text](<images/Screenshot from 2023-11-19 19-01-25.png>)

Проверить, что в браузере по адресу localhost:80 доступна стартовая страница nginx

![Alt text](<images/Screenshot from 2023-11-19 18-57-52.png>)

Перезапустить докер контейнер через docker restart [container_id|container_name]

![Alt text](<images/Screenshot from 2023-11-19 19-17-45.png>)

Проверить любым способом, что контейнер запустился

![Alt text](<images/Screenshot from 2023-11-19 19-14-53.png>)

## Part 2. Операции с контейнером

== Задание ==

Прочитать конфигурационный файл nginx.conf внутри докер контейнера через команду exec

![Alt text](<images/Screenshot from 2023-11-19 19-29-15.png>)

Создать на локальной машине файл nginx.conf.

Настроить в нем по пути /status отдачу страницы статуса сервера nginx

![Alt text](<images/Screenshot from 2023-11-19 21-08-54.png>)

Скопировать созданный файл nginx.conf внутрь докер образа через команду docker cp

![Alt text](<images/Screenshot from 2023-11-19 19-44-01.png>)

Перезапустить nginx внутри докер образа через команду exec

![Alt text](<images/Screenshot from 2023-11-19 19-47-12.png>)

Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx

![Alt text](<images/Screenshot from 2023-11-19 21-10-18.png>)

Экспортировать контейнер в файл container.tar через команду export

![Alt text](<images/Screenshot from 2023-11-19 21-14-01.png>)

Так выглядит наш контейнер:

![Alt text](<images/Screenshot from 2023-11-19 21-14-19.png>)

Остановить контейнер

![Alt text](<images/Screenshot from 2023-11-19 21-15-33.png>)

Удалить образ через docker rmi [image_id|repository], не удаляя перед этим контейнеры

![Alt text](<images/Screenshot from 2023-11-19 21-19-52.png>)

Удалить остановленный контейнер

![Alt text](<images/Screenshot from 2023-11-19 21-21-38.png>)

Импортировать контейнер обратно через команду import

![Alt text](<images/Screenshot from 2023-11-20 09-06-20.png>)

Запустить импортированный контейнер

![Alt text](<images/Screenshot from 2023-11-20 09-08-28.png>)

Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx

![Alt text](<images/Screenshot from 2023-11-20 09-09-17.png>)

## Part 3. Мини веб-сервер

== Задание ==

Написать мини сервер на C и FastCgi, который будет возвращать простейшую страничку с надписью Hello World!

![Alt text](<images/Screenshot from 2023-11-20 18-20-01.png>)

Запустить написанный мини сервер через spawn-fcgi на порту 8080

* Файл "_hello.c_" переместили в контейнер, а именно в /home. Скомпилировали файл.

![Alt text](<images/Screenshot from 2023-11-20 18-36-32.png>)

![Alt text](<images/Screenshot from 2023-11-20 18-20-43.png>)

* Выполняем команду:

![Alt text](<images/Screenshot from 2023-11-20 18-21-09.png>)

Написать свой nginx.conf, который будет проксировать все запросы с 81 порта на 127.0.0.1:8080

* Поменяли конфигурацию в nginx.conf:

![Alt text](<images/Screenshot from 2023-11-20 18-22-13.png>)

Проверить, что в браузере по localhost:81 отдается написанная вами страничка

![Alt text](<images/Screenshot from 2023-11-20 18-21-31.png>)

Положить файл nginx.conf по пути ./nginx/nginx.conf (это понадобится позже)

![Alt text](<images/Screenshot from 2023-11-20 18-35-48.png>)

## Part 4. Свой докер

== Задание ==

Написать свой докер образ, который:

1) собирает исходники мини сервера на FastCgi из Части 3

2) запускает его на 8080 порту

3) копирует внутрь образа написанный ./nginx/nginx.conf


4) запускает nginx.
nginx можно установить внутрь докера самостоятельно, а можно воспользоваться готовым образом с nginx'ом, как базовым.

![Alt text](<images/Screenshot from 2023-11-20 22-24-07.png>)

Собрать написанный докер образ через docker build при этом указав имя и тег

Проверить через docker images, что все собралось корректно

![Alt text](<images/Screenshot from 2023-11-20 22-23-50.png>)

Запустить собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки ./nginx внутрь контейнера по адресу, где лежат конфигурационные файлы nginx'а (см. Часть 2)

* Конфигурационный файл:

![Alt text](<images/Screenshot from 2023-11-20 22-32-42.png>)

* Запускаем наш образ и смотрим на результат:

![Alt text](<images/Screenshot from 2023-11-20 22-24-20.png>)

Проверить, что по localhost:80 доступна страничка написанного мини сервера

![Alt text](<images/Screenshot from 2023-11-20 22-23-18.png>)

Дописать в ./nginx/nginx.conf проксирование странички /status, по которой надо отдавать статус сервера nginx

![Alt text](<images/Screenshot from 2023-11-20 22-32-42.png>)

Перезапустить докер образ
Если всё сделано верно, то, после сохранения файла и перезапуска контейнера, конфигурационный файл внутри докер образа должен обновиться самостоятельно без лишних действий

Проверить, что теперь по localhost:80/status отдается страничка со статусом nginx

![Alt text](<images/Screenshot from 2023-11-20 22-42-05.png>)

## Part 5. Dockle

* В Docker Hub был скачан образ Dockle. Запускаем следующей командой:

![Alt text](<images/Screenshot from 2023-11-21 09-22-16.png>)

* Так выглядят исправления в файле Docker:

![Alt text](<images/Screenshot from 2023-11-21 12-42-51.png>)

Исправить образ так, чтобы при проверке через dockle не было ошибок и предупреждений

* Также провели проверку на запуск и работу контейнера:

![Alt text](<images/Screenshot from 2023-11-21 12-42-28.png>)

## Part 6. Базовый Docker Compose

== Задание ==

Написать файл docker-compose.yml, с помощью которого:

1) Поднять докер контейнер из Части 5 (он должен работать в локальной сети, т.е. не нужно использовать инструкцию EXPOSE и мапить порты на локальную машину)

2) Поднять докер контейнер с nginx, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера

Замапить 8080 порт второго контейнера на 80 порт локальной машины

![Alt text](<images/Screenshot from 2023-11-21 16-39-41.png>)

* Все запущенные контейнеры были остановлены. Так же в прокси-сервере поменяли файл nginx.conf:

![Alt text](<images/Screenshot from 2023-11-21 16-40-03.png>)

Собрать и запустить проект с помощью команд docker-compose build и docker-compose up

![Alt text](<images/Screenshot from 2023-11-21 16-42-37.png>)

![Alt text](<images/Screenshot from 2023-11-21 16-42-49.png>)

Проверить, что в браузере по localhost:80 отдается написанная вами страничка, как и ранее

* С помощью утилиты _curl_ проверим наше соединение:

![Alt text](<images/Screenshot from 2023-11-21 16-40-47.png>)
