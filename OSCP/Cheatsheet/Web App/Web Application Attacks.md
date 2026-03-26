# 基本情報

## Webアプリのディレクトリ

- LinuxシステムのApache Webアプリでは`/var/www/html`がWebのrootディレクトリとして使われることが多い
	- → つまり、例えば、http://example.com/file.txtと入力すると、`/var/www/html/file.txt`にアクセスできるということ
- WindowsシステムのIIS Webアプリでは、`C:\inetpub\wwwroot`
- このディレクトリに書き込み可能なら、Webshell獲得できるかも（[Bind & Reverse Shell・ペイロード・安定化手法](../Common/Bind%20&%20Reverse%20Shell・ペイロード・安定化手法.md#注意＆Tips)）

---

## Webアプリのユーザー

- セキュリティ上、Webのユーザーは権限が低く設定されていることが多い
	- （高権限をもつこともある）

- Debian, Nginxなどは`www-data`
- Apacheは`apache`
- WindowsのWebサーバー`IIS`においては、以下2つがアカウント
	- Network Service（旧）
	- IIS Application Pool Identity（新）（※）
		- (※：Application Pool＝Webアプリを独立して管理する仕組みで、安定性・セキュリティ向上に役立つ)
		- 例：`IIS APPPOOL\DefaultAppPool` のような形式で、ACLを使ってアプリごとに適切な権限が付与される
		
---

## Tips

- リクエスト・レスポンスの中身はBurp Suiteもしくはcurlコマンドで閲覧すること
- （ブラウザ上で閲覧すると、レンダリングの際に自動で情報が抜け落ちることがある）

---
---

# 攻撃手法


---

## API Test

- 基本はBSCPのAPI testingノートを参照→[🔍API testing](🔍API%20testing.md)
- API endpointの探索は、[👻Gobuster](../../Tools/👻Gobuster.md#Summery%20Gobuster)を参照

---

## XSS

- 基本はBSCP valutのXSSノートを参照→[🔍XSS](🔍XSS.md)

---

## Path traversal

- BSCP valutのPath traversalノートを参照→[🔍Path traversal](🔍Path%20traversal.md)

---


---
---

# Misc

## 開発者ツール

- BSCPノートを参照→[ブラウザ開発者ツール](ブラウザ開発者ツール.md)
