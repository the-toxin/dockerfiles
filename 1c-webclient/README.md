# 1C Web Client

This Dockerfile builds 1C Web Client image.

---

## Build/Configure

* Get 1Cv8.3 packets (`1c-enterprise*-common_*_amd64.deb`, `1c-enterprise*-server_*_amd64.deb`, `1c-enterprise*-ws_*_amd64.deb`) for Debian
* Copy files to a directory with Dockerfile
* If you need image with custom `.vrd` replace `default.vrd` then run (8.3.15-1830 - 1C packets version):

```shell
    docker build --build-arg CUSTOM_VRD=true \
                 --build-arg PLATFORM_VERSION=8.3.15-1830 \
                 -t 1c-webclient:8.3.15-1830 \
                 -f Dockerfile .
```
* Or run:

```shell
    docker build --build-arg PLATFORM_VERSION=8.3.15-1830 \
                 --build-arg CONNSTR_SERVER=servername.local \
                 --build-arg CONNSTR_DB=dbname \
                 --build-arg WSDIR=db \
                 -t 1c-webclient:8.3.15-1830 \
                 -f Dockerfile .
```

## Run

```shell
   docker run -d \
              --name my_1c-webclient \
              --restart=always \
              -p 8080:80 \
              1c-webclient:8.3.15-1830
```

