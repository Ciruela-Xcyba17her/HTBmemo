# Enumeration
- port 80 で `Microsoft IIS httpd 6.0` が動いている
  - ぐぐると CVE-2017-7269 に対し脆弱
  - https://www.exploit-db.com/exploits/41738

# Exploit
- https://github.com/g0rx/iis6-exploit-2017-CVE-2017-7269 からスクリプトをもらう
  - 適切な引数を指定し実行 

# Privesc
- いろいろファイルを置きたいので書き込み可能ディレクトリを探す
  - `C:\wmpub` は見かけないディレクトリであり書き込み可能
- Windows Exploit Suggester をする
