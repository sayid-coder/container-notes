# Containerization Notes
My notes on containers, docker, and K8s.

# Docker Commands
- **Checking Docker Version: `docker version`**

## Registry & Repository Commands
A repository is a *hosted* collection of tagged images that together create the file system for a container. A registry is a *host* (a server) that stores repositories and provides an HTTP API for [managing the uploading and downloading of repositories](https://docs.docker.com/engine/tutorials/dockerrepos/).
Docker.com hosts its own [index](https://hub.docker.com/) to a central registry which contains a large number of repositories.

- [`**docker login**`](https://docs.docker.com/engine/reference/commandline/login) to login to a registry.
- [**`docker logout`**](https://docs.docker.com/engine/reference/commandline/logout) to logout from a registry.
- [**`docker search`**](https://docs.docker.com/engine/reference/commandline/search) searches registry for image.
- [**`docker pull`**](https://docs.docker.com/engine/reference/commandline/pull) pulls an image from registry to local machine.
- [**`docker push`**](https://docs.docker.com/engine/reference/commandline/push) pushes an image to the registry from local machine.

## Image Commands
- [**`docker images`**](https://docs.docker.com/engine/reference/commandline/images) shows all images.
    - **[`docker image ls`](https://docs.docker.com/engine/reference/commandline/image_ls/)** lists images.
- [**`docker import`**](https://docs.docker.com/engine/reference/commandline/import) creates an image from a tarball.
- [**`docker build`**](https://docs.docker.com/engine/reference/commandline/build) creates image from Dockerfile.
- [**`docker commit`**](https://docs.docker.com/engine/reference/commandline/commit) creates image from a container, pausing it temporarily if it is running.
- [**`docker rmi`**](https://docs.docker.com/engine/reference/commandline/rmi) removes an image.
- [**`docker load`**](https://docs.docker.com/engine/reference/commandline/load) loads an image from a tar archive as STDIN, including images and tags.
- [`**docker save**`](https://docs.docker.com/engine/reference/commandline/save) saves an image to a tar archive stream to STDOUT with all parent layers, tags & versions.
    - [**`docker history`**](https://docs.docker.com/engine/reference/commandline/history) shows history of image.
    - [**`docker tag`**](https://docs.docker.com/engine/reference/commandline/tag) tags an image to a name (local or registry).

## Container Commands
- [**`docker ps`**](https://docs.docker.com/engine/reference/commandline/ps) shows running containers.
- **[`docker container ls`](https://docs.docker.com/engine/reference/commandline/container_ls/)** check if the container is running. The output tells us:
  - Which image the container is running; a short form of the container ID that Docker uniquely generates;
  - The container name that Docker will randomly assign unless we supply a name;
  - The command running in the container.
- [`**docker create**`](https://docs.docker.com/engine/reference/commandline/create) creates a container but does not start it.
- [**`docker run`**](https://docs.docker.com/engine/reference/commandline/run) creates and starts a container in one operation.
  - Normally if you run a container without options it will start and stop immediately.
  - If you want keep it running you can run `**docker run -it container_id --name friendly-name**`
    - `-i` will keep the STDIN open even if not attached.
    - `-t` will allocate a command line session (pseudo-TTY).
    - `-d` will detach the container from the current session and run it in the background.
    - `--name` will assign a friendly name to the container.
  - If you want to map a directory on the host to a docker container: `**docker run -v $HOSTDIR:$DOCKERDIR**`
  - If you want a transient container, `**docker run --rm**` will remove the container after it stops.
    - `**docker run --name yourname docker_image**` will allow you to start and stop a container by calling it with the name.
    - Create a new container from the "**nginx**" image and run it in the background with the **`--detach`** flag and with port 80 published with the **`--publish`** flag.
      - **`docker container run --detach --publish 80:80 nginx:alpine`**
- [**`docker rename`**](https://docs.docker.com/engine/reference/commandline/rename/) allows the container to be renamed.
- [**`docker rm`**](https://docs.docker.com/engine/reference/commandline/rm) deletes a container.
  - If you want to remove the volumes associated with the container, the deletion of the container must include the `-v` switch.
- [**`docker update`**](https://docs.docker.com/engine/reference/commandline/update/) updates a container's resource limits.
- [**`docker start`**](https://docs.docker.com/engine/reference/commandline/start) starts a container so it is running.
  - If you want to integrate a container with a [host process manager](https://docs.docker.com/engine/admin/host_integration/), start the daemon with `-r=false` then use `docker start -a`.
- [**`docker stop`**](https://docs.docker.com/engine/reference/commandline/stop) stops a running container.
- [**`docker restart`**](https://docs.docker.com/engine/reference/commandline/restart) stops and starts a container.
- [**`docker pause`**](https://docs.docker.com/engine/reference/commandline/pause/) pauses a running container, "freezing" it in place.
- [**`docker unpause`**](https://docs.docker.com/engine/reference/commandline/unpause/) will unpause a running container.
- [**`docker wait`**](https://docs.docker.com/engine/reference/commandline/wait) blocks until running container stops.
- [**`docker kill`**](https://docs.docker.com/engine/reference/commandline/kill) sends a SIGKILL to a running container.
- [`**docker attach**`](https://docs.docker.com/engine/reference/commandline/attach) will connect to a running container.
- [**`docker logs`**](https://docs.docker.com/engine/reference/commandline/logs) gets logs from container. (You can use a custom log driver, but logs is only available for `json-file` and `journald` in 1.10).
- [**`docker inspect`**](https://docs.docker.com/engine/reference/commandline/inspect) looks at all the info on a container (including IP address).
- [**`docker events`**](https://docs.docker.com/engine/reference/commandline/events) gets events from container.
- [**`docker port`**](https://docs.docker.com/engine/reference/commandline/port) shows public facing port of container.
- [**`docker top`**](https://docs.docker.com/engine/reference/commandline/top) shows running processes in container.
- [**`docker stats`**](https://docs.docker.com/engine/reference/commandline/stats) shows containers' resource usage statistics.
- [**`docker diff`**](https://docs.docker.com/engine/reference/commandline/diff) shows changed files in the container's FS.
- **`docker ps -a`** shows running and stopped containers.
- **`docker stats --all`** shows a running list of containers.
- [**`docker cp`**](https://docs.docker.com/engine/reference/commandline/cp) copies files or folders between a container and the local filesystem.
- [**`docker export`**](https://docs.docker.com/engine/reference/commandline/export) turns container filesystem into tarball archive stream to STDOUT.
- [**`docker exec`**](https://docs.docker.com/engine/reference/commandline/exec) to execute a command in a container.
  - To enter a running container, attach a new shell process to a running container called foo, use: `**docker exec -it foo /bin/bash**`
