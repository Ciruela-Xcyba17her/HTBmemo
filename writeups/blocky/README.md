# Enumeration
- 80 番で Apache httpd 2.4.18 が動いている
- web サイトを見てみると、どうやら wiki system や plugin の開発を行っているようだ
- web サイトの内容から、おそらく `notch` ユーザが存在する
- `~/tools/ffuf -u http://$IP/FUZZ -w /usr/share/dirb/wordlists/common.txt`
  - `/plugins` がある
- `/plugins` を見る
  - JAR ファイルがある
  - `BlockyCore.jar` をデコンパイルして調べるとクレデンシャル情報がある
    ```
    package com.myfirstplugin;

    public class BlockyCore {
      public String sqlHost = "localhost";

      public String sqlUser = "root";

      public String sqlPass = "8YsqfCTnvxAUeduzjNSXe22";

      public void onServerStart() {}

      public void onServerStop() {}

      public void onPlayerJoin() {
        sendMessage("TODO get username", "Welcome to the BlockyCraft!!!!!!!");
      }

      public void sendMessage(String username, String message) {}
    }
    ```
  - `griefprevention-1.11.2-3.1.1.298.jar` はハッシュ値を調べるとマイクラMOD

# Exploit & Privesc
- 見つかった user/pass はphpMyAdmin用と推測されるが、いろんなサービスに流用しているのではないかと考える
  - notch ユーザとして ssh ログイン可能
  - `sudo -l` ができ、その結果 notch はすべてのコマンドをあらゆるユーザ権限で実行できる
  - user.txt および root.txt を閲覧可能
