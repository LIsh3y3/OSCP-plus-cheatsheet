# Host Information

```zsh

```

---

# Scans

## Nmap

### quick scan

```
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp   open  ssl/http      Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=austin02.SKYLARK.com
| Not valid before: 2022-11-15T12:30:26
|_Not valid after:  2023-05-17T12:30:26
|_http-server-header: Microsoft-IIS/10.0
| tls-alpn: 
|_  http/1.1
445/tcp   open  microsoft-ds  Windows Server 2022 Standard 20348 microsoft-ds
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
10000/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2026-02-14T12:18:10+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=austin02.SKYLARK.com
| Not valid before: 2025-10-10T01:42:09
|_Not valid after:  2026-04-11T01:42:09
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
```
### full scan

```
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp   open  ssl/http      Microsoft IIS httpd 10.0
| tls-alpn: 
|_  http/1.1
|_http-title: IIS Windows Server
| ssl-cert: Subject: commonName=austin02.SKYLARK.com
| Not valid before: 2022-11-15T12:30:26
|_Not valid after:  2023-05-17T12:30:26
|_http-server-header: Microsoft-IIS/10.0
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|_  Potentially risky methods: TRACE
445/tcp   open  microsoft-ds  Windows Server 2022 Standard 20348 microsoft-ds
3387/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
5504/tcp  open  msrpc         Microsoft Windows RPC
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
10000/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=austin02.SKYLARK.com
| Not valid before: 2025-10-10T01:42:09
|_Not valid after:  2026-04-11T01:42:09
| rdp-ntlm-info: 
|   Target_Name: SKYLARK
|   NetBIOS_Domain_Name: SKYLARK
|   NetBIOS_Computer_Name: AUSTIN02
|   DNS_Domain_Name: SKYLARK.com
|   DNS_Computer_Name: austin02.SKYLARK.com
|   DNS_Tree_Name: SKYLARK.com
|   Product_Version: 10.0.20348
|_  System_Time: 2026-02-14T12:28:19+00:00
|_ssl-date: 2026-02-14T12:28:55+00:00; 0s from scanner time.
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
...
```

### udp scan

```
PORT      STATE         SERVICE       VERSION
80/tcp    open          http          Microsoft IIS httpd 10.0
135/tcp   open          msrpc         Microsoft Windows RPC
139/tcp   open          netbios-ssn   Microsoft Windows netbios-ssn
443/tcp   open          ssl/http      Microsoft IIS httpd 10.0
445/tcp   open          microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
10000/tcp open          ms-wbt-server Microsoft Terminal Services
9/udp     open|filtered discard
...
```

## Rustscan

