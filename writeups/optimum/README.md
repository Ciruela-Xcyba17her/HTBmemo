# Recon
- 80 番ポートで `HttpFileServer 2.3` が動いている
  - CVE-2014-6287 (RCE) がある

# Exploitation
- `exploit/windows/http/rejetto_hfs_exec` が使える
  - `C:\Users\kostas\Desktop` に入れる

# Privesc
- sysinfo
  - result
    ```
    meterpreter > sysinfo
    Computer        : OPTIMUM
    OS              : Windows 2012 R2 (6.3 Build 9600).
    Architecture    : x64
    System Language : el_GR
    Domain          : HTB
    Logged On Users : 1
    Meterpreter     : x86/windows
    ```
- 安定する explorer.exe に migrate
- x64 system では local_exploit_suggester が不安定なため exploit/windows/local で privesc する方法を探す

  
