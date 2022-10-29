---
tags:
    - Docker
---

Getting Started with PostgreSQL using Docker-Compose

docker-compose.yml：

```javascript
version: '3'
services:
  database:
    image: "postgres" # use latest official postgres version
    env_file:
      - database.env # configure postgres
    volumes:
      - database-data:/var/lib/postgresql/data/ # persist data even if container shuts downvolumes:
  database-data: # named volumes can be managed easier using docker-compose

```



database.env：

```javascript
POSTGRES_USER=unicorn_user
POSTGRES_PASSWORD=magical_password
POSTGRES_DB=rainbow_database

```



https://medium.com/analytics-vidhya/getting-started-with-postgresql-using-docker-compose-34d6b808c47c

