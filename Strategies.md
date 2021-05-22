# Strategies

## General
- username, passwordはどこかのサイト，ドキュメント，エラーメッセージ，質問サイトに載ってるものを流用している可能性がある

## Web services
### Tomcat
- Webコンテナ，サーブレットコンテナ．簡易的にWebサーバの機能も兼ねる
- 参考URL
  - [Tomcatのアーキテクチャ概要と各コンポーネントの役割について - Rainbow Engine](https://rainbow-engine.com/architecture-of-tomcat/)
- マネージャにログインしてみる: /manager/html
  - Basic認証がある場合、hydra等でパスワードクラック
  - マネージャでは、サーバのOS情報の閲覧やWARファイルのデプロイが可能
  - WARファイルをアップロードできれば悪意のあるコードを実行できる
