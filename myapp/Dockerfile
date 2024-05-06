FROM ruby:3.2.3

#nodjsなどインストール
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev

## 「-qqオプション」はエラー以外何も吐かないことを意味する。
## 「-y」オプションは全部yesで実行することを意味する。

# install nodejs(LTS)
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && apt-get install -y nodejs

# install yarn
RUN npm install --global yarn


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