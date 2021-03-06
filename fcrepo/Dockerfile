# syntax=docker/dockerfile:experimental
FROM local/tomcat:latest

RUN --mount=id=downloads,type=cache,target=/opt/downloads \
    DOWNLOAD_CACHE_DIRECTORY="/opt/downloads" && \
    FCREPO_VERSION="5.1.0" && \
    FCREPO_FILE="fcrepo-webapp-${FCREPO_VERSION}.war" && \
    FCREPO_URL="https://github.com/fcrepo4/fcrepo4/releases/download/fcrepo-${FCREPO_VERSION}/${FCREPO_FILE}" && \
    FCREPO_SHA256="fdcb43cfd1468a84ddb89c20e4f4c7f54476ab9a24f69beb335d26f2b58ecec5" && \
    download.sh --url "${FCREPO_URL}" --sha256 "${FCREPO_SHA256}" "${DOWNLOAD_CACHE_DIRECTORY}" && \
    install-war-into-tomcat.sh --name "fcrepo" --file "${DOWNLOAD_CACHE_DIRECTORY}/${FCREPO_FILE}" && \
    SYN_VERSION="1.1.0" && \
    SYN_FILE="islandora-syn-${SYN_VERSION}-all.jar" && \
    SYN_URL="https://github.com/Islandora-CLAW/Syn/releases/download/v${SYN_VERSION}/${SYN_FILE}" && \
    SYN_SHA256="bcad5f872930b1bcc9ea4a176c60e22683297121357336769a21ead9fadcbbd5" && \
    download.sh --url "${SYN_URL}" --sha256 "${SYN_SHA256}" "${DOWNLOAD_CACHE_DIRECTORY}" && \
    cp "${DOWNLOAD_CACHE_DIRECTORY}/${SYN_FILE}" /opt/tomcat/lib && \
    mkdir -p /opt/keys/jwt && \
    chown tomcat:tomcat /opt/keys/jwt && \
    cleanup.sh

COPY rootfs /

RUN mkdir /data && \
    chown tomcat:tomcat /data && \
    chown -R tomcat:tomcat /opt/tomcat
