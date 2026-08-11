# 駅オタク — セットアップガイド

## ファイル構成

```
ekiotaku/
├── index.html   ← アプリ本体(このファイルだけで動く)
└── README.md    ← このファイル
```

---

## STEP 1 — Firebase プロジェクトを作る

1. [https://console.firebase.google.com](https://console.firebase.google.com) を開く
2. **「プロジェクトを追加」** をクリック
3. プロジェクト名を入力(例: `ekiotaku`)して作成
4. Google アナリティクスは「今はしない」でOK

---

## STEP 2 — Realtime Database を有効にする

1. 左メニューの **「構築」→「Realtime Database」** をクリック
2. **「データベースを作成」** をクリック
3. ロケーションは **「asia-southeast1(シンガポール)」** を選択
4. セキュリティルールは **「テストモードで開始」** を選択 → 有効にする

> ⚠️ テストモードは30日間で期限切れになります。本番公開前に下記のルールに変更してください。

**本番用セキュリティルール(Realtime Databaseのルールタブに貼り付け):**

```json
{
  "rules": {
    "rankings": {
      ".read": true,
      ".write": true,
      "$entry": {
        ".validate": "newData.hasChildren(['name','score','ts']) &&
                      newData.child('score').isNumber() &&
                      newData.child('score').val() >= 0 &&
                      newData.child('score').val() <= 500 &&
                      newData.child('name').isString() &&
                      newData.child('name').val().length <= 12"
      }
    }
  }
}
```

---

## STEP 3 — APIキーを取得して index.html に貼り付ける

1. Firebase コンソールの左上の⚙️(プロジェクトの設定)をクリック
2. 「全般」タブの下にある **「マイアプリ」→「</>ウェブ」** をクリック
3. アプリ名を入力(なんでもOK)して登録
4. 表示された `firebaseConfig` の中身をコピーする

```js
// このような形式でコピーされる
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "ekiotaku-xxxx.firebaseapp.com",
  databaseURL: "https://ekiotaku-xxxx-default-rtdb.firebaseio.com",
  projectId: "ekiotaku-xxxx",
  storageBucket: "ekiotaku-xxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

5. `index.html` の以下の部分を上記の値で書き換える

```js
// index.html の中にある FIREBASE_CONFIG を探して書き換える
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSy...",        // ← ここを書き換え
  authDomain:        "ekiotaku-xxxx.firebaseapp.com",
  databaseURL:       "https://ekiotaku-xxxx-default-rtdb.firebaseio.com",
  projectId:         "ekiotaku-xxxx",
  storageBucket:     "ekiotaku-xxxx.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abcdef"
};
```

---

## STEP 4 — GitHub にアップロードする

1. [https://github.com](https://github.com) にログイン
2. 右上の **「+」→「New repository」** をクリック
3. Repository name に `ekiotaku` と入力
4. **Public** を選択 → **「Create repository」**
5. 「uploading an existing file」リンクをクリック
6. `index.html` と `README.md` をドラッグ&ドロップ
7. 「Commit changes」をクリック

---

## STEP 5 — GitHub Pages を有効にする

1. リポジトリの **「Settings」タブ** をクリック
2. 左メニューの **「Pages」** をクリック
3. Source を **「Deploy from a branch」** に設定
4. Branch を **「main」→「/(root)」** に設定して **「Save」**
5. 数分後に以下のURLで公開される

```
https://あなたのGitHubユーザー名.github.io/ekiotaku/
```

---

## Firebase 無料枠の目安

| 項目 | 無料枠 |
|------|--------|
| 同時接続数 | 100接続 |
| データ転送量 | 1GB/月 |
| ストレージ | 1GB |

駅オタク程度の規模では無料枠を超えることはほぼありません。
