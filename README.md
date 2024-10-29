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

docker-compose.yml
```command


version: '3'
 
services:
  pgadm:
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_PASSWORD: 123password
      PGADMIN_DEFAULT_EMAIL: test@example.com
      PGADMIN_DISABLE_POSTFIX: true
    ports:
      - 9081:80
    networks:
      - localnet
 
  postgres:
    image: postgres:15.4-alpine3.18
    volumes:
      - ./dbdata/:/var/lib/postgresql/data/
    
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: Pwd123
    networks:
      - localnet
 
  kc:
    image: quay.io/keycloak/keycloak:26.0.0
    container_name: Keycloak
    command: |
      start-dev
      --http-port 8484 
      --https-port 8444
      --metrics-enabled=true
      --log-level="INFO"

    volumes:
      - ./providers/:/opt/keycloak/providers/
  
    depends_on:
      - postgres
    environment:
      KC_DB: postgres
      KC_DB_URL_HOST: postgres
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: Pwd123
      KC_HEALTH_ENABLED: false
      KC_METRICS_ENABLED: false
      KC_HOSTNAME_STRICT: false
      KC_PROXY_ADDRESS_FORWARDING: true
      KC_HTTP_ENABLED: true
      QUARKUS_HTTP_ACCESS_LOG_ENABLED: true
      KC_PROXY: edge
      KC_HOSTNAME_STRICT_HTTPS: false

    ports:
      - 8444:8444
      - 8484:8484
    networks:
      - localnet



networks:
  localnet:
    name: localnet
```
#### Explication of docker-compose.yml
### Creates and starts containers
```
docker compose up
```



