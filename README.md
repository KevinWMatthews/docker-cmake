# Docker CMake

This Git repo contains Dockerfiles for generating images with CMake and gcc.


## Build Image

Adjust to the desired version:
```
$ docker build -t cmake-3.5.1 3.5.1/
```


## Run Container


### Build In Container

To compile source code on the host machine using the container's tools,
you must mount (bind) the source directory to the machine.
Per gcc's example:
```
$ docker run \
    --rm --name cmake-3.5.1 \
    -it \
    --mount type=bind,src="$PWD",dest=/usr/src/<source_repo> \
    -w /usr/src/<source_repo> \
    cmake-3.5.1 \
    bash
```
Of course, replace bash with the command of your choosing.
This works but compiles all source code as root, which can be problematic when later accessing files on the host system.


#### Permissions when Starting Container

For security reasons, Docker containers typically are not run as root.

This image has a default user built in:
```
ARG userid=1000
```

This will run the container with userid 1000 and group 1000 by default.

This can be overridden when the image is built and/or the container is run.

To set the image's default user, use:
```
$ docker build --build-arg userid=<your_userid> <docker_directory>
```

To override the user id at runtime only, use:
```
$ docker run --user <uid:gid>
```

It can be convenient to use the option:
```
--user $(id --user):$(id --group)
```



### Example Run Command

This will run the container as the current user:
```
$ docker run \
    --rm --name cmake-3.5.1 \
    -it \
    --user $(id --user):$(id --group) \
    --mount type=bind,src=/path/to/host/source,dst=/path/to/container/source \
    -w /path/to/container/source \
    cmake-3.5.1 \
    bash
```



### Shell Into Container

To explore the contents of a container, run
```
$ docker run --rm --name cmake-3.5.1 -it cmake-3.5.1 bash
```


## Additional Resources


### gcc Build Examples

  * [gcc's Docker Hub](https://hub.docker.com/_/gcc/) or [gcc's Docker Store](https://store.docker.com/images/gcc)
  * Four-part series on [Docker for embedded developers](https://blog.feabhas.com/2017/09/introduction-docker-embedded-developers-part-1-getting-started/).
    - Strong introduction to Docker.
    - Seems to ignore the permissions issue?
  * [C build environment](https://ownyourbits.com/2017/06/20/c-build-environment-in-a-docker-container/) in a Docker container.
    - Custom image build from Dockerfile.
    - User is hard-coded in Dockerfile.
  * [Debian build environment](https://ownyourbits.com/2017/06/24/debian-build-environment-in-a-docker-container/) in a Docker container.
    - Background info for C build environment link.


### Container Permissions

  * Blog post on using [gosu in a Dockerfile](https://denibertovic.com/posts/handling-permissions-with-docker-volumes/) to set container permissions.
    - The first comment provides an alternate solution:
        `docker run -ti \`
        `-v /etc/group:/etc/group:ro -v /etc/passwd:/etc/passwd:ro \`
        `-u $( id -u $USER ):$( id -g $USER ) \`
        `some-image:lastest bash`
  * Random [GitHub repo](https://github.com/schmidigital/permission-fix). Untested.
  * Blog post using the [nobody user's uid](https://blog.csanchez.org/2017/01/31/running-docker-containers-as-non-root/) when launching the container.
