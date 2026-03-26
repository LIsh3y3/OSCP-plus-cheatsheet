- 関連ノート：
	- [135,593 - MSRPC](135,593%20-%20MSRPC.md)

# 目的・シチュエーション

目的：
- ターゲットのRPCサービスのポート情報を収集し、次の攻撃対象（例：NFS, rusersd）を見つける
- NULLセッションでの接続確認とユーザー列挙

シチュエーション：
- LinuxもしくはWindowsマシンに対して使用可能

---

# RPCサービスの列挙

バナー取得（手動）
```zsh
nc -nvC <target_IP> 111
```

サービスの簡易一覧（概要）
```zsh
rpcinfo -s <target_IP>
```

RPCサービスごとのポートを列挙
```zsh
rpcinfo -p <target_IP>
```

---

# NULLセッションでのユーザー列挙

```zsh
rpcclient -U "" <target_IP>
```

ログイン後のコマンド例（対話モード）
```zsh
rpcclient $> enumdomusers
rpcclient $> queryuser 0x<RID>
```

- その他のコマンドは[135,593 - MSRPC](135,593%20-%20MSRPC.md)を参照