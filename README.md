# dev-apps

This is a YAML file to spin up containers of various applications useful for local development with simple default settings, to be used with Docker Compose.

**IMPORTANT: These applications are for local development only and are not set up to be secure in any way! Absolutely do not allow these to be accessible from the Internet without modifying security settings and properly dealing with secret information like database passwords!**

## Databases

- MongoDB: NoSQL document-based database
- MySQL: Relational database
- PostgreSQL: Relational database

## Database GUIs

- Mongo Express: Web GUI for MongoDB
- Adminer: Web GUI for relational databases (MySQL, PostgreSQL, SQLite). Can be made to work with MongoDB and others, but only by installing extensions.

## Other Applications

- MinIO: Object Storage, compatible with the AWS S3 API.

## Requirements:

- Docker
- Docker Compose (usually included with Docker)

See https://docs.docker.com/get-docker/ for help installing Docker on your system.

## Starting and Stopping

To start the application stack from the directory containing the `dev-apps.yaml` file, execute the following:

    docker-compose -f dev-apps.yaml up -d

_Note:_ The `-d` flag runs the applications in the background ("detached mode").

To stop the applications, execute:

    docker-compose -f dev-apps.yaml down

## Accessing Services

Applications in a Docker Compose stack run on an internal Docker network, so the applications can be referenced internally using the service name. Applications outside the Docker network can use the ports which the container exposes to the host to access the application within.

For example, the MySQL container exposes port 3306 on localhost, which can be used in an application to access the database. To access a GUI, the Adminer container is accessible in a web browser at http://localhost:8080. But to use Adminer to connect to the database, use the server "mysql", since that is the hostname on the containers' internal network.

_Note:_ To access the command line interface for the MySQL database, you must execute commands inside the docker container, such as

    docker exec -it dev-apps_mysql_1 mysql -u admin -p

which will prompt for the password for the `admin` user.

Here, `docker exec -it` instructs Docker to execute a command in an interactive terminal, `dev-apps_mysql_1` is the name given to the container (which can be found using `docker ps` if you do not know it), and `mysql -u admin -p` is the command being run.

## Persistent Storage

Many of these applications (such as databases) require persistent storage, which is accomplished using named Docker volumes. By default, Docker volumes are stored in the system at `/var/lib/docker/volumes/`, but this can vary depending on your Docker installation. For example, my Docker installation (running on Windows 10 with WSL2) keeps its volumes in a network directory

    \\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes
