###### 🚨
Client-sideはStage2(CSPP & DOM XSSはStage1)
Server-sideはStage2とStage3: Info窃取(by RCE)

###### Burp browserの設定

![[Pasted image 20240209104255.png | 300]]
![[Pasted image 20240209104326.png | 300]]

## Client-Side(Stage:2)

- [3. Client-side prototype pollution(CSPP)](3.%20Client-side%20prototype%20pollution(CSPP).md#CSPPのメソドロジー)

## Server-Side(Stage:2 & 3)

- Run maintainance job機能など、あらたなオブジェクトを作成しているリクエストが狙い目![[Pasted image 20240209114313.png]]
- [4. Server-side prototype pollution(SSPP)](4.%20Server-side%20prototype%20pollution(SSPP).md#SSPPのメソドロジー)
