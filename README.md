# Готовый конфиг WordPress для локальной разработки на основе Docker Compose:

### Не забудьте поменять версии в docker-compose.yml на необходимые для вашей разработки!
##### По умолчанию файл настроен на:
* docker: 3.3
* php: 7.4
* mysql: 5.7
* wordpress-6.2.2
###### Внешние порты на вашу локальную машину:
* phpmyadmin port: localhost:8081
* wordpress site port: localhost:80

#### Добавляем своего юзера в группу www-data чтоб файлы стали редактируемыми:
```bash
usermod -a -G $USER www-data
```

```bash
usermod -a -G root www-data
```
#### Или как вариант добавить своего пользователя и дать права рекурсивно:
```bash
sudo chgrp -R $USER ./wordpress
```
```bash
sudo chmod -R g+rw ./wordpress
```
#### Если темы не работают, или плагины, то эо опять проблемы прав.<br>
Можно создать путь к темам отдельно в docker-compose.yml задав ей права доступа 755 к примеру и тоже рекурсивно.<br>
Такое часто происходит когда вы скачали тему с сата по ftp. Еще можно загрузить тему через админку, тоже должно помочь.<br>
Пример:
```text
- ./themes/my_theme:/var/www/html/wp-content/themes/my_theme
```


### Запустить проект (в папке с yml файлом)
```bash
docker-compose up -d
```

#### Вы можете узнать свой ip адрес командой:
```bash
ip address
```
#### у вас там будет что-то на подобии этого: inet 192.168.3.23/24
#### это и есть ваш ip адрес.
#### wordperss будет доступен по вашему ip адресу который вы узнали выше
#### панель phpmyadmin будет достурна по ip_который_вы_узнали:8081 пароль: pwd12345a его также можно посмотреть в yml файле
#### порты можно поменять в yml файле

#### Узнаем название нашего контейнера (у меня он wordpress-dev_wordpress_1):
```bash
sudo docker ps
```
или

```bash
docker ps
```

#### запускаем команду в контейнере (wordpress-dev_wordpress_1 замените на то имя контейнера которое у вас):
```bash
docker exec -it wordpress-dev_wordpress_1 /bin/bash
```

#### Зашли в оболочку контейнера.
#### Выполняем там команду:
```bash
chown -R www-data:www-data /var/www/html
```
#### Эта команда решит проблему удаления и установки/обюновления тем и самого wordpress

#### Запустить проект (в папке с yml файлом)
```bash
docker-compose up -d
```
#### Остановить и удалить все:
```bash
docker-compose down --volumes
```

## Дополнительная информация по работе с Docker

### Просмотреть список всех образов которые есть на ПК
```bash
docker images
```

### Посмотреть перменные среды (вводные параметры контейнеров или процесов. к примеру пароль для SQL базы)
```bash
printenv
```

### Покажет список запущеных и остановленных контейнеров
```bash
docker ps -a
```

### Остановить контейнер мгновенно. На случай если подвис.
```bash
docker kill id_или_имя_контейнера
```

### Остановить все контейнера и удалить
```bash
docker-compose down
```

### Остановить все контейнера и удалить
```bash
docker-compose down --volumes
```

### Удалить все остановленные контейнеры
```bash
docker container prune
```
или
```bash
docker rm $(docker ps -qa)
```

### Просмотреть список всех образов которые есть на ПК
```bash
docker images
```

### Удалить image по конкретному id
```bash
docker rmi IMAGE_ID_образа
```

### Сам docker-compose.yml содержит:
```yml
version: '3.3'
services:
   wordpress:
     depends_on:
       - db
     image: wordpress:php7.4
     volumes:
       - ./wordpress:/var/www/html:rw
     ports:
       - "80:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: pwd12345a
       WORDPRESS_DB_NAME: wordpress
   db:
     image: mysql:5.7
     volumes:
       - ./mysql:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: pwd12345a
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: pwd12345a
   phpmyadmin:
     depends_on:
       - db
     image: phpmyadmin/phpmyadmin
     restart: always
     ports:
       - "8081:80"
     environment:
       PMA_HOST: db
       MYSQL_ROOT_PASSWORD: pwd12345a
```



