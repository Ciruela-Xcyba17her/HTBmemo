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
