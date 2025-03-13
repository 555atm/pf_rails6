# ベースイメージの指定は不要（デフォルトはマルチプラットフォームなので。m1 macなのでベースイメージの指定
#FROM --platform=linux/amd64 ubuntu:22.04 
# FROM ubuntu:22.04 #ベースイメージの指定
#FROM amazonlinux:2


#amazonlinux:2 はマルチアーキ対応なので、以下のように書けば OK 
# # Amazon Linux 2 のマルチアーキ対応イメージを使用
# FROM amazonlinux:2

# # パッケージの更新
# RUN yum update -y && yum install -y \
#     git \
#     curl \
#     vim


# Install base packages
RUN apt-get update -qq && \
    apt-get install --no-install-recommends -y  \
    curl  \
    default-mysql-client  \
    libjemalloc2  \
    libvips && \
    rm -rf /var/lib/apt/lists /var/cache/apt/archives


# 必要なパッケージのインストール
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libssl-dev \
    libreadline-dev \
    zlib1g-dev \
    autoconf \
    bison \
    build-essential \
    libyaml-dev \
    libreadline-dev \
    libncurses5-dev \
    libffi-dev \
    libgdbm-dev



# rbenvのインストール
RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv
RUN echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
RUN echo 'eval "$(rbenv init -)"' >> ~/.bashrc

# ruby-buildのインストール
RUN git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

# Ruby 3.3.5のインストール
ENV PATH /root/.rbenv/bin:$PATH
RUN rbenv install 3.3.5
RUN rbenv global 3.3.5


# syntax = docker/dockerfile:1

# This Dockerfile is designed for production, not development. Use with Kamal or build'n'run by hand:
# docker build -t my-app .
# docker run -d -p 80:80 -p 443:443 --name my-app -e RAILS_MASTER_KEY=<value from config/master.key> my-app

# Make sure RUBY_VERSION matches the Ruby version in .ruby-version
ARG RUBY_VERSION=3.3.5
FROM docker.io/library/ruby:$RUBY_VERSION-slim AS base

# Rails app lives here
WORKDIR /app


# Set production environment
ENV RAILS_ENV="production" \
    BUNDLE_DEPLOYMENT="1" \
    BUNDLE_PATH="/usr/local/bundle" \
    BUNDLE_WITHOUT="development"

# Throw-away build stage to reduce size of final image
FROM base AS build

# Install packages needed to build gems
RUN apt-get update -qq && \
    apt-get install --no-install-recommends -y build-essential default-libmysqlclient-dev git pkg-config && \
    rm -rf /var/lib/apt/lists /var/cache/apt/archives




# Install application gems
COPY Gemfile Gemfile.lock ./
RUN bundle install && \
    rm -rf ~/.bundle/ "${BUNDLE_PATH}"/ruby/*/cache "${BUNDLE_PATH}"/ruby/*/bundler/gems/*/.git && \
    bundle exec bootsnap precompile --gemfile

# Copy application code
COPY . .

# Precompile bootsnap code for faster boot times
RUN bundle exec bootsnap precompile app/ lib/

# Precompiling assets for production without requiring secret RAILS_MASTER_KEY
RUN SECRET_KEY_BASE_DUMMY=1 ./bin/rails assets:precompile




# Final stage for app image
FROM base

# Copy built artifacts: gems, application
COPY --from=build "${BUNDLE_PATH}" "${BUNDLE_PATH}"
COPY --from=build /rails /rails

# Run and own only the runtime files as a non-root user for security
RUN groupadd --system --gid 1000 rails && \
    useradd rails --uid 1000 --gid 1000 --create-home --shell /bin/bash && \
    chown -R rails:rails db log storage tmp
USER 1000:1000

# Entrypoint prepares the database.
ENTRYPOINT ["/rails/bin/docker-entrypoint"]

# Start the server by default, this can be overwritten at runtime
EXPOSE 3000
CMD ["./bin/rails", "server"]
