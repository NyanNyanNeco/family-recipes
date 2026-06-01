# 🍳 家族のレシピ帳

家族みんなのレシピをリアルタイムで管理・共有できるPWAアプリです。

---

## ✨ 機能

| 機能 | 説明 |
|------|------|
| 📋 レシピ管理 | レシピの追加・編集・削除 |
| 🔍 検索・フィルタ | レシピ名・タグ・カテゴリで絞り込み |
| ⭐ 評価 | 家族メンバーごとに星評価 |
| ❤️ お気に入り | メンバーごとのお気に入り登録 |
| 📅 献立カレンダー | 週単位で昼食・夕食の献立を計画 |
| 🛒 買い物リスト | 献立から材料を自動まとめ |
| 📤 エクスポート / 📥 インポート | JSONファイルでデータの持ち出し・取り込み |
| 📲 PWAインストール | ホーム画面に追加してアプリとして利用可能 |
| ☁️ リアルタイム同期 | Firebase Firestoreで家族間リアルタイム共有 |

---

## 📁 ファイル構成

```
/
├── index.html      # アプリ本体（HTML + CSS + JS 一体型）
├── icon-192.png    # PWAアイコン（192×192px）
└── icon-512.png    # PWAアイコン（512×512px）
```

> manifest.json・service worker はすべて `index.html` 内で動的生成されるため、追加ファイルは不要です。

---

## 🚀 使い方

### GitHub Pages で公開する場合

1. このリポジトリを **Settings → Pages** で `main` ブランチのルートを公開に設定
2. 表示された URL をブラウザで開く
3. Androidなら「ホーム画面に追加」ボタン、iOSならSafariの共有メニューから「ホーム画面に追加」

### ローカルで動かす場合

```bash
# 簡易HTTPサーバーを立てる（Python 3）
python -m http.server 8080
```

ブラウザで `http://localhost:8080` を開く。

> ⚠️ `file://` で直接開くとFirebase・Service Workerが正しく動作しません。必ずHTTPサーバー経由で開いてください。

---

## 🔥 Firebase 設定

Firebase Firestoreを使ってリアルタイム同期を行っています。

### 初期設定手順

1. [Firebase Console](https://console.firebase.google.com/) でプロジェクトを作成
2. **Firestore Database** を作成（テストモードで開始）
3. **プロジェクトの設定 → アプリを追加（Web）** から設定値を取得
4. `index.html` 内の以下の箇所を書き換える

```javascript
const FB_CONFIG = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### Firestore セキュリティルール（推奨）

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true; // 家族内利用のため全許可（必要に応じて制限）
    }
  }
}
```

---

## 👨‍👩‍👧‍👦 家族メンバーのカスタマイズ

初期メンバーは `["みんな", "パパ", "ママ", "太郎", "花子"]` です。  
アプリ内の「メンバー編集」から変更できます。

---

## 📱 対応環境

| 環境 | 対応状況 |
|------|---------|
| Chrome（Android） | ✅ PWAインストール対応 |
| Safari（iOS） | ✅ ホーム画面追加対応 |
| Chrome（PC） | ✅ PWAインストール対応 |
| Firefox | ✅ ブラウザで利用可能 |

---

## 📄 ライセンス

このプロジェクトは個人・家族利用を目的として作成されています。
