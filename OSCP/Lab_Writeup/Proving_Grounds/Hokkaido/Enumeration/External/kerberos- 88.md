# Nmap

```zsh
PORT   STATE SERVICE REASON  VERSION
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-02-11 05:27:26Z)
```

---

# Enumeration

- kerbruteでユーザー列挙
	- ユーザー名の形式は、先頭大文字、すべて小文字、すべて大文字の３つのパターンがある様子
```sh
┌──(koshi㉿kali)-[~/ProvingGrounds/Hokkaido]
└─$ kerbrute userenum --dc hokkaido-aerospace.com -d hokkaido-aerospace.com /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt 

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 02/12/26 - Ronnie Flathers @ropnop

2026/02/12 09:52:17 >  Using KDC(s):
2026/02/12 09:52:17 >   hokkaido-aerospace.com:88

2026/02/12 09:52:18 >  [+] VALID USERNAME:       info@hokkaido-aerospace.com
2026/02/12 09:52:55 >  [+] VALID USERNAME:       administrator@hokkaido-aerospace.com
2026/02/12 09:53:12 >  [+] VALID USERNAME:       INFO@hokkaido-aerospace.com
2026/02/12 09:54:53 >  [+] VALID USERNAME:       Info@hokkaido-aerospace.com


```

---

# Screenshot


