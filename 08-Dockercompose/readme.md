## Docker Compose

![compose](/assets/img/docker_compose.png)

- Another one kind of feature in Docker Eco system is **Docker Compose**.
- Running multiple docker containers for single applications by creating like,
    - Dockerfile Front End
    - Dockerfile Back End
    - Dockerfile Other 
- These all files can be defined in one **Composefile** and can be spun up by a single command.
- As making them as blocks, it can be achieved like shown below,

```text
-- Front End Block
-- Back End Block
-- Volume End Block
-- Other Block
```

- **Installation :**  follow this official [Documentation](https://docs.docker.com/compose/install/)

### Structure of docker composer

The file should be saved as : `docker-compose.yaml` or `docker-compose.yml`.

For example, The file [Word_press](example/docker-compose.yaml) used.

```yaml
version: '3.3'
services:
   db:
     image: mysql:5.7
     container_name: mysql_database
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: word@press
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: abc@123
   wordpress:   
     depends_on:
       - db
     image: wordpress:latest
     container_name: wd_frontend
     volumes:
       - wordpress_files:/var/www/html 
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: abc@123
volumes:
    wordpress_files:
    db_data:
```

Here you can see, This file contains two various sections,

- Structure
- Volumes

### Commands

- `docker-compose up`  To run (create and run) the docker containers from composed file.
- `docker-compose down`  To stop and remove the things containers, networks, images and volumes.
- `docker-compose config` Previews the docker compose file
- `docker-compose images` Shows the respected images created by compose file.
- `docker-compose logs` Prints the total logs
- `docker-compose ps` Status and details of docker-compose services. 
- `docker-compose top` Details of process running by docker-compose.

To view more See this [official docker-compose documentation](https://docs.docker.com/compose/reference/overview).

[next](/08-Dockercompose)
[home](/)