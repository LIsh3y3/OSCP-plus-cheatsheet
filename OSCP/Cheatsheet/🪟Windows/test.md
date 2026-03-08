
- 関連ノート：
	- [🔍Windows Local Enumeration](🔍Windows%20Local%20Enumeration.md)
	- [PrivEsc前提知識：Windowsの特権とAccess Controlの仕組み](Windowsの基礎・前提知識/PrivEsc前提知識：Windowsの特権とAccess%20Controlの仕組み.md)

> [!NOTE]
> 特権昇格した後も**再度列挙**し、重要な情報がないかどうかを探索する。
> また、Mimikatzでクレデンシャルをダンプする。


---

# Serviceを利用したPrivEsc


- Serviceについての基本事項：[用語](../../Misc/用語.md#サービス)
- Serviceの列挙：[🔍Windows Local Enumeration](🔍Windows%20Local%20Enumeration.md#サービスの列挙)

## Service Exploits - Service Binary Hijacking

サービスバイナリを直接上書き可能な場合に使用する手法。
たとえば、winPEASの結果、`Services Information`で`File Permissions: Everyone [Allow: AllAccess]`となっているような場合に使う。

1. [🔍Windows Local Enumeration](🔍Windows%20Local%20Enumeration.md#サービスの列挙)で、StartNameが"LocalSystem"であるサービスを探す
> [!TIP]
> - Stateが"Stopped"のサービスをスタートするには、一般的には高権限が必要なため、権限昇格ベクターとしての優先度は低い
> - どのサービスバイナリを書き換えればいいのかわからない場合 は、[💥Windows Privilege Escalation](#Service%20Binary%20Hijacking%20w/%20PowerUp（自動）)で列挙することも検討する
> 

2. 実行バイナリのパーミッションが(F)もしくは(M)であることを確認（[権限関連の知識、コマンド](../Common/権限関連の知識、コマンド.md#パーミッションの読み方)）
```powershell
icacls '<binary_path>'
```

3. [💥Windows Privilege Escalation](#バイナリのペイロード)を用意し、本来の実行バイナリのbkupはとった上で上書き
```powershell
mv "<binary_path>" <binary.exe.bkup>
mv <path_to_payload> "<binary_path>"
```
- mvは既存のファイルを直接上書きできないので、書き換え対象のファイルは他のディレクトリに移動させる必要がある

4. サービスを再起動する
```powershelll
net stop <service_name>
net start <service_name>
```
- [💥Windows Privilege Escalation](#Tips%20サービスの開始、終了に失敗したとき)

### Service Binary Hijacking w/ PowerUp（自動）

#### PowerUpの準備

1. 攻撃者のマシンから、以下のファイルをターゲットマシンに転送する
```
/usr/share/windows-resources/powersploit/Privesc/PowerUp.ps1
```

2. 