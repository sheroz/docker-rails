# Dockerfile
# rails:ruby

FROM ubuntu:16.04

RUN apt-get update &&\
apt-get install -y --no-install-recommends \
apt-utils \
build-essential \
ca-certificates \
curl \
dirmngr \
gnupg2 \
openssl \
libpq-dev &&\
gpg2 --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3 &&\
curl -sSL https://get.rvm.io | bash -s &&\
/bin/bash -l -c ". /etc/profile.d/rvm.sh && rvm install 2.5.3" &&\
apt-get clean &&\
rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/bin/bash", "-l", "-c"]

