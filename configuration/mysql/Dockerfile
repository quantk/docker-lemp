ARG MYSQL_VERSION=5.6
FROM mysql:${MYSQL_VERSION}

ENV HOST_USER_UID=1000
ENV HOST_USER_GID=1000

RUN set -ex                                                 && \
                                                               \
    echo 'Creating notroot user and group from host'        && \
    groupadd -g ${HOST_USER_UID} dockuser                      && \
    useradd -lm -u ${HOST_USER_UID} -g ${HOST_USER_GID} dockuser