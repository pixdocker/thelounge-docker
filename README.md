# thelounge-docker

A Docker image for [thelounge](https://thelounge.chat) mostly for personal use but feel free to use it.

# Why a Separate image?
- Learning Docker
- default image for thelounge had hardcoded version, this gets latest one
- `docker-entrypoint.sh`  issue `runc create failed: unable to start container process: exec: "docker-entrypoint.sh": executable file not found in $PATH: unknown`

# Usage
Download the `docker-compose` file and modify volume and port according to need and then run `sudo docker-compose up -d`

or use docker cli
```
docker run -d \
    --name thelounge \
    --publish 9000:9000 \
    --volume ~/apps/thelounge:/var/opt/thelounge \
    --restart unless-stopped \
    quay.io/pixelpizza/thelounge:latest
```
