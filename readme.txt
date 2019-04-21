Docker Engine: 18.09.2

Quick reference:

> docker-compose build
> docker-compose up -d
> docker exec -it app bash
# gem install bundler -v 1.16.1
# bundle install


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
