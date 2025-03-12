# pf_rails
For portfolio of rails

## 教材

- 【rails環境構築】docker + rails + mysql で環境構築（初心者でも30分で完了！）
- https://www.youtube.com/watch?v=Fq1PH0Gwi8I

- Ruby on Rails 6のDocker環境構築
- https://zenn.dev/yuma_ito_bd/scraps/1adb89dfe0661c



## version
- rails 6.1
- ruby 3.2.3
- mysql 8.0
- yarn
- webpacker
- node.js



### rails 5.2.2だとエラーになる
↓
エラーの内容を見ると、ArgumentError が発生しており、
active_support/messages/metadata.rb の wrap メソッドに2つの引数が渡されているが、期待されているのは1つの引数だけのようです。
この問題は、Ruby 3.0以降で導入されたキーワード引数の変更に関連している可能性があります。
Ruby 3.0では、位置引数とキーワード引数が分離されたため、メソッド呼び出しにおいて引数の扱いが厳格になりました
このエラーは、使用しているRailsのバージョンが古く、Ruby 3.0以降のキーワード引数の変更に対応していないことが原因である可能性が高いです。Rails 5.2.2はRuby 3.0のキーワード引数の変更に対応していないため、このようなエラーが発生することがあります。
　｜
解決策としては、以下のいずれかを試してみてください:
- Rubyのバージョンを下げる: Rubyのバージョンを2.7以前に下げることで、キーワード引数の変更前の挙動に戻すことができます。
- Railsのバージョンを上げる: Railsの新しいバージョン（例えばRails 6以上）にアップグレードすることで、Ruby 3.0以降のキーワード引数の変更に対応したコードに更新されます。


### RailsとRubyのバージョン対応表
https://www.fastruby.io/blog/ruby/rails/versions/compatibility-table.html




↓
docker-compose run web rails new . --force --database=mysql
を実行すると、
いろいろ試したけど
Yarnがインストールできない。以下エラーが解消できない
```
Yarn not installed. Please download and install Yarn from https://yarnpkg.com/lang/en/docs/install/
Exiting!
```










