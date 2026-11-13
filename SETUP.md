# トキカサネ「みんなの地図」の設定手順

無料の Firebase（Spark プラン）だけで動きます。所要時間はおよそ15分です。

## 1. Firebase プロジェクトを作る
1. https://console.firebase.google.com を開き「プロジェクトを作成」
2. 名前は自由（例：tokikasane）。Google アナリティクスはオフで構いません。

## 2. 匿名ログインを有効にする
投稿した人を見分けるため（本人だけが削除できるように）に使います。利用者の登録は不要です。
1. 左メニュー「構築」→「Authentication」→「始める」
2. 「ログイン方法」タブ →「匿名」→ 有効にして保存

## 3. データベースを作る
1. 「構築」→「Firestore Database」→「データベースを作成」
2. ロケーションは `asia-northeast1`（東京）がおすすめ
3. 「本番環境モード」で作成
4. 「ルール」タブを開き、このフォルダの `firestore.rules` の中身をすべて貼り付けて「公開」

※ 写真はデータベースに直接保存するので、有料プランが必要な Cloud Storage は使いません。

## 4. 設定値を config.js に書く
1. 歯車アイコン「プロジェクトの設定」→「マイアプリ」→ ウェブ（`</>`）アイコン
2. アプリ名を入れて登録（Hosting のチェックは任意）
3. 表示された `firebaseConfig = { ... }` の中身を `config.js` にコピー

```js
export const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "tokikasane-xxxx.firebaseapp.com",
  projectId: "tokikasane-xxxx",
  appId: "1:1234567890:web:abcdef",
};
```

この値は公開されても問題ないものです（安全性は `firestore.rules` で守っています）。

## 5. 公開する（https 必須）
どちらか一方で構いません。

**A. Firebase Hosting**（同じプロジェクトで完結）
```bash
npx firebase-tools login
```
```bash
npx firebase-tools deploy --project tokikasane-xxxx
```
`https://tokikasane-xxxx.web.app` で公開されます。ルールも同時に反映されます。

**B. Netlify Drop**
https://app.netlify.com/drop にこのフォルダをドラッグ。

## 運営のしかた
- **報告された写真**：Firestore の `spots` → 各投稿 → `reports` に入ります。
  問題があれば `spots/{ID}` と `images/{ID}` の2つを削除してください。
- **投稿者本人**はアプリから「取り下げ」できます（同じ端末・ブラウザに限る）。
- **無料枠の目安**：保存 1GB（写真 約2,000〜3,000枚）、読み取り 1日5万回。
  地図の表示で投稿数ぶん、写真を開くたびに1回の読み取りが発生します。
- 利用者が増えて荒らしが心配になったら、Firebase の「App Check」を有効にすると、
  このサイト以外からの書き込みを防げます。

## 手元でのテスト（任意）
```bash
npx firebase-tools emulators:start --only firestore,auth --project demo-tokikasane
```
別のターミナルでこのフォルダを `python3 -m http.server 8765` で配信し、
`config.js` に `projectId: "demo-tokikasane"` を入れて
`http://localhost:8765/?emulator=1` を開くと、本番に触れずに試せます。
