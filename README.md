# WatchTower ECR
A docker image based on [containrrr/watchtower](https://github.com/containrrr/watchtower/) for use with AWS ECR.

[![Docker Pulls](https://img.shields.io/docker/pulls/ottolini/watchtower-ecr.svg?style=flat-square)](https://hub.docker.com/r/ottolini/watchtower-ecr/)
[![Docker Stars](https://img.shields.io/docker/stars/ottolini/watchtower-ecr.svg?style=flat-square)](https://hub.docker.com/r/ottolini/watchtower-ecr/)

## Usage
Run the container with the following command:
```bash
docker run -d \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e "AWS_ACCESS_KEY_ID=<ACCESS_KEY_ID>" \
  -e "AWS_SECRET_ACCESS_KEY=<SECRET_ACCESS_KEY>" \
  ottolini/watchtower-ecr:latest --interval 300 --cleanup
```

If you prefer, you can use the docker-compose.yml
```bash
version: '3.6'
services:
  my-service:
    image: <id>.dkr.ecr.<region>.amazonaws.com/my-image:latest
  watchtower:
    image: ottolini/watchtower-ecr:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /path/to/docker-config.json:/config.json
    environment:
      AWS_ACCESS_KEY_ID: <ACCESS_KEY_ID>
      AWS_SECRET_ACCESS_KEY: <SECRET_ACCESS_KEY>
    command: --interval 300 --cleanup
```