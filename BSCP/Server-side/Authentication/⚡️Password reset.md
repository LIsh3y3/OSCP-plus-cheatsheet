#### 注目ポイント

- Change password
- Forgot password

###### Change passwordによるパスワードリセット(APPRENTICE)

- tokenパラメタの値を削除する
- [4. その他認証メカニズムによる脆弱性](4.%20その他認証メカニズムによる脆弱性.md#パスワードをリセットする)

###### Forgot passwordによるパスワードリセット

- `current-password`(現在のパスワード)のパラメタと値を削除する
- 具体例：[Github cheat sheet: Current Password](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study/tree/main?tab=readme-ov-file#current-password)
- [⚡️Business logic vuln](../Business%20logic%20vuln/⚡️Business%20logic%20vuln.md)の一種

#### HHIによるパスワードリセット

- [⚡️HTTP Host header attack](../../Advanced/HTTP%20Host%20header%20attacks/⚡️HTTP%20Host%20header%20attack.md#Password%20reset%20poisoning)