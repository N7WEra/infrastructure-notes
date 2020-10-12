# Docker

## Enumeration

View running containers:

`docker ps`

find local docker images:

`docker image ls`

Check if we are inside a container by running \(if the file exists we are inside a docker\):

`ls -la /.dockerenv`

## Run container

Run a container from the Alpine version 3.9 image, name the running container “web” and expose port 5000 externally, mapped to port 80 inside the container:

```text
docker container run - is the new command.
docker run - is still the old one, which will be deprecated soon, I guess.
```

`docker container run --name web -p 5000:80 alpine:3.9`

List the running containers \(add --all to include stopped containers\) 

`docker container ls`

Run docker in tty mode:

```text
docker run -it debian:buster /bin/bash
```

-i, --interactive \# attach stdin \(interactive\) 

-t, --tty \# pseudo-tty

## Privilege escalation

If we have a user which is part of the `docker` group we can mount the file system to the docker and see all files

mount the file system:

`docker run -v /:/hostOS -i -t [image name] bash`

* `-v /:/hostOS` - mount the host’s `/` as `/hostOS` inside the image
* `-i` - interactive
* `-t` - create a tty
* `[image name]` - the name of the image to run, got from `docker ps above`
* `bash` - command to run

