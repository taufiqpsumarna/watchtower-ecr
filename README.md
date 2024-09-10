# WatchTower ECR
A docker image based on [containrrr/watchtower](https://github.com/containrrr/watchtower/) for use with AWS ECR.
Project Repository [taufiqpsumarna/watchtower-ecr](https://github.com/taufiqpsumarna/watchtower-ecr) Forked from [conveos/watchtower-ecr](https://github.com/conveos/watchtower-ecr)

[![Docker Pulls](https://img.shields.io/docker/pulls/taufiq14s/watchtower-ecr.svg?style=flat-square)](https://hub.docker.com/r/taufiq14s/watchtower-ecr/)
[![Docker Stars](https://img.shields.io/docker/stars/taufiq14s/watchtower-ecr.svg?style=flat-square)](https://hub.docker.com/r/taufiq14s/watchtower-ecr/)

## Usage

### Build Docker Image (optional)
Installation based on [Amazon ECR Credential Helper installation from source](https://github.com/awslabs/amazon-ecr-credential-helper?tab=readme-ov-file#from-source) and [containrrr/watchtower](https://github.com/containrrr/watchtower/) docker image

```yaml
FROM alpine:3.20.3 AS build

# https://github.com/awslabs/amazon-ecr-credential-helper#installing
RUN apk add --no-cache libc6-compat gcc g++ git go
RUN export GOPATH=$HOME/go && export PATH=$PATH:$GOPATH/bin
RUN go install github.com/awslabs/amazon-ecr-credential-helper/ecr-login/cli/docker-credential-ecr-login@latest

# Final image has to be alpine, scratch doesn't support our env vars or credentials file
FROM alpine:3.20.3
COPY --from=containrrr/watchtower:latest / /
COPY --from=build /root/go/bin/docker-credential-ecr-login /bin/docker-credential-ecr-login
COPY ./docker/config.json /config.json

ENTRYPOINT ["/watchtower"]
```
### Docker Run

Run the container with the following command:
```bash
docker run -d \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e "AWS_ACCESS_KEY_ID=<AWS_ACCESS_KEY_ID>" \
  -e "AWS_SECRET_ACCESS_KEY=<AWS_SECRET_ACCESS_KEY>" \
  -e "AWS_REGION=<AWS_REGION>"
  taufiq14s/watchtower-ecr:latest --interval 300 --cleanup
```

### Docker Compose
If you prefer, you can use the [docker-compose.yml file](./docker-compose.yml) then update your .env file

```bash
cp .env.example .env
nano .env
docker-compose up -d
```

Both the single `docker run` and `docker-compose` can have the default ```config.json``` overridden to use a different authentication method for ECR. 
Refer to https://github.com/awslabs/amazon-ecr-credential-helper#aws-credentials for more info about it.

What you need to add is a new volume that map an existing ```config.json``` over the default one in [docker-compose.yml file](./docker-compose.yml)
```yaml
...
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /path/to/docker-config.json:/config.json #overide default config.json
...
```