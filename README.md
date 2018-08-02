# dcdocker4
Docker-based attempt at running Contenta. (work in progress)

Simple docker setup based off of nginx-simple, with mysql and php running in separate containers. PHP runs as PHP-FPM on port 9000, and has pdo_mysql installed for easy db connection management.

Has 4 services:

- nginx
- php
- mysqldb
- adminer

"adminer" is a simple phpmyadmin-like db admin interface, in this case available at port 8081.
nginx is available at port 8090.

## Volumes
Two volumes are created and bind-mounted to points in this dir:

- data (mounts the mysql data directory here, and persists the data)
- wwwroot (nginx docroot, persistent)

# Building the container

Clone this repository, and use the docker-compose.yml file provided here: `docker-compose build`

# Building Contenta

Clone contenta_jsonapi into a separate directory outside of the docker build directory you're working in:

`git clone https://github.com/contentacms/contenta_jsonapi`

cd into that directory, and then build using composer to the 'wwwroot' directory inside of your 'dcdocker4' directory:

`composer create-project contentacms/contenta-jsonapi-project [path_to_your_dcdocker4_dir]/dcdocker4/wwwroot --stability dev --no-interaction`

## Normal usage

  `docker-compose up -d`
  `docker-compose down`

##Drush

Drush gets installed in the "php" service, and can be invoked like this:

`docker-compose run php drush status`

