# トキカサネ

昔の写真を、今の町にかさねて見るWebアプリです。スマホのカメラを町に向けると、その場所の古い写真が半透明で重なります。

## できること
- 古い写真を登録して、カメラ映像に重ねる（すかし／ワイプ、指で位置・大きさ・回転を合わせる）
- 撮影地点までの距離と、向く方向の案内
- 重ねた画面を画像として保存・共有
- 写真の中の人・車・看板・水路などをタップで切り抜き、部品ごとに表示／非表示・濃さ・位置を変える（切り抜きAIは MediaPipe。写真は端末の外に送りません）
- 「みんなの地図」で、投稿された昔の写真を誰でも見られる（Firebase の設定が必要）

## GitHub Pages で公開する
1. リポジトリの **Settings → Pages** を開く
2. **Source** を「Deploy from a branch」、ブランチを `main`、フォルダを `/ (root)` にして Save
3. 1〜2分後に表示される `https://<ユーザー名>.github.io/<リポジトリ名>/` を開く

カメラは https でしか動かないので、GitHub Pages のURLならそのまま使えます。

## みんなの地図を有効にする
`config.js` に Firebase の設定値を書くと有効になります。手順は [SETUP.md](SETUP.md) を見てください。
設定しなくても、自分の写真を重ねる機能は使えます。

## ファイル
| ファイル | 内容 |
| --- | --- |
| `index.html` | アプリ本体 |
| `config.js` | Firebase の設定値 |
| `firestore.rules` | データベースの安全ルール（Firebase コンソールに貼る） |
| `firebase.json` | Firebase エミュレータ・Hosting 用の設定 |
| `SETUP.md` | Firebase の設定手順 |

地図データ © OpenStreetMap contributors
