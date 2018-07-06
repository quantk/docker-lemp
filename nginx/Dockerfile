# ARG HOST_USER_UID=1000
# ARG HOST_USER_GID=1000

FROM nginx:latest

RUN set -ex                                                 && \
                                                               \
    echo 'Creating notroot user and group from host'        && \
    groupadd -g 1000 dockuser                      && \
    useradd -lm -u 1000 -g 1000 dockuser