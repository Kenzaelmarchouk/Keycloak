# Keycloak with PostgreSQL and PgAdmin4 Deployment Using Docker Compose
This repository provides a practical guide on how to deploy Keycloak with a PostgreSQL database and access the database using PgAdmin4, all through a Docker Compose setup. 
### Overview
This tutorial sets up and configured in a single docker-compose.yml file:

- **Keycloak**: An open-source identity and access management solution.
- **PostgreSQL**: The database for Keycloak's data storage.
- **PgAdmin4**: A web-based GUI for PostgreSQL, making it easier to explore and manage the database.

### Prerequisites
**Docker**
**Docker compose**
Docker and Docker compose Come pre-installed with **Docker Desktop**

### Getting Started
#### Step1 - Create docker-compose.yml file
Start by cloning this repository, which includes the docker-compose.yml file you’ll need.
```
git clone https://github.com/yourusername/keycloak-postgresql-pgadmin-docker.git
cd keycloak-postgresql-pgadmin-docker

```


##### Explication of docker-compose.yml
This docker-compose.yml file defines a multi-container setup with three services: pgAdmin, PostgreSQL, and Keycloak.

Services:

1. pgadm:
   - Runs pgAdmin4 image.
   - Environment variables set the default email and password for logging into pgAdmin.
   - Exposes port 9081 (in localhost) to port 80 (inside the container).
   - Connects to the localnet network.
3. postgres:
   - Runs a PostgreSQL version 15.4 (lightweight Alpine version) image.
   - Stores database data on the host machine (./dbdata/).
   - Exposes port 5432 for database connections.
   - Environment variables set the database username and password.
4. kc:
   - Runs Keycloak version 26 image.
   - Runs Keycloak in development mode with specific ports (8484 for HTTP and 8444 for HTTPS).
   - Depends on the PostgreSQL service.
   - Various environment variables configure database connections, Keycloak admin credentials, and other settings.
   - Exposes ports 8484 and 8444 for external access.
   - Connects to the localnet network.
5. Networks:
   - localnet: Defines a custom Docker network to allow the containers to communicate with each other.

#### Step2 - Creates and starts containers
Start all services with Docker Compose:
```
docker compose up -d
```
> [!NOTE]
> This command will download the necessary images if they aren’t already present and start Keycloak, PostgreSQL, and PgAdmin4 containers.

#### Step3 - Access Keycloak and PgAdmin
- **Keycloak**: Open http://localhost:8484 and log in with the admin credentials defined in docker-compose.yml
- **PgAdmin4**: Open http://localhost:9081 and log in using the email and password specified for PgAdmin4 in docker-compose.yml

Once logged into PgAdmin4:

1. Add a new server connection.
2. Set these values in connection section to connect to the PostgreSQL instance:


| Variable      | Value                                                |
| ------------- | -----------------------------------------------------|
| host          | host.docker.internal                                 |
| username      | POSTGRES_USER (defined in docker-compose.yml)        |
|password       |  POSTGRES_PASSWORD (defined in docker-compose.yml)   |

To verify that the database is updated correctly, create a new realm in Keycloak. Then, in pgAdmin, query the realms table and confirm that the newly created realm appears in the results
```
select * from realm
```

