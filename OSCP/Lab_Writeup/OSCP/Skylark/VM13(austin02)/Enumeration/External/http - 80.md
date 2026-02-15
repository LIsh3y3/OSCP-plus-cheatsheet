# Nmap

```
PORT   STATE SERVICE REASON  VERSION
53/tcp open  domain? syn-ack
|_dns-nsec3-enum: Can't determine domain for host 10.10.11.174; use dns-nsec3-enum.domains script arg.
|_dns-nsec-enum: Can't determine domain for host 10.10.11.174; use dns-nsec-enum.domains script arg.

Host script results:
|_dns-brute: Can't guess domain of "10.10.11.174"; use dns-brute.domain script argument.
```

---

# Whatweb

```zsh
┌──(koshi㉿kali)-[~/PEN-200/Skylark/VM13]
└─$ whatweb -v -a3 --log-verbose WebEnum/whatweb_80.txt http://$TargetIP                  
WhatWeb report for http://192.168.222.221
Status    : 200 OK
Title     : IIS Windows Server
IP        : 192.168.222.221
Country   : RESERVED, ZZ

Summary   : HTTPServer[Microsoft-IIS/10.0], Microsoft-IIS[10.0], X-Powered-By[ASP.NET]

Detected Plugins:
[ HTTPServer ]
        HTTP server header string. This plugin also attempts to 
        identify the operating system from the server header. 

        String       : Microsoft-IIS/10.0 (from server string)

[ Microsoft-IIS ]
        Microsoft Internet Information Services (IIS) for Windows 
        Server is a flexible, secure and easy-to-manage Web server 
        for hosting anything on the Web. From media streaming to 
        web application hosting, IIS's scalable and open 
        architecture is ready to handle the most demanding tasks. 

        Version      : 10.0
        Website     : http://www.iis.net/

[ X-Powered-By ]
        X-Powered-By HTTP header 

        String       : ASP.NET (from x-powered-by string)

HTTP Headers:
        HTTP/1.1 200 OK
        Content-Type: text/html
        Last-Modified: Wed, 16 Nov 2022 11:43:42 GMT
        Accept-Ranges: bytes
        ETag: "73d4fadb0f9d81:0"
        Server: Microsoft-IIS/10.0
        X-Powered-By: ASP.NET
        Date: Sun, 15 Feb 2026 00:30:36 GMT
        Connection: close
        Content-Length: 703

```

---

# Gobuster

errorが多ければ`-t 64`も試す

```zsh

```

---

# Nikto

```zsh

```

---

# Enumeration


---

# Screenshot


