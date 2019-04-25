# Docker environment for Ruby on Rails Development ( tested on Windows 10 )

The full description would be soon...

For short: I am using this environment for Ruby on Rails development on Windows. To achieve better performance, I am using File Watchers plugin on RubyMine to synchronize host based source files with the in-container directory. The development performance and overall rails server performance is acceptable (really good, not a big difference compared to the MacBook Pro I normally used for Rails development at office). 

## Docker engine version: 18.09.2 (Docker Desktop for Windows)

## 1. Set build parameters in the .env file
## 2. Build containers
> docker-compose build
## run containers in the background and leave them running
> docker-compose up -d
## 3. Open a bash console in the running app container
> docker exec -it app bash
## 4. Initial copy of source files into app directory
$ cp -R /opt/src/. /opt/app
## 5. Install bundler
$ gem install bundler -v 1.16.1
## 6. Install bundles
$ bundle install
## 7. Install bower
$ npm install bower -g
## 8. Build assets
$ rake bower:install['--allow-root']
## 9. Start rails 
$ rails s -b 0.0.0.0
## 10. Open browser at http://localhost:3000

## Source <-> App files synchronization
## File Watchers configuration for JetBrains IDEs:
**File type:** Any

**Scope:** Project files

**Program:** C:\Program Files\Docker\Docker\resources\bin\docker

**Arguments:** cp $FilePath$ app:/opt/app/$FilePathRelativeToProjectRoot$

**Working directory:** $ProjectFileDir$


## Docker Engine configuration
In the General section of your Docker settings, turn on the Expose daemon on tcp://localhost:2375 without TLS option. 

## RubyMin IDE configuration
https://www.jetbrains.com/help/ruby/settings-docker-tools.html

# Quick reference of docker commands:
## run <command> inside the 'app' service container
> docker-compose run --rm app <command>
---
> docker-compose run --rm app mc

> docker-compose run --rm app irb

> docker-compose run --rm app bundle install
---
## run container app and open a bash console
--- 
> docker run -it app mc
 
> docker run -it app bash
 
> docker run -it app irb 
---
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
