# Nmap

- 80とは異なり、Tomcatが使用されている
```sh
PORT   STATE SERVICE REASON  VERSION
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0

8443/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-generator: Nicepage 4.5.4, nicepage.com
|_http-title: Home
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
```

---

# Whatweb

```zsh
┌──(koshi㉿kali)-[~/Exam/Happy]
└─$ whatweb -v -a3 --log-verbose WebEnum/whatweb_8443.txt http://$TargetIP:8443

WhatWeb report for http://192.168.104.111:8443
Status    : 200 OK
Title     : Home
IP        : 192.168.104.111
Country   : RESERVED, ZZ

Summary   : Email[exam@oscp.com,hi@designstudio.com], HTML5, HTTPServer[Microsoft-IIS/10.0], JQuery, MetaGenerator[Nicepage 4.5.4, nicepage.com], Microsoft-IIS[10.0], Open-Graph-Protocol[website], Script[application/ld+json,text/javascript]

Detected Plugins:
[ Email ]
        Extract email addresses. Find valid email address and 
        syntactically invalid email addresses from mailto: link 
        tags. We match syntactically invalid links containing 
        mailto: to catch anti-spam email addresses, eg. bob at 
        gmail.com. This uses the simplified email regular 
        expression from 
        http://www.regular-expressions.info/email.html for valid 
        email address matching. 

        String       : exam@oscp.com,hi@designstudio.com
        String       : hi@designstudio.com

[ HTML5 ]
        HTML version 5, detected by the doctype declaration 


[ HTTPServer ]
        HTTP server header string. This plugin also attempts to 
        identify the operating system from the server header. 

        String       : Microsoft-IIS/10.0 (from server string)

[ JQuery ]
        A fast, concise, JavaScript that simplifies how to traverse 
        HTML documents, handle events, perform animations, and add 
        AJAX. 

        Website     : http://jquery.com/

[ MetaGenerator ]
        This plugin identifies meta generator tags and extracts its 
        value. 

        String       : Nicepage 4.5.4, nicepage.com

[ Microsoft-IIS ]
        Microsoft Internet Information Services (IIS) for Windows 
        Server is a flexible, secure and easy-to-manage Web server 
        for hosting anything on the Web. From media streaming to 
        web application hosting, IIS's scalable and open 
        architecture is ready to handle the most demanding tasks. 

        Version      : 10.0
        Website     : http://www.iis.net/

[ Open-Graph-Protocol ]
        The Open Graph protocol enables you to integrate your Web 
        pages into the social graph. It is currently designed for 
        Web pages representing profiles of real-world things . 
        things like movies, sports teams, celebrities, and 
        restaurants. Including Open Graph tags on your Web page, 
        makes your page equivalent to a Facebook Page. 

        Version      : website

[ Script ]
        This plugin detects instances of script HTML elements and 
        returns the script language/type. 

        String       : application/ld+json,text/javascript

HTTP Headers:
        HTTP/1.1 200 OK
        Content-Type: text/html
        Last-Modified: Wed, 23 Feb 2022 15:02:12 GMT
        Accept-Ranges: bytes
        ETag: "b9eb3d56c628d81:0"
        Server: Microsoft-IIS/10.0
        Date: Sun, 22 Feb 2026 05:10:33 GMT
        Connection: close
        Content-Length: 49181

```

---

# Gobuster

errorが多ければ`-t 64`も試す

```zsh
┌──(koshi㉿kali)-[~/Exam/Happy]
└─$ gobuster dir -u http://$TargetIP:8443/ -r -k -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words.txt -t 100 -o WebEnum/gobuster_8443.txt -x 'html,htm,txt,sh,php,cgi,asp,aspx,jsp,pl,py,pdf'

```

---

# Nikto

```zsh

```

---

# Enumeration


---

# Screenshot


