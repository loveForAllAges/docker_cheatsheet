# Шпаргалка по Docker
**Docker** – платформа, которая предназначена для разработки, развертывания и запуска приложений в контейнерах. Docker Контейнеризация позволяет изолировать зависимости и создать единую простую точку запуска ПО. Предшественником Docker является виртуальная машина. Docker потребляет меньше ресурсов, его очень легко переносить и он быстрее запускается и приходит в работоспособное состояние.

**Dockle** – инструмент для проверки безопасности образов контейнеров, который можно использовать для поиска уязвимостей. Функции Dockle: поиск уязвимостей в образах, помощь в создании правильного Dockerfile.

**Docker Compose** – инструментальное средство, которое предназначено для решения задач, связанных с развертыванием проектов. Docker Compose может пригодиться, если для обеспечения функционирования проекта используется несколько сервисов. Используется для одновременного управления несколькими контейнерами, входящими в состав приложения.

**Dockerfile** – текстовый документ, содержащий все команды, которые пользователь может вызвать в командной строке для сборки образа. Ниже перечислены поддерживаемые инструкции Dockerfile.
## Docker service
```bash
# Версия Docker
docker --version
# Очистка всех ресурсов – образы, контейнеры, volumes, networks
docker system prune
# Инициализация Docker внутри приложения
docker init
# Посмотреть сервис/контейнер приложения
docker compose watch
```
## Docker Image
**Docker-образ** – упаковка для приложения и зависимостей. Состоит из слоев. Каждый слой описывает какое-то изменение, которое должно быть выполнено с данными на запущенном контейнере. Структура связей между слоями – иерархическая. Имеется базовый слой,  на который накладываются остальные слои. Для создания образа используется Dockerfile. Каждая инструкция в нем создает новый слой.
```bash
# Создать образ из Dockerfile
docker build -t image_name path_to_dockerfile
# Список локальных образов
# Или: docker image ls
docker images
# Запулить образ из DockerHub
docker pull image_name:tag
# Удалить локальный образ
# Или: docker rmi container_id
docker rmi image_name:tag
# Тегнуть образ
docker tag source_image:tag new_image:tag
# Закинуть образ в DockerHub
docker push image_name:tag
# Детали образа
docker image inspect image_name:tag
# Сохранить образ в архив
docker save -o image_name.tar image_name:tag
# Загрузить образ из архива
docker load -i image_name.tar
# Удалить не используемые образы
docker image prune
```
## Docker container
```bash
# Запустить контейнер с именем из образа
docker run --name container_name image_name:tag
# Список всех запущенных контейнеров
docker ps
# Список всех контейнеров
docker ps -a
# Внутрь контейнера
docker exec -it container_name_or_id /bin/bash
# Статистика контейнера
docker stats container_name_or_id
# Остановить запущенный контейнер
docker stop container_name_or_id
# Запустить остановленный контейнер
docker start container_name_or_id
# Удалить остановленный контейнер
docker rm container_name_or_id
# Удалить запущенный контейнер
docker rm -f container_name_or_id
# Детали контейнера
docker inspect container_name_or_id
# Логи контейнера
docker logs container_name_or_id
# Поставить на паузу запущенный контейнер
docker pause container_name_or_id
# Поставить на плей запущенный контейнер
docker unpause container_name_or_id
```
## Docker volume
**Docker volume** – специальные директории на хостовой машине или удаленном сервере, которые монтируются в контейнер. Они обеспечивают постоянное хранение данных и могут использоваться для обмена данными между контейнерами.
```bash
# Create a named volume
docker volume create volume_name
# List of all volumes
docker volume ls
# Inspect details of a volume
docker volume inspect volume_name
# Remove a volume
docker volume rm volume_name
# Run a container with a volume (mount)
docker run --name container_name -v volume_name:/path/in/container image_name:tag
# Copy files between a container and a volume
docker cp local_file_or_directory container_name:/path/in/container
```
## Docker network
**Docker network (Port mapping)** – сеть, позволяющая контейнерам подключаться и взаимодействовать друг с другом. Сетевые драйверы:
- **bridge**: Сетевой драйвер по умолчанию;
- **host**: Удаляет сетевую изоляцию между контейнером и хостом;
- **overlay**: Оверлейные сети соединяют несколько демонов Docker вместе;
- **none**: Полностью изолирует контейнер от хоста и других контейнеров;
- **ipvlan**: Обеспечивают полный контроль над адресацией как IPv4, так и IPv6;
- **macvlan**: Назначает MAC-адрес контейнеру.
```bash
# Run a container with a port mapping
docker run --name container_name -p host_port:container_port image_name
# List all networks
docker network ls
# Inspect details of a network
docker network inspect network_name
# Create a user-defined bridge network
docker network create network_name
# Remove a network
docker network rm network_name
# Connect a container to a network
docker network connect network_name container_name
# Disconnect a container from a network
docker network disconnect network_name container_name
```
## Docker compose
```bash
# Create and start containers defined in a docker-compose.yml file. This command reads the docker-compose.yml file and starts the defined services in the background.
docker compose up
# Stop and remove containers defined in a docker-compose.yml file. This command stops and removes the containers, networks and volumes defined in the .yml file.
docker compose down
# Build or rebuild services. This command builds or rebuilds the Docker images for the services defined in the docker-compose.yml file.
docker compose build
# List containers for a specific Docker Compose project. This command lists the containers for the services defined in the docker-compose.yml file.
docker compose ps
# View logs for services. This command shows the logs for all services defined in the docker-compose.yml file.
docker compose logs
# Scale services to a specific number of containers
docker compose up -d --scale service_name=number_of_containers
# Run a one-time command in a service
docker compose run service_name command
# List all volumes. Docker Compose creates volumes for services. This command shows it
docker volume ls
# Pause a service. This command pauses the specified service
docker volume pause service_name
# Unpause a service
docker volume unpause service_name
# View details of a service
docker compose ps service_name
```
## Docker compose reference
```dockercompose
# Specifies the version of the Docker Compose file format.
version: ‘3.8’

# Defines the services/containers that make up the application
services:
	web:
		image: nginx:latest

# Configures custom networks for the application
networks:
	my_network:
		driver: bridge

# Defines named volumes that the services can use
volumes:
	my_volume:

# Sets environment variables for a service
environment:
	- NODE_ENV=production

# Maps host ports to container ports
ports:
	- “8080:80”

# Specifies dependencies between services, ensuring one service starts before another
depends_on:
	- db

# Configures the build context and Dockerfile for a service
build:
	context: .
	dockerfile: Dockerfile.dev

# Mounts volumes from another service or container
volumes_from:
	- service_name

# Overrides the default command specified in the Docker image.
command: [“npm”, “start”]
```
## Dockerfile reference
```dockerfile
# Specifies the base image for the Docker image
FROM image_name:tag
# Sets the working directory for subsequent instructions
WORKDIR /path/to/directory
# Copies files or directories from the build context to the container
COPY host_source_path container_destination_path
# Executes commands in the shell
RUN command1 && command2
# Sets environment variables in the image
ENV KEY=VALUE
# Informs Docker that the container listens on specified network ports at runtime
EXPOSE port
# Provides default commands or parameters for an executing container
# Или: CMD executable param1 param2
CMD [“executable”, “param1”, “param2”]
# Configures a container that will run as an executable
# Или: ENTRYPOINT executable param1 param2
ENTRYPOINT [“executable”, “param1”, “param2”]
# Defines variables that users can pass at build-time to the builder with the docker build command
ARG VARIABLE_NAME=default_value
# Creates a mount point for external volumes or other containers
VOLUME /path/to/volume
# Adds metadata to an image in the form of key-value pairs
LABEL key=”value”
# Specifies the username or UID to use when running the image
USER user_name
# Copies files or directories and can extract tarballs in the process
ADD source_path destination_path
# Check a container's health on startup
HEALTHCHECK options
# Specify the author of an image
MAINTAINER name
# Specify instructions for when the image is used in a build
ONBUILD instruction
# Set the default shell of an image
SHELL [“executable”, “params”]
# Specify the system call signal for exiting a container
STOPSIGNAL signal
```
## Примеры Docker compose
Пример 1:
```dockercompose
services:
    mongo:
        image: mongo:latest
        ports:
            - “27017: 27017”
        volumes:
            - mongo_data: /data/db
        environment:
            MONGO_INITDB_ROOT_USERNAME: admin
            MONGO_INITDB_ROOT_PASSWORD: admin
        api:
            build:
                context: ./api
            dockerfile: Dockerfile
            ports:
                - “5000:5000”
            depends_on:
                - mongo
        environment:
            MONGO_URI: mongo_db://admin:admin@mongo:27017/mydatabase
        networks:
            - mern_network
        client:
            build:
                context: ./client
                dockerfile: Dockerfile
        ports:
            - “3000:3000”
        depends_on:
            - api
            networks:
                - mern_network
    volumes:
        mongo_data:
    networks:
        mern_network:
```
Пример 2:
```dockercompose
version: '3.5'
services:
  web-server:
    image: nginx:stable
    container_name: mynginx
    volumes:
      - '/home/john/it/html:/var/www/html'
      - '/home/john/it/pics:/var/www/pictures'
    environment:
      - 'NGINX_HOST=web.user'
      - 'NGINX_PORT=80'
    ports:
      - '80:80'
      - '443:443'
    restart: unless-stopped
networks:
  default:
    driver: bridge
    name: webnet
```
## Примеры Dockerfile
Пример 1:
```dockerfile
# Use an official Node.js runtime as a base image
FROM node:20-alpine

# Set the working directory to /app
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the current directory contents to the container at /app
COPY . .

# Expose port 8080 to the outside world
EXPOSE 8080

# Define environment variable
ENV NODE_ENV=production

# Run app.js when the container launches
CMD node app.js
```