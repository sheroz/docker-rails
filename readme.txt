Docker Engine: 18.09.2

Quick reference:

## build containers
> docker-compose build
## run containers in the background and leave them running
> docker-compose up -d
## open a bash in a running app container
> docker exec -it app bash
## copy source files into app directory
$ cp -R /opt/src/. /opt/app
## install bundler
$ gem install bundler -v 1.16.1
## install bundles
$ bundle install
## install bower
$ npm install bower -g
## build assets
$ rake bower:install['--allow-root']
## start rails 
$ rails s -b 0.0.0.0
# open browser at http://localhost:3000

## ---
# source <-> app files synchronization
# File Watchers configuration for JetBrains IDEs:
# File type: Any
# Scope: Project files
# Program: C:\Program Files\Docker\Docker\resources\bin\docker
# Arguments: cp $FilePath$ app:/opt/app/$FilePathRelativeToProjectRoot$
# Working directory: $ProjectFileDir$
## ---

## run <command> inside the 'app' service container
> docker-compose run --rm app <command>
## samples
> docker-compose run --rm app mc
> docker-compose run --rm app irb
> docker-compose run --rm app bundle install

## run container app and open a bash console 
## samples
> docker run -it app mc
> docker run -it app bash
> docker run -it app irb

## show docker stats (analog of linux's top)
> docker stats

## List containers
> docker-compose ps
## List all containers
> docker ps -a
## List services
> docker-compose ps --services
## Delete all containers
> docker rm $(docker ps -a -q)

## show all images
> docker image ls -a
## Delete all images
> docker rmi -f $(docker images -q)

## Run services
> docker-compose up -d
## Stop services
> docker-compose stop
## Stop services and remove containers and networks
> docker-compose down
## Stop services and remove ALL related containers, networks, images, and volumes 
> docker-compose down --rmi all -v --remove-orphans

## list docker volumes
> docker volume ls
## remove all unused volumes
> docker volume prune -f

## clean up everything - remove all images, containers, and networks
> docker system prune --volumes -af

## Inspect running container
> docker inspect <container ID>


***
https://docs.docker.com/engine/reference/run/#network-container
docker run -d --name postgres1 athena_postgres:latest
docker run --rm -it --network container:postgres1 app:dev

 docker run -d --network=host athena_postgres:latest
 docker run --rm -it --network=host app:dev
***
references:
https://www.jetbrains.com/help/ruby/using-docker-as-a-remote-interpreter.html

https://revs.runtime-revolution.com/setting-up-a-simple-rails-development-environment-with-docker-for-fun-and-profit-71b8aa0d33c1
https://tomazy.com/blog/2017/05/rails-development-with-docker/
https://github.com/cweagans/docker-bg-sync
https://blog.codeship.com/running-rails-development-environment-docker/
https://nickjanetakis.com/blog/dockerize-a-rails-5-postgres-redis-sidekiq-action-cable-app-with-docker-compose
https://www.packet.com/blog/how-to-run-your-rails-app-on-docker/

https://nftb.saturdaymp.com/today-i-learned-how-to-create-rails-docker-container-without-ruby-installed/
http://bradgessler.com/articles/docker-bundler/
https://medium.com/magnetis-backstage/how-to-cache-bundle-install-with-docker-7bed453a5800
