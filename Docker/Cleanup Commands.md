## docker rm

Removes one or more stopped containers.  
Does not delete running containers unless forced.

`docker rm container_id`  
Removes a stopped container.

`docker rm -f container_id`  
Forces removal even if the container is running.

`docker rm $(docker ps -aq)`  
Removes all containers.

## docker rmi

Removes one or more images.  
Images cannot be deleted if containers still depend on them.

`docker rmi image_id`  
Removes a specific image.

`docker rmi $(docker images -q)`  
Removes all images.

`docker rmi -f image_id`  
Forces deletion even if referenced.

## docker volume rm

Deletes unused Docker volumes.  
Volumes remain on disk unless removed manually.

`docker volume rm volume_name`  
Deletes a named volume.

`docker volume rm $(docker volume ls -q)`  
Removes all volumes.

`docker volume prune`  
Removes only unused volumes.

## docker network rm

Deletes Docker networks.

`docker network rm network_name`  
Removes a specific network.

`docker network prune`  
Removes networks not used by any containers.

## docker system prune

Cleans up unused resources.  
By default removes stopped containers, dangling images, unused networks and build cache.

`docker system prune`  
Interactive removal of unused objects.

`docker system prune -f`  
Runs without confirmation.

`docker system prune -a`  
Removes all unused images as well; aggressive cleanup.

## docker builder prune

Removes build cache from Docker builds.

`docker builder prune`  
Clears build cache.

`docker builder prune -a`  
Removes all intermediate layers and cached data.

## docker system df

Displays disk usage of Docker components.  
Useful before deciding what to clean.

`docker system df`  
Shows summary.

`docker system df -v`  
Detailed view.