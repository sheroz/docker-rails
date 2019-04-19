
docker-compose build
# open a bash console 
docker run -it ruby bash
# open an IRB - interactive Ruby
docker run -it ruby irb

# show all containers
docker ps -a
# Delete all containers
docker rm $(docker ps -a -q)
# Delete all images
docker rmi -f $(docker images -q)

# start containers 
docker-compose up -d stop
# stop containers 
docker-compose stop
# remove containers 
docker-compose down

# list docker volumes
docker volume ls
# remove all unused volumes
docker volume prune




references:
https://revs.runtime-revolution.com/setting-up-a-simple-rails-development-environment-with-docker-for-fun-and-profit-71b8aa0d33c1
https://tomazy.com/blog/2017/05/rails-development-with-docker/
https://github.com/cweagans/docker-bg-sync
https://blog.codeship.com/running-rails-development-environment-docker/
https://nickjanetakis.com/blog/dockerize-a-rails-5-postgres-redis-sidekiq-action-cable-app-with-docker-compose
https://www.packet.com/blog/how-to-run-your-rails-app-on-docker/

https://nftb.saturdaymp.com/today-i-learned-how-to-create-rails-docker-container-without-ruby-installed/