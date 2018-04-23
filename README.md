# azorge_microservices
#### Homework 16:
Все предыдушее перенес в `docker-monolith`.

*. Cборка ui началась не с первого шага потому что часть уже была сделана при сборке `comment` и была закэширована.

1. В reddit-microservices находятся три компонента приложения:
- `post-py` - сервис, отвечающий за написание постов;
- `comment` - сервис, отвечающий за написание комментариев;
- `ui` - веб-интерфейс, работающий с другими сервисами.

2. Создал лля каждого из них Dockerfile и с помощью линтера `hadolint` оптимизировал их. 

3. Собрал образы для всех компонентов:
```
docker build -t azorge/post:1.0 ./post-py
docker build -t azorge/comment:1.0 ./comment
docker build -t azorge/ui:1.0 ./ui
```

4. Создап сеть для приложения:
`docker network create reddit`

5. Запустил контейнеры
```
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post azorge/post:1.0
docker run -d --network=reddit --network-alias=comment azorge/comment:1.0
docker run -d --network=reddit -p 9292:9292 azorge/ui:1.0
```

6. Добавил volume для mongodb
`docker volume create reddit_db`
И перезапустил все контейнеры:
```
docker kill $(docker ps -q)
docker run -d --network=reddit --network-alias=post_db \
--network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post azorge/post:1.0
docker run -d --network=reddit --network-alias=comment azorge/comment:1.0
docker run -d --network=reddit -p 9292:9292 azorge/ui:2.0
```
7. Теперь посты сохраняются после пересобирания образов.

#### Homework 15:
- Создал в `gcp` новый проект `docker`
- Поставил и настроил `docker-machine` для работы с новым проектом
- Собрал ноывый образ `reddit:latest`
- создал новое `firewall-rule` для `docker-machine`
- проверил что есть доступ к приложению по `9292` порту
- загрузил образ на `docker-hub` -  [azorge docker image](https://goo.gl/dARdGE)


#### Homework 14:

установка `docker` и основные операции с ним:
- `docker run <name>` - запуск нового контейнера
- `docker ps -a` - список всех контейнеров
- `docker images` - список сохранненных образов
- `docker start <container_id>` - запуск ранее созданного контейнера
- `docker attach <container_id>` - присоединяет терминал к запущенному контейнеру
- `docker exec -it <container_id> <command>` - запуск команды внутри запущенного контейнера
- `docker stop <container_id>` - SIGTERM и через 10сек KILL контейнера 
- `docker kill <container_id>` - сразу посылает SIGKILL
- `docker commit <u_container_id>` - создает образ из контейнера
- `docker rm <container_id> [-f]` - удалить [работающий] контейнер
- `docker rmi <image_id> [-f]` - удалить [используемый] образ
- `docker rm $(docker ps -a -q)` - удалит все незапущенные контейнеры
- `docker rmi $(docker images -q)` - удалит все образы
