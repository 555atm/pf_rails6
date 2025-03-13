#### [1] ##########################################
# 1. ベースイメージの指定は不要（デフォルトはマルチプラットフォームなので。m1 macなのでベースイメージの指定
#FROM --platform=linux/amd64 ubuntu:22.04 
# FROM ubuntu:22.04 #ベースイメージの指定
#FROM amazonlinux:2


#amazonlinux:2 はマルチアーキ対応なので、以下のように書けば OK 
# # Amazon Linux 2 のマルチアーキ対応イメージを使用
# FROM amazonlinux:2

#### [2] ##########################################
# FROM：使用するイメージ、rubyバージョン
FROM ruby:3.1
# 公式→https://hub.docker.com/_/ruby



#### [3] ##########################################
# 追加でインストールしたいものをインストール
# Rails 7ではWebpackerが標準では組み込まれなくなったので、yarnやnodejsのインストールが不要！！！！！！！！！！！！！！！！！！！！！！！

# パッケージの更新(yum)
RUN yum update -y && yum install -y \
    git \
    curl \
    default-mysql-client \
    vim


# Install base packages(apt)
# RUN apt-get update -qq && \
#     apt-get install --no-install-recommends -y  \
#     git  \
#     curl  \
#     default-mysql-client  \
#     vim


########### コンテナにrbenvのインストールは不要 ############
# # rbenvのインストール
# RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv
# RUN echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
# RUN echo 'eval "$(rbenv init -)"' >> ~/.bashrc

# # ruby-buildのインストール
# RUN git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

# # Ruby 3.3.5のインストール
# ENV PATH /root/.rbenv/bin:$PATH
# RUN rbenv install 3.3.5
# RUN rbenv global 3.3.5

# Rails app lives here
WORKDIR /app
COPY ./Gemfile Gemfile
COPY ./Gemfile.lock Gemfile.lock
RUN bundle install
# COPY . /app
COPY .entrypoint.sh /usr/bin/entrypoint.sh
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0"]



