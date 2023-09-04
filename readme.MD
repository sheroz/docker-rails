# Ruby on Rails Development on Windows using Docker (tested with Windows 10)

The brief description: [https://sheroz.com/pages/blog/ruby-rails-windows-docker.html](http://sheroz.com/pages/blog/ruby-rails-windows-docker.html)

## Docker engine version: 18.09.2 (Docker Desktop for Windows)

## 1. Set build parameters in the .env file
## 2. Build containers
\> docker-compose build
## run containers in the background and leave them running
\> docker-compose up -d
## 3. Install bower
\> docker-compose exec app bash

$ sudo npm install bower -g
## 4. Prepare directories
$ sudo mkdir -p $BUNDLE_PATH

$ sudo chown $HOST_USER:$HOST_USER $BUNDLE_PATH

$ chmod 700 $BUNDLE_PATH

$ sudo chmod 777 /opt/app

## 5. Initial copy of source files into app directory
$ cp -R /opt/src/. /opt/app
## 6. Install bundler
$ gem install bundler -v 1.17.0
## 7. Install bundles (try bundle update if install doesn't work)
$ bundle install
## 8. Build assets
$ rake bower:install['--allow-root']

## Restore databases through rake aws:restore_db or other manual database restore scripts   
\> docker-compose exec app bash

$ rake aws:restore_db

## Start rails 
\> docker-compose exec app bash

$ rails s -b 0.0.0.0
## Open host's browser at http://localhost:3000

## Starting RSpec tests
\> docker-compose exec app bash

$ bundle exec rspec

## Capistrano: Authentication & Authorisation configuratiom
https://capistranorb.com/documentation/getting-started/authentication-and-authorisation/
### Make sure that you have correct id_rsa settings in .env file
### Copy your id_rsa key file to the SSH_PRIVATE_KEY location
### Give to the id_rsa file required user ownership and permissions
### Exit and re-enter to the app container 
\> docker-compose exec app bash

$ sudo chown $HOST_USER:$HOST_USER $SSH_PRIVATE_KEY

$ chmod 600 $SSH_PRIVATE_KEY

$ exit

### Capistrano: deploy
\> docker-compose exec app bash

$ cd /opt/src

$ cap staging deploy
### Capistrano: test github access
$ ssh -vT git@github.com

## Source / App files synchronization
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

## Solution to fix the error messages below - restart Docker after each reboot :) 
ERROR: for app  Cannot start service app: driver failed programming external connectivity on endpoint (...)  Error starting userland proxy: (...)

# Quick reference of docker commands:
## run <command> inside the 'app' service
\> docker-compose run --rm app \<command\>

\> docker-compose run --rm app mc

\> docker-compose run --rm app irb

\> docker-compose run --rm app bundle install
## Open a bash console inside the 'app' container
\> docker run -it app bash

\> docker run -it app mc
 
\> docker run -it app irb 
## Show docker stats (analog of linux's top)
\> docker stats
## List containers
\> docker-compose ps
## List all containers
\> docker ps -a
## List services
\> docker-compose ps --services
## Delete all containers
\> docker rm $(docker ps -a -q)
## Show all images
\> docker image ls -a
## Delete all images
\> docker rmi -f $(docker images -q)
## Run services
\> docker-compose up -d
## Stop services
\> docker-compose stop
## Stop services and remove containers and networks
\> docker-compose down
## Stop services and remove ALL related containers, networks, images, and volumes 
\> docker-compose down --rmi all -v --remove-orphans
## List docker volumes
\> docker volume ls
## Remove all unused volumes
\> docker volume prune -f
## Clean up everything - remove all images, containers, and networks
\> docker system prune --volumes -af
## Inspect running container
\> docker inspect \<container ID\>
