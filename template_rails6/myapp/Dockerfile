FROM ruby:3.2.3

#nodjsなどインストール
# RUN yum update -qq && yum install -y build-essential libpq-dev

## 「-qqオプション」はエラー以外何も吐かないことを意味する。
## 「-y」オプションは全部yesで実行することを意味する。

# install nodejs(LTS)　※もちろんlts.xじゃだめ
# RUN curl -fsSL https://deb.nodesource.com/setup_lts | bash - && apt-get install -y nodejs
# または　https://deb.nodesource.com/
# RUN curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash -
# RUN sudo apt-get install -y nodejs
# RUN curl -sL https://rpm.nodesource.com/setup_20.x | sudo bash -
# RUN sudo yum install -y nodejs

# install yarn
# RUN sudo npm install --global yarn

# 必要なパッケージのインストール（基本的に必要になってくるものだと思うので削らないこと）
RUN apt-get update -qq && \
    apt-get install -y build-essential \ 
                       libpq-dev 

# yarnパッケージ管理ツールをインストール
RUN apt-get update && apt-get install -y curl apt-transport-https wget && \
  curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
  echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
  apt-get update && apt-get install -y yarn
# Node.jsをインストール
RUN curl -sL https://deb.nodesource.com/setup_7.x | bash - && \
  apt-get install nodejs 



# # Add a script to be executed every time the container starts.
# COPY entrypoint.sh /usr/bin/
# RUN chmod +x /usr/bin/entrypoint.sh
# ENTRYPOINT ["entrypoint.sh"]
# EXPOSE 3000

RUN mkdir /myapp
WORKDIR /myapp

# COPY Gemfile* /myapp/
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock




RUN bundle install
COPY . /myapp

# Railsサーバーを起動
CMD ["rails", "server", "-b", "0.0.0.0"]