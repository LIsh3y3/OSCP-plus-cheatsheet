# Nmap

```zsh
PORT   STATE SERVICE REASON  VERSION
445/tcp  open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
```

---

# Enumeration

## 総合列挙

### enum4linux

```sh
================================( Share Enumeration on 192.168.227.145 )================================

do_connect: Connection to 192.168.227.145 failed (Error NT_STATUS_IO_TIMEOUT)

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        docs            Disk      Documents
        IPC$            IPC       IPC Service (APEX server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.
Unable to connect with SMB1 -- no workgroup available

```

### smbmap



---

# Screenshot


