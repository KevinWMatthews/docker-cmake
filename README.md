# Docker CMake

This Git repo contains Dockerfiles for generating images with CMake and gcc.

## Build

Adjust to the desired version:

```
$ docker build -t cmake-3.5.1 3.5.1/
```

## Run

To explore the container in a shell, run
```
$ docker run --rm --name cmake-3.5.1 cmake-3.5.1 bash
```

Examples of how to compile source code can be found from the [gcc container](https://hub.docker.com/_/gcc/).
