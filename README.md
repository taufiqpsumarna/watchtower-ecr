# WatchTower ECR
A docker image based on [containrrr/watchtower](https://github.com/containrrr/watchtower/) for use with AWS ECR.
Project Repository [taufiqpsumarna/watchtower-ecr](https://github.com/taufiqpsumarna/watchtower-ecr) Forked from [conveos/watchtower-ecr](https://github.com/conveos/watchtower-ecr)

[![Docker Pulls](https://img.shields.io/docker/pulls/taufiq14s/watchtower-ecr.svg?style=flat-square)](https://hub.docker.com/r/taufiq14s/watchtower-ecr/)
[![Docker Stars](https://img.shields.io/docker/stars/taufiq14s/watchtower-ecr.svg?style=flat-square)](https://hub.docker.com/r/taufiq14s/watchtower-ecr/)

## Usage
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

If you prefer, you can use the [docker-compose.yml file](./docker-compose.yml) then update your .env file
```bash
cp .env.example .env
nano .env
docker-compose up -d
```

Both the single `docker run` and `docker-compose` can have the default config.json overridden to use a different authentication method for ECR. Refer to https://github.com/awslabs/amazon-ecr-credential-helper#aws-credentials for more info about it.
What you need to add is a new volume that map an existing config.json over the default one:

      - /path/to/docker-config.json:/config.json