# Ready-to-Use MySQL Docker Infrastructure

This repository provides a ready-to-use Docker Compose configuration for deploying a MySQL container with updated environment configurations. The setup is fully customizable via environment variables, making it ideal for development, testing, or small-scale deployments.

## Features
- **Containerized MySQL Instance:** Quickly spin up a MySQL server using Docker.
- **Customizable Settings:** Configure MySQL credentials, ports, data persistence, and health checks using environment variables.
- **Persistent Data Storage:** MySQL data is stored on your host using a mapped volume (`MOUNT_PATH_FOR_VAR_LIB_MYSQL`).
- **Built-in Health Checks:** Ensure the MySQL server is operational with periodic health checks.
- **Easy Orchestration:** Use Docker Compose for simple startup and teardown processes.

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Getting Started

### 1. Clone the Repository
Clone the repository to your local machine:

```bash
git clone https://github.com/yourusername/yourrepository.git
cd yourrepository
```

### 2. Configure Environment Variables
An example environment configuration is provided in the `.env.example` file. Copy it to a new file named `.env`:

```bash
cp .env.example .env
```

Open the `.env` file in your favorite text editor and update the settings as needed. Key configurations include:

- **Docker & MySQL Settings:** The MySQL image version is set to `mysql:8.0`.
- **Credentials:**  
  - `MYSQL_ROOT_SECRET` for the MySQL root user.
  - `MYSQL_USER_SECRET` for the non-root MySQL user.
- **Ports:** Both the internal and external MySQL ports are set to `3307`.
- **Data Persistence:** Data is stored at the host directory specified by `MOUNT_PATH_FOR_VAR_LIB_MYSQL` (default: `./mysql`).
- **Health Checks:** Settings for verifying MySQL is running correctly.
- **Miscellaneous:** Other configurations such as timezone (`TZ`) and whether to allow an empty secret (`ALLOW_EMPTY_PASSWORD`).

### 3. Start the MySQL Container
Launch the container using Docker Compose:

```bash
docker-compose --env-file .env up -d
```

This command starts the MySQL container in detached mode using the configuration defined in your `.env` file.

## Environment Variables

Below is an overview of the key environment variables used in this project:

| Variable                         | Description                                                                              | Example / Default Value          |
| -------------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------- |
| `ENV_FILE`                       | Path to the environment file for Docker Compose.                                         | `.env`                           |
| `DOCKER_IMAGE_NAME`              | Docker image name for MySQL.                                                             | `mysql:8.0`                      |
| `MYSQL_CONTAINER`                | Name assigned to the MySQL container.                                                    | `mysql_db`                       |
| `MYSQL_ROOT_SECRET`              | Secret for the MySQL root user.                                                          | `my_root_secret`                 |
| `MYSQL_RANDOM_ROOT_PASSWORD`     | When set to `yes`, a random root secret is generated (ignores `MYSQL_ROOT_SECRET`).         | `no`                             |
| `MYSQL_ONETIME_PASSWORD`         | Forces a root secret change at first login when set to `yes`.                            | `no`                             |
| `MYSQL_DATABASE`                 | Name of the default database created at container startup.                               | `example_db`                     |
| `MYSQL_USER`                     | Name of the non-root MySQL user created in the container.                                | `example_user`                   |
| `MYSQL_USER_SECRET`              | Secret for the non-root MySQL user (formerly `MYSQL_PASSWORD`).                           | `example_user_secret`            |
| `MYSQL_ROOT_HOST`                | Defines which hosts can connect as root (e.g., `%` allows connections from any host).       | `%`                              |
| `MYSQL_INITDB_SKIP_TZINFO`       | If set to `yes`, skips importing timezone data during MySQL initialization.                | `no`                             |
| `INTERNAL_PORT`                  | Port number within the container where MySQL listens.                                    | `3307`                           |
| `EXTERNAL_PORT`                  | Port number exposed on the host that maps to the container’s MySQL port.                   | `3307`                           |
| `CONTAINER_RESTART`              | Docker restart policy (e.g., `unless-stopped`, `always`).                                | `unless-stopped`                 |
| `MOUNT_PATH_FOR_VAR_LIB_MYSQL`   | Host directory path for persisting MySQL data.                                           | `./mysql`                        |
| `HEALTHCHECK_TEST`               | Command to perform MySQL health check.                                                   | `mysqladmin ping -h localhost --silent` |
| `HEALTHCHECK_INTERVAL`           | Interval between health check attempts.                                                  | `30s`                            |
| `HEALTHCHECK_TIMEOUT`            | Maximum allowed time for each health check attempt.                                      | `10s`                            |
| `HEALTHCHECK_RETRIES`            | Number of failed health check attempts before marking the container unhealthy.             | `3`                              |
| `TZ`                             | Timezone for the container.                                                               | `UTC`                            |
| `ALLOW_EMPTY_PASSWORD`           | Allows MySQL to run with an empty root secret (not recommended for production).             | `no`                             |

## Data Persistence

The MySQL data directory is mounted from the host directory defined by `MOUNT_PATH_FOR_VAR_LIB_MYSQL` (default: `./mysql`) to the container’s `/var/lib/mysql`. This setup ensures that your data survives container restarts or removals.

## Managing the Container

### Viewing Logs
To view the MySQL container logs:

```bash
docker-compose --env-file .env logs mysql
```

### Stopping the Container
To stop the container without removing it:

```bash
docker-compose --env-file .env stop
```

To stop and remove the container:

```bash
docker-compose --env-file .env down
```

To stop, remove the container, and remove the associated volumes:

```bash
docker-compose --env-file .env down -v
```

## Troubleshooting

If you experience issues:
- Review the logs using `docker-compose --env-file .env logs mysql`.
- Confirm that the settings in your `.env` file match your desired configuration.
- Ensure that the directory specified by `MOUNT_PATH_FOR_VAR_LIB_MYSQL` has the necessary permissions.

## Contributing

Contributions are welcome! To contribute:
1. Fork this repository.
2. Create a feature branch for your changes.
3. Commit your changes with descriptive messages.
4. Open a pull request describing your modifications.

## License

This project is licensed under the [MIT License](LICENSE).

---

Happy coding and enjoy your MySQL Docker infrastructure!