# Docker tips

## Run

```bash
> docker container run --name web -p 5000:80 alpine:3.9
```

Run a container from the Alpine version 3.9 image, name the running container “web” and expose port 5000 externally,
mapped to port 80 inside the container.

## Start and stop

```bash
> docker container start web
```

Start a stopped container named `web`.

## Check containers

```bash
> docker container ls
```

List all running containers

```bash
> docker container ls -a
```

List all containers (running and stopped)

## Interactive mode

```bash
> docker container run -it --name webserver nginx
```

Run docker container nginx in interactive mode (_-it_). This allows to see the image output directly in the shell.

To have the complete control of the image (accessing it shell), we need to pass an argument (_bash_) to the image:

```bash
> docker container run -it --name webserver nginx bash
```

To access an already running docker image, we need to use `exec` command:

```bash
> docker container exec -it webserver bash
```

## Networking

`-p source:dest` makes a port forwarding from the image to localhost, so we can expose a port inside docker image (the port where the app/db/whatever is running inside the image) to a local port, and access to it.

```bash
> docker container port webserver
```

shows the port mapping of that container.

## Access container informations

```bash
> docker container inspect webserver
```

Returns a json formatted list of informations about selected container (config, newtwork, etc)it.

To filter only some needed informations, we can use `--format` argument:

```bash
 docker container inspect --format '{{ .NetworkSettings.IPAddress}}' webhost
```

Return only the container's ip addres (see the json structure to understand how to get other infos).
