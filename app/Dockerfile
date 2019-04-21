# Dockerfile
# Docker image app:dev

FROM ubuntu:18.04
LABEL maintainer="Sheroz Khaydarov http://sheroz.com"

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
apt-utils \
autoconf \
build-essential \
bison \
dpkg-dev \
libgdbm-dev \
ca-certificates \
dirmngr \
gnupg2 \
openssl \
ssh \
libssl1.0-dev \
libxml2-dev \
libxslt-dev \
libredline-dev \
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
&& apt-get clean 
## && rm -rf /var/lib/apt/lists/* \  

# Set locale
RUN locale-gen en_US.UTF-8

# Install Ruby
ARG RUBY_DOWNLOAD_URL 
ARG RUBY_DOWNLOAD_SHA256
RUN mkdir -p /tmp/build/ruby \
&& cd /tmp/build \
&& wget -q --show-progress --progress=bar:force:noscroll -O ruby.tar.xz "$RUBY_DOWNLOAD_URL" \
&& echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.xz" | sha256sum -c - \
&& tar -xJf ruby.tar.xz -C /tmp/build/ruby --strip-components=1 \
&& rm ruby.tar.xz \
&& cd /tmp/build/ruby \
&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
&& autoconf \
&& ./configure --build="$gnuArch" --enable-shared --disable-install-doc --disable-install-rdoc \
&& make \
&& make install \
&& rm -rf /tmp/build

ARG BUNDLE_PATH
ENV BUNDLE_PATH=$BUNDLE_PATH 
ENV GEM_HOME=$BUNDLE_PATH 
ENV	BUNDLE_APP_CONFIG=$BUNDLE_PATH 
ENV	BUNDLE_SILENCE_ROOT_WARNING=1 
ENV PATH $GEM_HOME/bin:$BUNDLE_PATH/gems/bin:$PATH
RUN mkdir -p $BUNDLE_PATH && chmod 777 $BUNDLE_PATH
# RUN mkdir ~/.gnupg && echo "disable-ipv6" >> ~/.gnupg/dirmngr.conf \
# && gpg2 --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3 \
# && curl -sSL https://get.rvm.io | bash -s \
# && /bin/bash -l -c ". /etc/profile.d/rvm.sh" \
# && /bin/bash -l -c "rvm install 2.5.3"

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
ENV	LANGUAGE en_US:en 
ENV LC_ALL en_US.UTF-8

ARG SOURCE_PATH
RUN mkdir -p $SOURCE_PATH
  
EXPOSE 3000
#ENTRYPOINT ["/bin/bash", "-l"]
#ENTRYPOINT ["/bin/bash", "-l", "-c"]
#ENTRYPOINT ["bundle", "exec"]
#CMD ["rails", "server", "-b", "0.0.0.0"]