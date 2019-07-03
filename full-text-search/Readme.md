- Start mysql with docker

``` bash
docker run --rm \
-e MYSQL_ALLOW_EMPTY_PASSWORD=yes \
-e MYSQL_DATABASE=test \
-p 3306:3306 \
mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

- Access database 

``` bash
# enable charset to input Chinese
docker exec -it CONTAINER_ID \
env LANG=C.UTF-8  /bin/bash
```

- Create full text search enabled table

``` mysql
CREATE TABLE articles (
    id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
    title VARCHAR(200),
    body TEXT,
    FULLTEXT (title,body) WITH PARSER ngram
) ENGINE=InnoDB CHARACTER SET utf8mb4;
```

- Insert 

``` mysql
INSERT INTO articles (title,body) VALUES
('数据库管理','在本教程中我将向你展示如何管理数据库'),
('数据库应用开发','学习开发数据库应用程序');
```


- Test case 

``` mysql 
 SET GLOBAL innodb_ft_aux_table="test/articles";

SELECT 
    *
FROM
    information_schema.innodb_ft_index_cache
ORDER BY doc_id , position;


SELECT 
    *
FROM
    articles
WHERE
    MATCH (title , body) AGAINST ('程中' IN natural language MODE);
```