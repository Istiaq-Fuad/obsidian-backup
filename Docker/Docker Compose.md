## 1. Purpose of Docker Compose

Docker Compose is used to define and run multi-container applications.  
It allows you to declare services, networks, and volumes in a single YAML file and manage them with simple commands.

Use Compose when:  
You need multiple services (app, database, cache, proxy)  
You want reproducible local or server environments  
You want to manage service dependencies and shared networks easily

## 2. File naming and versions

Default file name is `docker-compose.yml`.  
Alternative files can be used with `-f`.

Modern Compose no longer requires a `version:` field.  
If used, keep it compatible (for example `3.8`).

## 3. Basic structure of a Compose file

Top-level keys typically include:  
services  
volumes  
networks  
configs  
secrets

Minimal example:

```yaml
services:
  app:
    image: nginx
```

## 4. Defining services

Each service represents a container.

Key service options:  
image  
build  
container_name  
command  
entrypoint  
environment  
env_file  
ports  
expose  
volumes  
depends_on  
restart  
healthcheck  
user

Example:

```yaml
services:
  app:
    image: node:20
    container_name: web_app
    working_dir: /app
    command: node index.js
```

## 5. Image vs build

Use `image` when pulling from a registry.  
Use `build` when creating an image from a Dockerfile.

Example using build:

```yaml
services:
  api:
    build:
      context: .
      dockerfile: Dockerfile
```

Avoid mixing `image` and `build` unless tagging the built image.

## 6. Environment variables

Use environment variables for configuration, not secrets.

Inline environment:

```yaml
environment:
  NODE_ENV: production
  PORT: 3000
```

Using env file:

```yaml
env_file:
  - .env
```

Do not commit `.env` files containing secrets.

## 7. Ports and networking

`ports` maps host ports to container ports.  
`expose` makes ports visible only inside Docker networks.

```yaml
ports:
  - "8080:80"

expose:
  - "5432"
```

Services can communicate by service name as hostname.

## 8. Volumes and persistence

Use volumes for persistent data such as databases.

Named volume:

```yaml
volumes:
  - db_data:/var/lib/postgresql/data
```

Bind mount (development):

```yaml
volumes:
  - ./src:/app
```

Define volumes at top level:

```yaml
volumes:
  db_data:
```

## 9. Networks

Compose automatically creates a default network.  
Custom networks are useful for isolation and control.

```yaml
networks:
  backend:
    driver: bridge
```

Assign services:

```yaml
services:
  api:
    networks:
      - backend
```

## 10. Service dependencies

`depends_on` controls startup order only.  
It does not wait for services to be ready.

```yaml
depends_on:
  - db
```

Combine with healthchecks for readiness logic.

## 11. Healthchecks

Healthchecks allow Compose to detect service health.

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:3000"]
  interval: 30s
  timeout: 5s
  retries: 3
```

Use healthchecks for databases and APIs.

## 12. Restart policies

Control container restart behavior.

```yaml
restart: unless-stopped
```

Common values:  
no  
always  
on-failure  
unless-stopped

## 13. Security best practices

Run services as non-root when possible.  
Avoid hardcoding secrets.  
Use minimal base images.  
Restrict exposed ports.  
Use separate networks for public and private services.

## 14. Profiles

Profiles allow enabling or disabling services.

```yaml
profiles:
  - dev
```

Run specific profile:  
`docker compose --profile dev up`

## 15. Scaling services

Scale stateless services.

```bash
docker compose up --scale worker=3
```

Do not scale stateful services like databases without clustering support.

## 16. Override files

Use override files for environment-specific configuration.

Default override:  
docker-compose.override.yml

Example use:  
Development overrides for volumes and ports  
Production overrides for resources and scaling

## 17. Logging configuration

Control logging drivers and limits.

```yaml
logging:
  driver: json-file
  options:
    max-size: "10m"
    max-file: "3"
```

## 18. Example: web + database stack

```yaml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    env_file:
      - .env

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: appdb
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

## 19. Lifecycle commands reference

`docker compose up -d`  
Start services in detached mode

`docker compose down`  
Stop and remove containers and networks

`docker compose logs -f`  
View logs

`docker compose exec service shell`  
Execute command in a running service

`docker compose ps`  
List services

## 20. Common mistakes to avoid

Using latest tags blindly  
Hardcoding secrets  
Using bind mounts in production  
Assuming depends_on means readiness  
Running everything as root  
Mixing dev and prod config in one file

## 21. Quick checklist

Clear service separation  
Minimal exposed ports  
Persistent volumes defined  
Networks properly segmented  
Healthchecks present  
Restart policy configured  
Secrets handled securely  
Profiles or overrides used  
Compose file readable and documented
