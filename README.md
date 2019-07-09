# Generic LAMP local dev for Docker

## Requrements

- Docker for Mac 2.0+

## Getting started

Make sure you've got macOS build in firewall service enabled.

MySQL credentials are in `conf/mysql/credentials.env`. Usually there's no need to change them.

To make sure you've got latest images:
- `docker-compose pull`

To start LAMP stack:
- `docker-compose up -d`

Then you can browse to http://127.0.0.1:8080/

You can use MySQL clients like mysqldump, mysql CLI and Sequel Pro on `127.0.0.1:3306`.

### Example WordPress project boostrapping

Place your project files under `project` directory.

For example to initialize WP Bedrock project:
1. `cd projects`
1. `composer create-project roots/bedrock myproject`
1. `cd web`
1. `ln -s ../myproject/web myproject`

Then open http://127.0.0.1:8080/myproject/

## Removing environment from Docker

To stop and remove containers:
- `docker-compose rm -s`

To stop and remove containers and MySQL data volume:
- `docker-compose rm -s -v`

