- Start mysql with docker

``` bash
docker run --rm \
-e MYSQL_ALLOW_EMPTY_PASSWORD=yes \
mysql
```

- Access database 

``` bash
docker exec -it CONTAINER_ID \
mysql -uroot -p
```
