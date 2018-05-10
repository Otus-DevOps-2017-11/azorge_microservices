# azorge_microservices

#### Homework 14:

установка `docker` и основные операции с ним:
- `docker run <name>' - запуск нового контейнера
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

