FROM ruby:2.4.3-alpine3.7

ENV BUILD_PACKAGES="build-base bash" \
    DEV_PACKAGES="curl-dev ruby-dev zlib-dev libxml2-dev libxslt-dev tzdata yaml-dev curl mariadb-client" \
    RUBY_PACKAGES="ruby-json yaml nodejs yarn" \
    RAILS_ROOT="/myapp"

RUN apk update && apk add --no-cache \
    $BUILD_PACKAGES \
    $DEV_PACKAGES \
    $RUBY_PACKAGES \
    && apk add --no-cache --virtual gem_dep_packages \
    mariadb-dev \
    imagemagick6 \
    imagemagick6-dev \
    linux-headers \
    git \
    ### For Phantomjs
    && apk add --no-cache fontconfig \
    && mkdir -p /usr/share \
    && cd /usr/share \
    && curl -L https://github.com/Overbryd/docker-phantomjs-alpine/releases/download/2.11/phantomjs-alpine-x86_64.tar.bz2 | tar xj \
    && ln -s /usr/share/phantomjs/phantomjs /usr/bin/phantomjs \
    && rm -rf phantomjs-alpine-x86_64.tar.bz2 \
    ###
    ### TZ
    && cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    ###
    && mkdir -p $RAILS_ROOT

WORKDIR $RAILS_ROOT
COPY . $RAILS_ROOT
EXPOSE 3000
RUN bundle config build.nokogiri --use-system-libraries \
    && bundle install -j4 \
    && bundle clean \
