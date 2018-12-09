# gcc and CMake in Docker

CMake installed to the official gcc Docker image.


## Tags and `Dockerfile` links

Images are tagged with gcc version first and CMake version second, so `gcc8` with `CMake 3.13.1` is `8-3.13.1`:

  * [gcc-cmake:8-3.13.1](https://github.com/KevinWMatthews/gcc-cmake/blob/master/gcc8/3.13.1/Dockerfile)
  * [gcc-cmake:7-3.7.2](https://github.com/KevinWMatthews/gcc-cmake/blob/master/gcc7/3.7.2/Dockerfile)
  * [gcc-cmake:5-3.5.1](https://github.com/KevinWMatthews/gcc-cmake/blob/master/gcc5/3.5.1/Dockerfile)

These images are updated as changes to the Dockerfiles are made.

Additionally, images are tagged with a date stamp `YYYYMMDD`:

  * gcc-cmake:8-3.13.1-20181209

These images are built [from tags](https://github.com/KevinWMatthews/gcc-cmake/tags)
in the source repo and are stable.

Here is a complete list of [Docker tags](https://hub.docker.com/r/kevinwmatthews/gcc-cmake/tags/).


### Build In Container

For rapid development, bind mount your source code build directories to the
container:

```bash
$ docker run \
    --rm -it \
    --user $(id --user):$(id --group) \
    --mount type=bind,src=/path/to/source/dir,dst=/usr/src/<source_dir> \
    --mount type=bind,src=/path/to/build/dir,dst=/usr/src/<build_dir> \
    --workdir /usr/src/<build_dir> \
    <image> \
    <command>
```

This will run the container, execute `<command>`, and exit the container.

Leave `<command>` blank to shell into the container.


### Permissions

When mounting code to the container, it is advisable to run the container
with current system user ID. If the container is run as root, all files
that the container touches in the source and build directories will be owned by root.

This can be prevented with `docker run`'s `--user` option. On Ubuntu Linux, use:

```bash
--user $(id --user):$(id --group)
```
or something similar.


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
