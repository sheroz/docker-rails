# Dockerfile
# app

FROM ubuntu:16.04

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
apt-utils \
build-essential \
ca-certificates \
dirmngr \
gnupg2 \
openssl \
ssh \
libxml2-dev \
libxslt-dev \
libpq-dev \
tzdata \
locales \
curl \
wget \
zip \
unzip \
mc \
git \
nodejs \
&& apt-get clean \
## && rm -rf /var/lib/apt/lists/* \  
&& locale-gen en_US.UTF-8 \
&& gpg2 --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3 \
&& curl -sSL https://get.rvm.io | bash -s \
&& /bin/bash -l -c ". /etc/profile.d/rvm.sh" \
&& /bin/bash -l -c "rvm install 2.5.3"

# Install PostgreSQL client tools
RUN touch /etc/apt/sources.list.d/pgdg.list \
&& echo "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main" >> /etc/apt/sources.list.d/pgdg.list \
&& wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
&& apt-get update \
&& apt-get -y install postgresql-client-10 \
&& apt-get clean

# Install MongoDB client tools
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5 \
&& echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.6.list \
&& apt-get update \
&& apt-get -y install mongodb-org-tools mongodb-org-shell \
&& apt-get clean

ENV LANG en_US.UTF-8 
ENV LANGUAGE en_US:en 
ENV LC_ALL en_US.UTF-8

RUN mkdir -p /app 
WORKDIR /app

EXPOSE 3000

ENTRYPOINT ["/bin/bash", "-l", "-c"]

#ENTRYPOINT ["bundle", "exec"]
#CMD ["rails", "server", "-b", "0.0.0.0"]