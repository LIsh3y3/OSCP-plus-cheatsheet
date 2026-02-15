# Nmap

```zsh
PORT   STATE SERVICE REASON  VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
```

---

# Enumeration

- NULLセッションン接続試行 -> 失敗
```sh
┌──(koshi㉿kali)-[~/PEN-200/Skylark/VM12]
└─$ rpcclient $TargetIP -U "" -N
Cannot connect to server.  Error was NT_STATUS_ACCESS_DENIED
```


---

# Screenshot


