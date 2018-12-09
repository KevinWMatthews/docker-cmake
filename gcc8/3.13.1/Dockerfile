FROM gcc:8.2.0 as build_cmake

WORKDIR /usr/src
RUN curl \
    --location https://github.com/Kitware/CMake/releases/download/v3.13.1/cmake-3.13.1-Linux-x86_64.sh \
    -o curl-install.sh
RUN chmod u+x curl-install.sh
RUN ./curl-install.sh --skip-license --prefix=/usr/local

FROM gcc:8.2.0

# Copy executables
WORKDIR /usr/local/bin
COPY --from=build_cmake /usr/local/bin/ccmake .
COPY --from=build_cmake /usr/local/bin/cmake .
COPY --from=build_cmake /usr/local/bin/cmake-gui .
COPY --from=build_cmake /usr/local/bin/cpack .
COPY --from=build_cmake /usr/local/bin/ctest .

# Copy limited set of shared resources
WORKDIR /usr/local/share
COPY --from=build_cmake /usr/local/share/cmake-3.13/ /usr/local/share/cmake-3.13/

WORKDIR /usr/src
