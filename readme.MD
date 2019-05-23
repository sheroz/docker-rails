# Docker environment for Ruby on Rails Development ( tested on Windows 10 )

The full description may or may not come soon... :) 

I am using this environment for Ruby on Rails development on Windows.

I use Ubunutu 18.0.4 (bionic) as a base image for the app (Ruby on Rails) container to make sure that it would work the same way in development as in production. To achieve better performance in synchronizing source files on the host with the in-container directory (I had to duplicate sources inside of the container, because directly accessing mounted host files is very slow and, unfortunately, the volume 'cached' option does not work yet for Windows based hosts), I use File Watchers plugin on RubyMine (finally, after having tried many other different options like unison, lsyncd, ...).

About network settings for the database and redis services: The app service needed access to the databases and redis on the same network's localhost (127.0.0.1) and this required is achieved by using **network_mode: "service:[service name]"** option.

I normally start rails in bash console and that allows to control messages and debug using **binding.pry**.
When needed, I use another separate bash console to start **sidekiq**.
Also, the app image has bunch of tools, including the **mc** (Midnight Commander)

The development and overall rails performance is acceptable. I can say that the difference not a big, compared to the development on  MacBook Pro 15 2018 what I normally use for Rails development at the office. The hardware components of the hosts (CPU, SSD, RAM) are approximately the same. 

## Docker engine version: 18.09.2 (Docker Desktop for Windows)

## 1. Set build parameters in the .env file
## 2. Build containers
> docker-compose build
## run containers in the background and leave them running
> docker-compose up -d
## 3. Install bower (needs a root access)
> docker exec -u 0 app bash
$ npm install bower -g
$ exit
## 4. Open a bash console in the running app container (as a default user)
> docker exec -it app bash
## 5. Prepare directories
$ mkdir -p $BUNDLE_PATH
$ chmod 700 $BUNDLE_PATH
$ mkdir -p /opt/src
## 6. Initial copy of source files into app directory
$ cp -R /opt/src/. /opt/app
## 7. Install bundler
$ gem install bundler -v 1.17.0
## 8. Install bundles (try bundle update if install doesn't work)
$ bundle install
## 9. Build assets
$ rake bower:install['--allow-root']
## 10. Start rails 
$ rails s -b 0.0.0.0
## 11. Open browser at http://localhost:3000

## Opening a bash console in the running app container as a root
> docker-compose exec -u 0 app bash

## Starting RSpec tests
$ bundle exec rspec

## Capistrano Authentication & Authorisation
https://capistranorb.com/documentation/getting-started/authentication-and-authorisation/

## Make sure that you have id_rsa setting correct in .env 
## id_rsa file should have correct permissions
$ chown sheroz:sheroz /opt/storage/keys/id_rsa
$ chmod 600 /opt/storage/keys/id_rsa

## Capistrano deploy as authorized user
> docker-compose exec -u 0 app bash
$ su sheroz
$ cd /opt/src
$ cap staging deploy

## Test github authentication (needed for Capistrano) 
$ ssh -vT git@github.com

## Adding sudo user
$ apt-get -y install sudo
$ adduser sheroz
$ usermod -aG sudo sheroz
$ echo "sheroz  ALL=(ALL:ALL) ALL" >> /etc/sudoers


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

## Solution to fix the error messages below is - restart Docker after each reboot :) 
Cannot start service app: driver failed programming external connectivity on endpoint (...)  Error starting userland proxy: (...)

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
