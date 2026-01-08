## docker ps

Lists running containers.  
Shows container ID, image, command, uptime, ports, and names.  
Useful for checking which containers are currently active.

`docker ps`  
Lists only running containers.

`docker ps --format "{{.ID}} {{.Names}} {{.Status}}"`  
Shows selected fields for quick readability.

## docker ps --all

Lists every container, including stopped ones.  
Helpful for checking containers created earlier and diagnosing failures.  
Stopped containers remain on the system until removed.

`docker ps --all`  
Full list of running and exited containers.

`docker ps -aq`  
Shows only container IDs for scripting or bulk operations.

## docker create

Creates a container from an image but does not start it.  
Useful when you want to configure volumes, networks or environment variables before running.

`docker create --name mycontainer ubuntu`  
Creates a container named _mycontainer_ from the ubuntu image.

`docker create -p 8080:80 nginx`  
Creates a container with port mapping but does not run it yet.

## docker start

Starts one or more previously created or stopped containers.  
Does not create a new container; resumes an existing one.

`docker start mycontainer`  
Starts container named _mycontainer_.

`docker start $(docker ps -aq)`  
Starts all containers.

## docker run

Creates and starts a container in a single step.  
Most frequently used command for launching applications.  
Accepts many options such as port mapping, environment variables, volumes, interactive mode and more.

`docker run ubuntu`  
Runs an ubuntu container and exits if no command is kept running.

`docker run -it ubuntu bash`  
Interactive shell.

`docker run -d -p 8080:80 nginx`  
Detached mode web server. Here `8080` is local port and `80` is docker internal port.

`docker run --name myapp -v data:/data alpine ls /data`  
Runs a container with a named volume.

## docker logs

Shows logs from a running or stopped container.  
Helpful for debugging errors, startup issues, and application output.

`docker logs mycontainer`  
Fetches standard output logs.

`docker logs -f mycontainer`  
Follows logs in real time.

`docker logs --tail 100 mycontainer`  
Shows only the last 100 lines.

## docker stop

Stops a running container gracefully.  
Sends a SIGTERM signal, giving the process time to shut down cleanly before SIGKILL is used.

`docker stop mycontainer`  
Gracefully stops the container.

`docker stop $(docker ps -q)`  
Stops all running containers.

## docker kill

Immediately stops a running container by sending SIGKILL.  
Useful if a process becomes unresponsive.  
Faster but not graceful compared to docker stop.

`docker kill mycontainer`  
Forces the container to stop instantly.

`docker kill $(docker ps -q)`  
Kills all containers instantly.

## docker exec -it

Executes commands inside a running container.  
`-i` keeps STDIN open  
`-t` allocates a pseudo-terminal  
Very useful for debugging or interacting with applications.

`docker exec -it mycontainer bash`  
Opens an interactive shell.

`docker exec -it mycontainer sh`  
Used when bash is not available.

`docker exec mycontainer ls /usr/local`  
Runs a command without opening a shell.

## docker build

Builds a Docker image from a Dockerfile.  
Used for packaging your source code into reproducible images.  
Outputs intermediate steps and caching information.

`docker build .`  
Builds an image from Dockerfile in the current directory.

`docker build -t myimage:latest .`  
Tags the built image for easier reference.

`docker build -f custom.Dockerfile -t app:v1 .`  
Uses a custom Dockerfile.

## docker commit

Creates a new image from an existing container.  
Captures the current filesystem state.  
Useful for preserving manual changes, though not ideal for production workflows.

`docker commit mycontainer myimage:v1`  
Creates an image reflecting the container’s current state.

`docker commit -m "Added tools" -a "Fuad" mycontainer workspace:image`  
Adds a commit message and author info.

## docker compose up

Creates networks, volumes, and containers, then starts services.

`docker compose up`  
Starts services in foreground.

`docker compose up -d`  
Starts services in detached mode.

`docker compose up --build`  
Forces rebuild of images before starting.

## docker compose down

Stops and removes containers, default networks, and resources created by Compose.

`docker compose down`  
Stops and cleans up services.

`docker compose down -v`  
Also removes named volumes.

`docker compose down --rmi all`  
Removes images used by the services.

## docker compose start

Starts existing stopped services.

`docker compose start`  
Starts all stopped services.

## docker compose stop

Stops running services without removing containers.

`docker compose stop`  
Gracefully stops services.

## docker compose restart

Stops and then starts services again.

`docker compose restart`  
Restarts all services.

`docker compose restart web`  
Restarts a specific service.

## docker compose ps

Lists containers managed by the current Compose project.

`docker compose ps`  
Shows service status.

## docker compose logs

Displays logs from services.

`docker compose logs`  
Shows logs.

`docker compose logs -f`  
Follows logs in real time.

`docker compose logs web`  
Shows logs for a single service.

## docker compose exec

Runs a command inside a running service container.

`docker compose exec app bash`  
Opens an interactive shell.

`docker compose exec db psql -U user`  
Executes a command directly.

## docker compose run

Runs a one-off command in a new container.

`docker compose run app npm test`  
Runs a temporary container for a task.

`docker compose run --rm app bash`  
Removes the container after exit.

## docker compose build

Builds or rebuilds service images.

`docker compose build`  
Builds all services.

`docker compose build api`  
Builds a specific service.

## docker compose pull

Pulls images from registries.

`docker compose pull`  
Pulls all service images.

## docker compose config

Validates and displays the final Compose configuration.

`docker compose config`  
Checks syntax and resolves variables.

## docker compose top

Displays running processes inside service containers.

`docker compose top`  
Shows process list.

