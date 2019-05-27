# Docker environment for Ruby on Rails Development (tested on Windows 10)

The full description may or may not come soon... :) 

I am using this environment for Ruby on Rails development on Windows.

I use Ubunutu 18.0.4 (bionic) as a base image for the **app** (Ruby on Rails) container to make sure that it would work the same way in development as in production. To achieve better performance in synchronizing source files on the host with the in-container directory (I had to duplicate sources inside of the container, because directly accessing mounted host files is very slow and, unfortunately, the volume 'cached' option does not work yet for Windows based hosts), I use File Watchers plugin on RubyMine (finally, after having tried many other different options like unison, lsyncd, ...).

About network settings for the database and redis services: The app service needed access to the databases and redis on the same network's localhost (127.0.0.1) and this required is achieved by using **network_mode: "service:[service name]"** option.

The **app** container runs in user mode.

I normally start rails in bash console and that allows to control messages and debug using **binding.pry**.
When needed, I am running another separate bash console to start **sidekiq** service.
The app image has bunch of tools, including the **mc** ([The Midnight Commander](http://midnight-commander.org), my favorite file manager).
The **sudo** command is availabe without password prompt.
I use **capistrano** inside **app** container as a deployment automation tool.

The development and overall rails performance is acceptable. I can say that the difference not a big, compared to the development on  MacBook Pro 15 2018 what I normally use for Rails development at the office. The hardware components of the hosts (CPU, SSD, RAM) are approximately the same. 

## Docker engine version: 18.09.2 (Docker Desktop for Windows)

## 1. Set build parameters in the .env file
## 2. Build containers
\> docker-compose build
## run containers in the background and leave them running
\> docker-compose up -d
## 3. Install bower
\> docker exec -it app bash

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
## 9. Restore your databases, run rake aws:restore_db or other manual database restore scripts   
## 10. Start rails 
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
