# rs_auth_passkey_pg

 Version: 0.9.1

 Author  : 

 date    : 2026/11/18
 
 update  :

***

Rust Axum , Passkey Authrize PostgreSQL Example

* PostgreSQL DB
* windows11 , Windows hello
* node22
* Linux

***
* .env
```
DATABASE_URL=postgres://root:admin@localhost:5432/mydb
# WebAuthn の RP ID（ドメイン名）と Origin。ローカル開発は localhost のまま
RP_ID=localhost
RP_ORIGIN=http://localhost:3000
BIND_ADDR=127.0.0.1:3000
# HTTPS 運用時は true にする
COOKIE_SECURE=false
RUST_LOG=info,passkey_axum=debug
```

***
* start
```
cargo build
cargo run
```


***
## 補足

* ブラウザで http://localhost:3000 を開き、ユーザー名を入力して「Windows Hello でパスキーを登録」→「パスキーでログイン」の順に操作してください。ブラウザは Windows 上のもの
 （Edge / Chrome）である必要があります。

***
* 主な実装内容

 - 登録は platform 認証器（Windows Hello）に限定。
 - ログインは userVerification=required で、PIN・顔・指紋による本人確認が必須。
 - チャレンジは DB に保存し、5 分の期限付き・1 回限りで消費（リプレイ防止）。
 - 署名カウンタは認証のたびに更新。
 - セッションは 256bit の乱数トークンを HttpOnly / SameSite=Strict Cookie で渡し、DB には SHA-256 ハッシュのみ保存。

***
* 本番に向けた注意
 - /api/login/start はユーザーの存在有無を返すため、列挙可能です。本番では応答の統一やレート制限が必要です。
 - RP_ID / RP_ORIGIN を本番ドメインに合わせ、HTTPS 運用時は COOKIE_SECURE=true にしてください。
 - パスキーの追加・削除 API は含めていません。

***
### version
* V_0_9_1: new

***
### blog

https://zenn.dev/knaka0209/scraps/31c994860f1fa7

