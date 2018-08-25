# dcdocker4
Docker-based attempt at running Contenta. (work in progress)

Simple docker setup based off of nginx-simple, with mysql and php running in separate containers. PHP runs as PHP-FPM on port 9000, and has pdo_mysql installed for easy db connection management.

Has 4 services:

- nginx
- php
- mysqldb
- adminer
- appserver

"adminer" is a simple phpmyadmin-like db admin interface, in this case available at port 8081.
nginx is available at port 8090. "appserver" is a second nginx container running on port 8091. The "ngroot" dir is bind-mounted in this container, and is meant as a location to deploy a web front end to consume the api endpoints exposed by Contenta.

## Volumes
Three volumes are created and bind-mounted to points in this dir:

- data (mounts the mysql data directory here, and persists the data)
- wwwroot (nginx docroot, persistent)
- ngroot (mounted to the appserver container, persistent)

# Building the container

Clone this repository, and use the docker-compose.yml file provided here: `docker-compose build`. Then bring up the containers with `docker-compose up`

# Create a database for Contenta

Use the adminer interface at localhost::8081 and create a new empty database called "contenta".

# Building Contenta

Remove the "web" directory inside of wwwroot (the contenta install will replace this folder, but won't install if the "web" dir already exists.)

Clone contenta_jsonapi into a separate directory outside of the docker build directory you're working in:

`git clone https://github.com/contentacms/contenta_jsonapi`

cd into that directory, and then build using composer to the 'wwwroot' directory inside of your 'dcdocker4' directory:

`composer create-project contentacms/contenta-jsonapi-project [path_to_your_dcdocker4_dir]/dcdocker4/wwwroot --stability dev --no-interaction`

If you get an out of memory error, increase php's memory limit: https://getcomposer.org/doc/articles/troubleshooting.md#memory-limit-errors

Once composer finishes, visit localhost:8090, and you should get the Drupal install screen. For the database options, choose the "contenta" database you created earlier, and open the "advanced" options and change "host" from "localhost" to "mysqldb", as this is the hostname where the database will be available in the docker-created network.

install errors: 
The specified file core/modules/media/images/icons/audio.png could not be moved/copied because the destination directory is not properly configured. This may be caused by a problem with file or directory permissions. More information is available in the system log.

# Building into ngroot

Create an angular app in another directory and use "ng build" to deploy to ngroot/web.

## Normal usage

  `docker-compose up -d`
  `docker-compose down`

## Drush

Drush gets installed in the "php" service, and can be invoked like this:

`docker-compose run php drush status`