```
PORT      STATE SERVICE       REASON          VERSION
80/tcp    open  http          syn-ack ttl 125 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
443/tcp   open  ssl/http      syn-ack ttl 125 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
| ssl-cert: Subject: commonName=austin02.SKYLARK.com
| Issuer: commonName=austin02.SKYLARK.com
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-11-15T12:30:26
| Not valid after:  2023-05-17T12:30:26
| MD5:   01d0:9b4e:fc5d:353b:4411:4f3d:897b:e792
| SHA-1: 25c3:6186:4243:5f86:16b0:d0c7:190f:0105:d4fd:996b
| -----BEGIN CERTIFICATE-----
| MIIC7DCCAdSgAwIBAgIQFYVtMzWCnKtKKultNhzkYDANBgkqhkiG9w0BAQsFADAf
| MR0wGwYDVQQDExRhdXN0aW4wMi5TS1lMQVJLLmNvbTAeFw0yMjExMTUxMjMwMjZa
| Fw0yMzA1MTcxMjMwMjZaMB8xHTAbBgNVBAMTFGF1c3RpbjAyLlNLWUxBUksuY29t
| MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2knwvxJ+0NjWKcUJ/BxS
| WcAcU5uy2KRceIGUi1O+wJQxIBB9cKqk4oz4lTixi6gdH1GeKfT8poDl8elVN8Sr
| 9ZtzMLCRakMAlVfXa6K1+2lIeKWmI2At2qP00yR6KVzq55LRrv1cvzeDtRv3a8ez
| Zll40W2aGf/OdOjRChvadA8sNevv5g6DORTS6id8rNdkKQAodwaS2RPA/5MSv2xs
| iz5fLnviOXpJiDCVxkzlZoczViGJu2movUGedJik+P/yuoeQ4ffJ2VkYE1Egoc8B
| JK44N7AGFE1G6Xt0JGYsqSGiFVnzeUx9Twoosd/O4WFClM357eFfSuVkB6HHJMcy
| KQIDAQABoyQwIjATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJ
| KoZIhvcNAQELBQADggEBADnajVSV7ovjR5PSVpNLw32xosj9AJtSTD+7BQIy+xUd
| 1P8hfHamtlBi5V4mbqedydDEWNt4gIemc3HBgr5Q6e+qb8inwE7F8O4LeCnjj868
| K+8Kx5FO89rw9VHGeAfAFxMhKDMR/CWLZdn1jELN86IQSl9F89h16B0XQPfnNyPh
| WGgNYLDg9G/ZD3J3cRtLnTOkXYSNpDsRneBEhAGU6HNV46OU/9YVald8Nm4d0ICu
| txl2k/74IOSih9GsEYiK6hwuP9bcJ+AF03VK1+SNWQP54VmXicsNJu/RjrgKmoW6
| Sr33U3diljDkNxxc0MZUWJ0g/cZciGYJDDQaykhGvo4=
|_-----END CERTIFICATE-----
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
445/tcp   open  microsoft-ds  syn-ack ttl 125 Windows Server 2022 Standard 20348 microsoft-ds
3387/tcp  open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
5504/tcp  open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
5985/tcp  open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
10000/tcp open  ms-wbt-server syn-ack ttl 125 Microsoft Terminal Services
|_ssl-date: 2026-02-14T12:39:22+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=austin02.SKYLARK.com
| Issuer: commonName=austin02.SKYLARK.com
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-10T01:42:09
| Not valid after:  2026-04-11T01:42:09
| MD5:   409f:353b:1f05:03b7:f532:9e21:31ea:4aa2
| SHA-1: f252:dd11:241d:1a88:ecd7:6021:8b6b:150a:dceb:9df6
| -----BEGIN CERTIFICATE-----
| MIIC7DCCAdSgAwIBAgIQGYjn+Lw7EpBGKVpNasH70zANBgkqhkiG9w0BAQsFADAf
| MR0wGwYDVQQDExRhdXN0aW4wMi5TS1lMQVJLLmNvbTAeFw0yNTEwMTAwMTQyMDla
| Fw0yNjA0MTEwMTQyMDlaMB8xHTAbBgNVBAMTFGF1c3RpbjAyLlNLWUxBUksuY29t
| MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA6WblxJU76MNa4G6NhHi4
| rZUvHlTwQQm51Zzq5SpgRmZ9ItYZOAjWYbTWyA1wb14UO3kx7HaarB/1dm4DZ9i2
| OBeNdC2bIeEdNeSUUkIo8qLQdgSFTRLoZ+B8DFbzN8We1dmCEp3jlA5IJWjmf46q
| DoG/Li5+JpAq+yZG/JqjskJJcYSPjWLrR9TegB8g0iyXdZd1h0u2pLgvUkUrUlPA
| Lyr1t0BV6SypPZLHoVwNHBbriXitcEOH2F3y5FKVCZAD7lnQjrzKSUMDu47BQVjX
| f9P/RT2W82CleWUtBR4CA3iWbz7L2k90zxA5ZPifNxbITNHqubS5LToLTnbrRzMr
| /QIDAQABoyQwIjATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJ
| KoZIhvcNAQELBQADggEBAGnvmJKNt+BwTcLpfoqUsLeE13Cr05iJn/ItxZHfNDsI
| qICVpmfth/3kwvD4q7Hau99dToO/3bwkq76XZ4RANMPZCQU7XKpVdKr2Yf08jUNh
| /+q5y20UB+2XvCz6N7Km0dRCv1jjemJKOYZsULyyfxsuMrVMsp9E2daiahSyJ2Fe
| Fr68WHmuxtUxabZT4IhnORUKTUl+C539jRfCK/R+IRQOdk12sHh9i/7NJsqI8CWj
| p6OZ2yqfd4B/zqHqQuzBu3EpwwQmNnT6XM4Km7nw4NSupYYJiRcV1qcKWBtt+Kzx
| j9QiJn87mTo7ERXpyTm6s1cSOmp+7sGd7mmpldnu9dQ=
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: SKYLARK
|   NetBIOS_Domain_Name: SKYLARK
|   NetBIOS_Computer_Name: AUSTIN02
|   DNS_Domain_Name: SKYLARK.com
|   DNS_Computer_Name: austin02.SKYLARK.com
|   DNS_Tree_Name: SKYLARK.com
|   Product_Version: 10.0.20348
|_  System_Time: 2026-02-14T12:38:54+00:00
47001/tcp open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
...
```