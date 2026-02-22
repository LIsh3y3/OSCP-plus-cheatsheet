# Host Information

```zsh
172.16.104.202
```

---

# Scans

## Nmap

### quick scan

```
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: OSCP
|   NetBIOS_Domain_Name: OSCP
|   NetBIOS_Computer_Name: SRV22
|   DNS_Domain_Name: oscp.exam
|   DNS_Computer_Name: SRV22.oscp.exam
|   DNS_Tree_Name: oscp.exam
|   Product_Version: 10.0.17763
|_  System_Time: 2026-02-22T09:44:43+00:00
| ssl-cert: Subject: commonName=SRV22.oscp.exam
| Not valid before: 2026-02-21T03:01:36
|_Not valid after:  2026-08-23T03:01:36
|_ssl-date: 2026-02-22T09:45:21+00:00; +1s from scanner time.
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8080/tcp open  http          Jetty 10.0.20
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(10.0.20)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```
### full scan

```

```

### udp scan

```

```

## Rustscan

```

```

![[Pasted image 20260222184814.png]]