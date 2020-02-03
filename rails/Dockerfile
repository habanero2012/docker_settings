FROM ruby:2.7.0

ENV LANG C.UTF-8

# install dependencies
RUN apt-get update -qq && apt-get install -y build-essential nodejs
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn

ENV APP_HOME /app_home

RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile Gemfile
ADD Gemfile.lock Gemfile.lock

RUN bundle install

ADD . $APP_HOME