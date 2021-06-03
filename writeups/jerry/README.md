# Recon
- port 21 で anonymous login が許可されている ftp server が動いている
- port 80 で PRTG Network monitor が動いている
- ftp server anonymous login するとCドライブをルートとしてるのでいろんなファイルが見れる
  - user.txt を閲覧可能
- PRTG Network Monitor の configuration ファイルを確認
  - ```%programfiles%\PRTG Network Monitor```
  - ```%programfiles(x86)%\PRTG Network Monitor```
  - https://kb.paessler.com/en/topic/463-how-and-where-does-prtg-store-its-data
