# Cheat Sheet


## Portscan

### nmap
- 全ポートスキャン: `nmap -sS -A -T4 -Pn -p- $IP`

### rustscan
- installation: `sudo snap install -y rustscan`
- 全ポートスキャン: `rustscan -a $IP -- -Pn -A`
  - その後簡易的に脆弱性探し: `sudo nmap -sS -A -p[port] --script vuln $IP`

## Password Cracking

### hydra
- webサービスで総当たりする，
- Basic認証総当たり(コロン形式のコーパス): `hydra -C ~/tools/wordlists/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-post://$IP:8080/manager/html`
