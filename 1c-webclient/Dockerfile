FROM httpd:2.4

LABEL maintainer="thetoxin <the.toxin.rus@gmail.com>"

ARG PLATFORM_VERSION
ARG CUSTOM_VRD
ARG CONNSTR_SERVER
ARG CONNSTR_DB
ARG WSDIR

ENV PLATFORM_VERSION ${PLATFORM_VERSION:-8.3.15-1830}
ENV CUSTOM_VRD ${CUSTOM_VRD:-false}
ENV CONNSTR_SERVER ${CONNSTR_SERVER:-localhost}
ENV CONNSTR_DB ${CONNSTR_DB:-db}
ENV WSDIR ${WSDIR:-db}

COPY ./1c-enterprise83-*_${PLATFORM_VERSION}_amd64.deb /
COPY ./default.vrd /usr/local/apache2/htdocs/${WSDIR}/

RUN apt-get install -y /1c-enterprise83-common_${PLATFORM_VERSION}_amd64.deb \
    && apt-get install -y /1c-enterprise83-server_${PLATFORM_VERSION}_amd64.deb \
    && apt-get install -y /1c-enterprise83-ws_${PLATFORM_VERSION}_amd64.deb \
    && rm /1c-enterprise83-*_${PLATFORM_VERSION}_amd64.deb \
    && if [ "$CUSTOM_VRD" = "true" ]; then \
           /opt/1C/v8.3/x86_64/webinst -publish \
               -apache24 \
               -descriptor /usr/local/apache2/htdocs/${WSDIR}/default.vrd \
               -dir /usr/local/apache2/htdocs/${WSDIR} \
               -confPath /usr/local/apache2/conf/httpd.conf; \
       else \
           /opt/1C/v8.3/x86_64/webinst -publish \
               -apache24 \
               -wsdir ${WSDIR} \
               -dir /usr/local/apache2/htdocs/${WSDIR} \
               -connstr "Srvr=${CONNSTR_SERVER};Ref=${CONNSTR_DB};" \
               -confPath /usr/local/apache2/conf/httpd.conf; \
       fi \
    && chmod 755 /usr/local/apache2/htdocs/${WSDIR}/default.vrd

EXPOSE 80

CMD ["httpd-foreground"]
