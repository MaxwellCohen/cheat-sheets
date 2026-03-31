# Docker Commands Cheat Sheet

## Basic Info

- `docker --version` - Show Docker version
- `docker info` - Show system-wide Docker information
- `docker help` - Show all Docker commands
- `docker <command> --help` - Help for a specific command

## Images

- `docker images` - List local images
- `docker pull <image>:<tag>` - Download image from registry
- `docker build -t <name>:<tag> .` - Build image from `Dockerfile`
- `docker rmi <image>` - Remove an image
- `docker image prune` - Remove dangling images
- `docker image prune -a` - Remove all unused images

## Containers

- `docker ps` - List running containers
- `docker ps -a` - List all containers
- `docker run <image>` - Run a container
- `docker run -it <image> sh` - Start interactive shell
- `docker run -d --name <name> <image>` - Run in detached mode with name
- `docker run -p 8080:80 <image>` - Map host port to container port
- `docker run -v <host_path>:<container_path> <image>` - Mount volume/bind path
- `docker start <container>` - Start stopped container
- `docker stop <container>` - Stop running container
- `docker restart <container>` - Restart container
- `docker rm <container>` - Remove container
- `docker rm -f <container>` - Force remove running container

## Logs and Debugging

- `docker logs <container>` - Show container logs
- `docker logs -f <container>` - Follow logs live
- `docker exec -it <container> sh` - Open shell in running container
- `docker inspect <container_or_image>` - Detailed JSON metadata
- `docker stats` - Live resource usage
- `docker top <container>` - Show running processes in container

## Copy Files

- `docker cp <container>:<path> <host_path>` - Copy from container to host
- `docker cp <host_path> <container>:<path>` - Copy from host to container

## Networks

- `docker network ls` - List networks
- `docker network create <network_name>` - Create network
- `docker network inspect <network_name>` - Inspect network details
- `docker network connect <network_name> <container>` - Connect container to network
- `docker network disconnect <network_name> <container>` - Disconnect container from network
- `docker network rm <network_name>` - Remove network

## Volumes

- `docker volume ls` - List volumes
- `docker volume create <volume_name>` - Create volume
- `docker volume inspect <volume_name>` - Show volume details
- `docker volume rm <volume_name>` - Remove volume
- `docker volume prune` - Remove unused volumes

## Cleanup

- `docker container prune` - Remove stopped containers
- `docker network prune` - Remove unused networks
- `docker volume prune` - Remove unused volumes
- `docker system prune` - Remove unused data (safe-ish)
- `docker system prune -a --volumes` - Aggressive cleanup (includes images + volumes)

## Docker Compose (v2)

- `docker compose version` - Show Compose version
- `docker compose up` - Start services
- `docker compose up -d` - Start services in background
- `docker compose down` - Stop and remove services
- `docker compose down -v` - Remove services and named volumes
- `docker compose ps` - List compose containers
- `docker compose logs` - Show service logs
- `docker compose logs -f` - Follow service logs
- `docker compose build` - Build/rebuild services
- `docker compose exec <service> sh` - Run shell in service container

## Useful One-Liners

- `docker stop $(docker ps -q)` - Stop all running containers
- `docker rm $(docker ps -aq)` - Remove all containers
- `docker rmi $(docker images -q)` - Remove all images
- `docker system df` - Show Docker disk usage

