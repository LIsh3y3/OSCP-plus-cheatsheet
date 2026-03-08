
#### 注目ポイント

- "Check Stock"機能：`/product/stock`
- Blogの"next"や"back"リンク:  [⚡️SSRF](⚡️SSRF.md#Open%20redirect脆弱性を使用したフィルタのバイパス)
- My Accountで`o-auth`Sign-inページにリダイレクトされる：[⚡️SSRF](⚡️SSRF.md#&OpenID)

---
## Detect

- Issue: External service intraction(HTTP)
- 基本はユーザ制御可能な値で動的にアクセス先を決める場所

#### ①脆弱性の場所を探す

###### リクエスト内にURLを指定しているか？

- Check Stock機能
```http
stockAPI=https://example.com/check?productId=1234
```
- 🚨Open redirectを使用してフィルタをバイパスしないとSSRFが悪用できないケースもある[⚡️SSRF](⚡️SSRF.md#Open%20redirect脆弱性を使用したフィルタのバイパス)

###### XML内にURLがあるか？

- Check Stock機能
```dtd
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://example.com/"> ]>
<stockCheck>
  <productId>3</productId>
</stockCheck>
```
[⚡️SSRF](⚡️SSRF.md#&%20XXE)

###### Refererヘッダに指定したURLにアクセスしてくれるか？

```http
Referer: https://COLLABORATOR_DOMAIN
```
- [⚡️SSRF](⚡️SSRF.md#Routing%20based)

###### OIDCのRP登録エンドポイントが閲覧できるか？

1. [3. OpenID Connectとその脆弱性](../../Advanced/OAuth/3.%20OpenID%20Connectとその脆弱性.md#OIDC利用しているかの特定方法4つ)
![[Pasted image 20240209100552.png | 800]]
2. 上記のエンドポイントで"registration_endpoint"とURLが記載されているか？
- NO -> [🔍OAuth](../../Advanced/OAuth/🔍OAuth.md)

- [⚡️SSRF](⚡️SSRF.md#&OpenID)

###### Hostヘッダに指定した内部アドレスにアクセスするか？

- 別のStage (Stage1)[🔍HTTP Host header attack](../../Advanced/HTTP%20Host%20header%20attacks/🔍HTTP%20Host%20header%20attack.md#⚡️HHI%20&%20Routing-based%20SSRF)

---
#### ②内部ファイルを探す

###### 内部アドレスを探す

httpでうまくいかなければhttps試すこと

- ⭐️localhost(試験ではこれだけ)
```
http://localhost:6566/
```
```
http://127.1:6566/
```
- [2.  一般的なSSRF防御の回避](2.%20%20一般的なSSRF防御の回避.md#IPアドレスの代替表記)

- プライベートIPアドレス(試験では出ない)
	- Intruder -> Payload type: Numbers -> 0 to 255 step 1
```
http://192.168.0.§x§:6566/
```

###### ファイルを探す

- 発見した内部アドレスにIntruder
	- Payload Settings->Add from list-> Filenames-long
```
http://localhost:6566/§x§
```