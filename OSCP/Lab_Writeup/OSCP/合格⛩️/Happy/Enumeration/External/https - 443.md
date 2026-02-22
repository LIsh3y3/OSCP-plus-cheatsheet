# Nmap

- 80とは異なり、Tomcatが使用されている
```
PORT   STATE SERVICE REASON  VERSION
443/tcp  open  ssl/http      Apache Tomcat 8.5.34
|_ssl-date: TLS randomness does not represent time
| http-title: Site doesn't have a title (text/html;charset=utf-8).
|_Requested resource was /cbs/Logon.do
| ssl-cert: Subject: commonName=Not Secure/organizationName=Ahsay System Corporation Limited/stateOrProvinceName=Hong Kong SAR/countryName=CN
| Not valid before: 2017-03-21T20:52:17
|_Not valid after:  2020-03-20T20:52:17
```

---

# Whatweb

```zsh

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


