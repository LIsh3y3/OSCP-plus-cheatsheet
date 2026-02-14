Tecrail RESPONSIVE filemanger の Path Traversal の脆弱性をエクスプロイトするPoCに軽微な変更を加え、OpenEMRというOSSからSQLの認証情報を抽出。
リモートでMySQLに接続し、OpenEMRの認証情報ハッシュをダンプし、クラック。
OpenEMRのRCE (Authenticated)の脆弱性を使い、Foothold を獲得。
Kernel Exploit である PwnKit を用いて権限昇格。

---

1. Port Scanをし、HTTPサービスとSamba、MySQLが動作していることを確認

2. Webにアクセスし、サイトを巡回していると、ユーザー名の候補やOpenEMRのログインフォームを発見

3. ディレクトリスキャンをし、Responsive Filemanager 9.13.4を発見