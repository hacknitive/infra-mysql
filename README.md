# Ready-to-Use MySQL Docker Infrastructure

This repository provides a ready-to-use Docker Compose configuration for deploying a MySQL container. The configuration is highly customizable via environment variables, making it ideal for local development, testing, or small-scale deployments.

## Features

- **Containerized MySQL Instance:** Quickly spin up a MySQL server using Docker.
- **Customizable Settings:** Configure all MySQL settings (credentials, database name, ports, etc.) using environment variables.
- **Persistent Data Storage:** The MySQL data is stored in a local volume (`./mysql`), ensuring persistence across container restarts.
- **Built-in Health Checks:** Automatic health checks using `mysqladmin` ensure the MySQL service is running correctly.
- **Easy Orchestration:** Leverage Docker Compose for simple startup and teardown processes.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Getting Started

Follow these steps to get your MySQL container up and running:

### 1. Clone the Repository

Clone the repository to your local machine:

```bash
git clone https://your.repository.url.git
cd your_repository_directory
```

### 2. Configure Environment Variables

An example environment configuration is provided in the `.env.example` file. To use it:

1. Copy the `.env.example` file to a new file named `.env`:
   ```bash
   cp .env.example .env
   ```
2. Open the `.env` file in your favorite text editor and update the values as needed. This file controls:
   - Docker image settings (e.g., MySQL version)
   - MySQL credentials (e.g., root password, user, and database names)
   - Port configuration
   - Health check parameters
   - Miscellaneous settings (e.g., timezone)

### 3. Start the MySQL Container

Run the following command to start the container in detached mode:

```bash
docker-compose --env-file .env up -d
```

The MySQL container will start using the settings defined in your `.env` file.

## Environment Variables

The following key environment variables are defined in the `.env.example` file:

| Variable                     | Description                                                                 | Example / Default Value                           |
| ---------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------- |
| `ENV_FILE`                   | Relational or absolute path of the env file.                                | `.env`                                            |
| `DOCKER_IMAGE_NAME`          | Docker image name for MySQL.                                                | `mysql:8.4`                                       |
| `MYSQL_ROOT_PASSWORD`        | Password for the MySQL root user.                                           | `my_root_password`                                |
| `MYSQL_RANDOM_ROOT_PASSWORD` | When set to `yes`, generates a random root password.                        | `no`                                              |
| `MYSQL_ONETIME_PASSWORD`     | Forces a password change at first login when set to `yes`.                  | `no`                                              |
| `MYSQL_DATABASE`             | Default database to be created at container startup.                      | `example_db`                                      |
| `MYSQL_USER`                 | Name of the non-root MySQL user to create.                                  | `example_user`                                    |
| `MYSQL_PASSWORD`             | Password for the non-root MySQL user defined above.                         | `example_password`                                |
| `MYSQL_ROOT_HOST`            | Controls which hosts can connect as root (e.g., `%` allows connections from any host).   | `%`                                   |
| `MYSQL_INITDB_SKIP_TZINFO`   | If set to `yes`, skips importing timezone info into MySQL.                  | `no`                                              |
| `INTERNAL_PORT`              | Port number inside the container where MySQL listens.                       | `3306`                                            |
| `EXTERNAL_PORT`              | Port number exposed on the host that maps to the container’s MySQL port.      | `3306`                                            |
| `CONTAINER_RESTART`          | Docker restart policy (e.g., `unless-stopped`, `always`).                   | `unless-stopped`                                  |
| `HEALTHCHECK_TEST`           | Health check command using `mysqladmin` to confirm MySQL is running.        | `mysqladmin ping -h localhost --silent`           |
| `HEALTHCHECK_INTERVAL`       | Interval between consecutive health check attempts.                       | `30s`                                             |
| `HEALTHCHECK_TIMEOUT`        | Maximum time allowed for a single health check attempt.                     | `10s`                                             |
| `HEALTHCHECK_RETRIES`        | Number of failed health checks before the container is marked as unhealthy.   | `3`                                               |
| `TZ`                         | Timezone for the container (e.g., `UTC`, `America/New_York`).               | `UTC`                                             |
| `MYSQL_ALLOW_EMPTY_PASSWORD` | Allows MySQL to run with an empty root password (not recommended for production). | `no`                                         |

## Data Persistence

The MySQL data directory is mounted from the host directory (`./mysql`) to the container's `/var/lib/mysql`. This setup ensures that your data remains intact even if the container is restarted or removed.

## Health Check Details

The Docker Compose configuration includes a health check to ensure that the MySQL server is running smoothly. The health check leverages the following settings from the `.env` file:

- **Health Check Command:** Uses `mysqladmin ping -h localhost --silent` to verify connectivity.
- **Interval:** The time between successive health checks.
- **Timeout:** The allowed maximum duration for each health check attempt.
- **Retries:** The number of times a health check failure is tolerated before marking the container as unhealthy.

## Stopping the Container

To stop the container without removing it, run:

```bash
docker-compose --env-file .env stop
```

To stop and remove the container (and optionally its associated volumes), execute:

```bash
docker-compose  --env-file .env down       # Stops and removes containers
docker-compose  --env-file .env down -v    # Stops, removes containers, and removes volumes
```

## Logs and Troubleshooting

For viewing container logs, use:

```bash
docker-compose  --env-file .env logs mysql
```

If you encounter issues:
- Verify your environment variables in the `.env` file.
- Check that the `./mysql` directory has the required permissions.
- Review the logs for any specific error messages.

## Customization

- **MySQL Version:** Change the `DOCKER_IMAGE_NAME` variable to use a different MySQL version.
- **Database Configuration:** Adjust the environment variables to suit your development needs.
- **Additional Services:** If needed, extend `docker-compose.yml` to add more services or dependencies.

## Contributing

Contributions are welcome! If you have suggestions or improvements, please fork the repository and open a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
