FROM httpd:2.4

LABEL maintainer="thetoxin <the.toxin.rus@gmail.com>"

ARG OneC_PLATFORM_VERSION
ARG OneC_CUSTOM_VRD
ARG OneC_CONNSTR_SERVER
ARG OneC_CONNSTR_DB
ARG OneC_WSDIR

ENV OneC_PLATFORM_VERSION ${OneC_PLATFORM_VERSION:-8.3.15-1830}
ENV OneC_CUSTOM_VRD ${OneC_CUSTOM_VRD:-false}
ENV OneC_CONNSTR_SERVER ${OneC_CONNSTR_SERVER:-localhost}
ENV OneC_CONNSTR_DB ${OneC_CONNSTR_DB:-db}
ENV OneC_WSDIR ${OneC_WSDIR:-db}

COPY ./1c-enterprise83-*_${OneC_PLATFORM_VERSION}_amd64.deb /
COPY ./default.vrd /usr/local/apache2/htdocs/${OneC_WSDIR}/

RUN apt-get install -y /1c-enterprise83-common_${OneC_PLATFORM_VERSION}_amd64.deb \
    && apt-get install -y /1c-enterprise83-server_${OneC_PLATFORM_VERSION}_amd64.deb \
    && apt-get install -y /1c-enterprise83-ws_${OneC_PLATFORM_VERSION}_amd64.deb \ 
    && rm /1c-enterprise83-*_${OneC_PLATFORM_VERSION}_amd64.deb \
    && if [ "$OneC_CUSTOM_VRD" = "true" ]; then \
           /opt/1C/v8.3/x86_64/webinst -publish \
               -apache24 \
               -descriptor /usr/local/apache2/htdocs/${OneC_WSDIR}/default.vrd \
               -dir /usr/local/apache2/htdocs/${OneC_WSDIR} \
               -confPath /usr/local/apache2/conf/httpd.conf; \
       else \
           /opt/1C/v8.3/x86_64/webinst -publish \
               -apache24 \ 
               -wsdir ${OneC_WSDIR} \
               -dir /usr/local/apache2/htdocs/${OneC_WSDIR} \
               -connstr "Srvr=${OneC_CONNSTR_SERVER};Ref=${OneC_CONNSTR_DB};" \
               -confPath /usr/local/apache2/conf/httpd.conf; \
       fi \
    && chmod 755 /usr/local/apache2/htdocs/${OneC_WSDIR}/default.vrd

EXPOSE 80
 
CMD ["httpd-foreground"]
