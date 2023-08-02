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


#### Запустить проект (в папке с yml файлом)
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



