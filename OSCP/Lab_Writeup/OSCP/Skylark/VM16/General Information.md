# Host Information

```zsh

```

---

# Scans

## Nmap

### quick scan

```
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3a:86:87:62:d1:b3:e3:74:97:fa:94:4b:31:a8:41:e5 (RSA)
|   256 64:b0:c3:72:98:ec:47:ca:77:01:5d:73:5d:12:4b:69 (ECDSA)
|_  256 de:0c:d1:ed:27:70:b6:9f:1f:99:31:e8:eb:cd:ff:b7 (ED25519)
3128/tcp open  squid-http?
8000/tcp open  http        Apache httpd 2.4.41
|_http-title: Index of /
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| 1.1K  2026-02-14 07:40  debug.txt
|_
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
### full scan

```
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3a:86:87:62:d1:b3:e3:74:97:fa:94:4b:31:a8:41:e5 (RSA)
|   256 64:b0:c3:72:98:ec:47:ca:77:01:5d:73:5d:12:4b:69 (ECDSA)
|_  256 de:0c:d1:ed:27:70:b6:9f:1f:99:31:e8:eb:cd:ff:b7 (ED25519)
3128/tcp open  squid-http?
8000/tcp open  http        Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| 1.1K  2026-02-14 07:48  debug.txt
|_
|_http-title: Index of /
```

### udp scan

```
PORT      STATE         SERVICE     VERSION
22/tcp    open          ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
3128/tcp  open          squid-http?
8000/tcp  open          http        Apache httpd 2.4.41
111/udp   open|filtered rpcbind
...
```

## Rustscan

```
PORT     STATE SERVICE     REASON         VERSION
22/tcp   open  ssh         syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3a:86:87:62:d1:b3:e3:74:97:fa:94:4b:31:a8:41:e5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDbVTnB85/XY7iFS2vyyomPyOXe8ckK4kBAZ/a725H2e3kC/coaeM6ASrFtDofAYrMALyX6kDSJZq8E8xVaJfEKB4PwYZsQOjzSzvrHEmYgjcnO4UJRyx9MTywj/y0e6lcZGy593UjaGLow8plpcOKZrgJ9Wpghx7YJJjSMTkI/SeS5cRI6MfmXcnjkF+6RXydRGBVop95PRNodRRca8y6SxmyNpfx93Z/9pOpzqq4a9CXxHss2vRp8cY99R7fvq1r4FL8hEvbUQjtnS9XOx12pxunsS2nLan3zPBHXn5sMLgM8fYzgS0jzbLtxLGT6no3G0YyqS5qd6ovUboeX3nwleyev/r5UT8Cwbb19zD/pAshRhPgsJpD4cvUDWopC7ywuCTI6gYr2rvSsA8h300cIqk9VeIZ67ry/1RX9iAVRFbAGFMkfDD55mBRNR2qxP4cnFBlJ+6G3UqVv2Ovl3jCuficOip24AHW14gsNhWd4Ucn8L5iDBME5bc3BTXzI2v0=
|   256 64:b0:c3:72:98:ec:47:ca:77:01:5d:73:5d:12:4b:69 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJnjSSJ0dYXkD3Q51X1r6wjZq6crazCQ14XF8TQF1ZPxJJVTWRm/SZeqkCHQzRH8BdPtrjwfB/pG6wkd14B3fXA=
|   256 de:0c:d1:ed:27:70:b6:9f:1f:99:31:e8:eb:cd:ff:b7 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHJGWgFuo9+cQrE+CwvVz1aDFmKhTgPKBKIEUV1Ua4f7
3128/tcp open  squid-http? syn-ack ttl 61
8000/tcp open  http        syn-ack ttl 61 Apache httpd 2.4.41
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Index of /
| http-ls: Volume /
| SIZE  TIME              FILENAME
| 1.1K  2026-02-14 07:57  debug.txt
```