# Strategies

## General
- username, passwordはどこかのサイト，ドキュメント，エラーメッセージ，質問サイトに載ってるものを流用している可能性がある
- デフォルトパスワードを検索して試してみる

## Web services
### Tomcat
- Webコンテナ，サーブレットコンテナ．簡易的にWebサーバの機能も兼ねる
- 参考URL
  - [Tomcatのアーキテクチャ概要と各コンポーネントの役割について - Rainbow Engine](https://rainbow-engine.com/architecture-of-tomcat/)
- マネージャにログインしてみる: /manager/html
  - Basic認証がある場合、hydra等でパスワードクラック
    - `hydra -C ~/tools/wordlists/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-post://$IP:8080/manager/html`
  - マネージャでは、サーバのOS情報の閲覧やWARファイルのデプロイが可能
  - WARファイルをアップロードできれば悪意のあるコードを実行できる
    - warファイルは展開可能で、アップロード後に対象パスに移動すれば任意コード実行が可能
    - https://github.com/SecurityRiskAdvisors/cmd.jsp
