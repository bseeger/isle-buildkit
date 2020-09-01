# syntax=docker/dockerfile:experimental
FROM local/nginx:latest

RUN --mount=id=downloads,type=cache,target=/opt/downloads \
    DOWNLOAD_CACHE_DIRECTORY="/opt/downloads" && \
    DRUSH_VERSION="0.6.0" && \
    DRUSH_FILE="drush.phar" && \
    DRUSH_URL="https://github.com/drush-ops/drush-launcher/releases/download/${DRUSH_VERSION}/${DRUSH_FILE}" && \
    DRUSH_SHA256="c3f32a800a2f18470b0010cd71c49e49ef5c087f8131eecfe9b686dc1f3f3d4e" && \
    download.sh --url "${DRUSH_URL}" --sha256 "${DRUSH_SHA256}" "${DOWNLOAD_CACHE_DIRECTORY}" && \
    cp "${DOWNLOAD_CACHE_DIRECTORY}/${DRUSH_FILE}" /usr/bin/drush && \
    chmod a+x /usr/bin/drush && \
    mkdir -p /var/www/drupal/config && \
    mkdir -p /var/www/drupal/web/libraries && \
    chown -R nginx:nginx /var/www && \
    mkdir -p /opt/keys/jwt && \
    chown nginx:nginx /opt/keys/jwt

    ARG WITH_XDEBUG=true

    COPY ssh/insecure_id_rsa.pub /root/insecure_id_rsa.pub

    RUN if [ ${WITH_XDEBUG} = "true" ] ; then \
        mkdir -p /root/.ssh && \
        mv /root/insecure_id_rsa.pub /root/.ssh/authorized_keys && \
        chown -R root:root /root/.ssh && \
        chmod 700 /root/.ssh && \
        chmod 600 /root/.ssh/authorized_keys && \
        #rm -rf ssh/insecure_id_rsa.pub && \ 
        apk add php7-xdebug --repository http://dl-3.alpinelinux.org/alpine/edge/testing && \
        apk add openssh-server iputils netcat-openbsd shadow && \
        mkdir -p /var/run/sshd && \
        echo "zend_extension=xdebug.so" >> /etc/php7/conf.d/xdebug.ini && \
        echo "xdebug.coverage_enable=0" >> /etc/php7/conf.d/xdebug.ini && \
        echo "xdebug.remote_enable=1"   >> /etc/php7/conf.d/xdebug.ini && \
        echo "xdebug.remote_connect_back=1" >> /etc/php7/conf.d/xdebug.ini && \
        echo "xdebug.remote_log=/tmp/xdebug.log" >> /etc/php7/conf.d/xdebug.ini && \
        echo "xdebug.remote_autostart=true" >> /etc/php7/conf.d/xdebug.ini && \
        /usr/bin/ssh-keygen -A && \
        usermod -U root; fi
        #CMD ["/usr/sbin/sshd &"]

WORKDIR /var/www/drupal

COPY rootfs /

EXPOSE 80 22
