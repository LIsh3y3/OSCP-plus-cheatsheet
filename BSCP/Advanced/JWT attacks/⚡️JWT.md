###### 検出

- ログイン後、HTTP Historyが緑でハイライトされたら検出
- JWTをIntruderで選択 -> Scanで、どの脆弱性か、秘密鍵は何かなど検出するかも。
- 下記エンドポイントにアクセスする。公開鍵など重要情報アリ
```
/.well-known/jwks.json
```

###### 目的

- `/admin`へのリクエストで`"sub": "administrator"`にすること(PrivEsc)

###### まとめ

- [1. JWT attackの基本](1.%20JWT%20attackの基本.md#JWTのBurp%20Suiteでの扱い方)

- [2. JWT署名検証の欠陥を悪用する](2.%20JWT署名検証の欠陥を悪用する.md)
	- APPERENTICE LABなので可能性低い
	- 目的の値に変更するだけ。もしくは鍵なしでsignするだけ

- [3. 秘密鍵のブルートフォース with hashcat](3.%20秘密鍵のブルートフォース%20with%20hashcat.md)
	- Scannerで自動的にSecretを検出してくれるときもある

- [4. JWTヘッダパラメタインジェクション](4.%20JWTヘッダパラメタインジェクション.md): 可能性大
	- jwk: 鍵を表す埋め込みJSONオブジェクト
	- jku: サーバが正しい鍵を含むjwkセットを取得できるURLを提供する
	- kid: どの鍵が正しいかサーバに伝える