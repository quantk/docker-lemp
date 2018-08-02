ARG NGINX_VERSION=latest
FROM nginx:${NGINX_VERSION}

ENV HOST_USER_UID=1000
ENV HOST_USER_GID=1000

RUN set -ex                                                 && \
                                                               \
    echo 'Creating notroot user and group from host'        && \
    groupadd -g ${HOST_USER_UID} dockuser                      && \
    useradd -lm -u ${HOST_USER_UID} -g ${HOST_USER_GID} dockuser

COPY ./default_page.html /var/www/html/index.html