## これは何

fluentd v0.12 -> v0.10に転送するときにmsgpackのパースエラーが発生したので調査

```
2017-06-30 22:51:36 +0900 fluent.error: {"error":"#<MessagePack::UnpackError: parse error.>","error_class":"MessagePack::UnpackError","message":"forward error error=#<MessagePack::UnpackError: parse error.> error_class=MessagePack::UnpackError"}
```

こんな感じにエラーが出る

原因はfluentd v0.10で利用しているmsgpackが0.4系で、fluentd v0.12で利用しているmsgpackが1系だとうまくいかない。
msgpack 0.5系 -> 0.4系なら問題なく動く

ただし、そのためにはmsgmacp0.5で動かすRubyは2.3系になってる必要がありそう（2.4だとgem installに失敗する） 

## ディレクトリ構成の説明

v0.10とv0.12のディレクトリに移動して以下のコマンドでfluentdを起動させる

```
$ bundle exec fluentd -c fluentd.conf
```


v0.12 -> v0.10に転送している設定なので、v0.12になにがしかのデータを送ると実験できる（今回の例だとbrew installしたnginxのログを使っている）
