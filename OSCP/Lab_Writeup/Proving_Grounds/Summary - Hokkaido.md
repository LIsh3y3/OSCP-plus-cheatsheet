kerbruteで入手したユーザー名にパスワードブルートフォースをして有効なアカウントを発見。SMBを列挙して他ユーザーの認証情報を入手し、MSSQLにログイン。MSSQLでIMPERSONATEしてテーブルを列挙し、さらに他ユーザーの認証情報を入手。BloodHound でGenericWriteやForceChangePassword をエクスプロイトし横移動していき、SeBackupPrivilegeで権限昇格。

1. Port Scanをしたところ、WebサービスやMSSQLに加え、SMB、KerberosなどのActive Directory環境のポートがopenになっていることを確認

2. Webサービスから特に情報が入手できなかったため、`kerbrute`でユーザー名を列挙
```sh
kerbrute userenum --dc <DC_IP> -d <domain> /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
```

3. 見つかったユーザー名に対して、ユーザー名をパスワードとして利用してブルートフォースし、有効な認証情報を発見
```sh
kerbrute passwordspray --dc <DC_IP> -d <domain> <username wordlist> '<pw>'
```