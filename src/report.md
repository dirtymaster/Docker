## Part 1.
* Выкачиваем docker образ с ngnix с помощью команды docker pull ngnix<br>![](part_1/1.png)
* Проверяем наличие docker образа с помощью команды docker images<br>![](part_1/2.png)
* Запускаем docker образ командой docker run -d 0e901e68141f (подставить IMAGE ID из предыдущего пункта)<br>![](part_1/3.png)
* Проверяем, что образ запустился командой docker ps<br>![](part_1/4.png)
* Смотрим информацию о системе через docker inspect 55e98072a471 (подставить CONTAINER ID из предыдущего пункта)
    + Размер контейнера (в байтах)<br>![](part_1/6.png)
    + Список открытых портов<br>![](part_1/7.png)
    + IP контейнера<br>![](part_1/8.png)
* Останавливаем docker образ командой docker stop 55e98072a471<br>![](part_1/9.png)
* Проверяем, что docker остановился командой docker ps<br>![](part_1/10.png)
* Запускаем docker c замапленными портами 80 и 443 командой docker run -p 80:80 -p 443:443 -d nginx<br>![](part_1/11.png)
* Проверяем, что в браузере доступна стартовая страница nginx по адресу localhost:80<br>![](part_1/12.png)
* Перезапускаем docker контейнер командой docker restart 6e08df820721<br>![](part_1/13.png)
* Проверяем, что контейнер запустился командой docker ps<br>![](part_1/14.png)

## Part 2.
* Читаем nginx.conf внутри docker образа командой docker exec -it 095f04c1a6f3 cat /etc/nginx/nginx.conf<br>![](part_2/1.png)
* Полезная инфа: http://nginx.org/ru/docs/beginners_guide.html
* Создаем на локальной машине файл nginx.conf и настраиваем в нем отдачу страницу статуса сервера nginx по пути /status<br>![](part_2/2.png)
* Полезная инфа: https://900913.ru/tldr/common/en/docker-cp/
* Копируем созданный файл внутрь docker образа через команду docker cp nginx.conf 095f04c1a6f3:/etc/nginx<br>![](part_2/3.png)
* Перезапускаем nginx внутри docker образа командой docker exec 095f04c1a6f3 nginx -s reload<br>![](part_2/4.png)
* Проверяем, что по адресу localhost:80/status отдается страничка со статусом сервера nginx<br>![](part_2/5.png)
* Полезная инфа: https://www.youtube.com/watch?v=oUzHjXdq7gk
* Экспортируем контейнер в файл container.tar командой docker export -o container.tar 095f04c1a6f3<br>![](part_2/6.png)
* Останавливаем контейнер командой docker stop 6e08df820721<br>![](part_2/7.png)
* Удаляем образ командой docker rmi nginx -f<br>![](part_2/8.png)
* Импортируем контейнер обратно командой docker import container.tar nginx<br>![](part_2/9.png)
* Запускаем импортированный контейнер командой docker run -p 80:80 -p 443:443 -d -it nginx bash<br>![](part_2/10.png)
* Запускаем nginx командой docker exec -it 54879401f13b service nginx start<br>![](part_2/11.png)
* Проверяем, что по адресу localhost:80/status отдается страничка со статусом сервера nginx<br>![](part_2/12.png)

## Part 3.
* Запустить контейнер с портом 81 командой docker run -p 81:81 -d nginx
* Перейти в установленный контейнер командой docker exec -it beautiful_mccarthy bash
* Установить sudo apt-get install -y libfcgi-dev, sudo, apt-get install -y spawn-fcgi
* Перекинуть в контейнер файлы main.c, run.sh, nginx.conf
* Скомпилировать main.c командой gcc main.c -lpthread -lfcgi -o server
* Запустить бинарник командой spawn-fcgi -p 8080 -n ./server
* В другом терминале дать права скрипту командой chmod +x run.sh, запустить run.sh

## Part 4.
* Полезная инфа: https://www.youtube.com/watch?v=ZB8JBWriDVo
* Собираем Dockerfile командой docker build -t hello_world:1.0 .
* Запускаем контейнер командой docker run -p 80:81 -d hello_world:1.0

## Part 5.
* Устанавливаем Dockle через brew
* Выполняем команду dockle hello_world:1.0 и исправляем ошибки

## Part 6.
* Выполняем команду docker-compose up
