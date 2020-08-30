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
