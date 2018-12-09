FROM gcc:5

RUN curl https://cmake.org/files/v3.5/cmake-3.5.1-Linux-x86_64.sh -o /tmp/curl-install.sh \
    && chmod u+x /tmp/curl-install.sh \
    && /tmp/curl-install.sh --skip-license --prefix=/usr/local \
    && rm /tmp/curl-install.sh
