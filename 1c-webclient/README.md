# 1C Web Client

This Dockerfile builds 1C Web Client image.

## Build/Configure

* Get 1Cv8.3 packets (`1c-enterprise83-common_*_amd64.deb`, `1c-enterprise83-server_*_amd64.deb`, `1c-enterprise83-ws_*_amd64.deb`) for Debian
* Copy files to a directory with Dockerfile
* If you need image with custom `.vrd` replace `default.vrd` then run (8.3.15-1830 - 1C packets version):

    docker build --build-arg OneC_CUSTOM_VRD=true \
                 --build-arg OneC_PLATFORM_VERSION=8.3.15-1830 \
                 -t 1c-webclient:8.3.15-1830 \
                 -f Dockerfile .

* Or run:

    docker build --build-arg OneC_PLATFORM_VERSION=8.3.15-1830 \
                 --build-arg OneC_CONNSTR_SERVER=servername.local \
                 --build-arg OneC_CONNSTR_DB=dbname \
                 --build-arg OneC_WSDIR=db \
                 -t 1c-webclient:8.3.15-1830 \
                 -f Dockerfile .

## Run

   docker run -d \
              --name my_1c-webclient \
              -p 8080:80 \
              1c-webclient:8.3.15-1830

