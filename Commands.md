# Команды для поднятия сайта локально

## Создаем образы

```shell
docker build -t backend_todo ./backend
```

```shell
docker build -t database_todo ./database
```

```shell
docker build -t nginx_todo ./frontend
```

## Создаем сеть и вольюм

```shell
docker volume create volume_todo
```

```shell
docker network create back_net
```

## Поднимаем базу данных

```shell
docker run --rm -d \
  --name database \
  --net=back_net \
  -v volume_todo:/var/lib/postgresql/data \
  -e POSTGRES_DB=docker_app_db \
  -e POSTGRES_USER=docker_app \
  -e POSTGRES_PASSWORD=docker_app \
  database_todo
```

## Поднимаем бекенд

```shell
docker run --rm -d \
  --name backend \
  --net=back_net \
  -e HOST=database \
  -e PORT=5432 \
  -e DB=docker_app_db \
  -e DB_USERNAME=docker_app \
  -e DB_PASSWORD=docker_app \
  backend_todo
```

## Поднимаем фронтедн

```shell
docker run --rm -d \
  --name frontend \
  --net=back_net \
  -p 80:80 \
  -v $(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
  nginx_todo
```

## Заходим на сайт [http://localhost](http://localhost)

