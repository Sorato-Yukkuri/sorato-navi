# そらとナビ

そらとアプリシリーズ 第2作 - ブラウザで動くナビゲーションアプリ

## 技術スタック

- **React + TypeScript** (Vite)
- **Leaflet / react-leaflet** - 地図表示
- **Tailwind CSS** - スタイリング
- **OpenStreetMap / Nominatim** - 地図データ・場所検索（無料・サーバー不要）

## GitHub Pages へのデプロイ

### 自動デプロイ（推奨）

1. このリポジトリを GitHub にプッシュ
2. Settings → Pages → Source を **GitHub Actions** に設定
3. `main` ブランチにプッシュすると自動でビルド・デプロイされます

### 手動ビルド

```bash
npm install
npm run build
# dist/ フォルダを GitHub Pages の root として公開
```

## 修正したバグ

### 「位置情報を読み込み中」で止まる問題

**原因**: `use-geolocation.ts` に以下の問題がありました：

1. **タイムアウト時にloadingが解除されない**: 高精度GPS取得（10秒タイムアウト）が失敗しても、すでに低精度で位置取得済みの場合でも `loading: true` のまま止まるケースがあった

2. **初期状態で `loading: false` でなかった**: ページ読み込み直後は位置を取得しようとしていないのに `loading` が曖昧な状態になることがあった

**修正内容** (`src/hooks/use-geolocation.ts`):
- 初期値を `loading: false` に明示
- **全体タイムアウト（15秒）** を追加: 何があっても15秒後には `loading: false` になる
- 高精度取得が失敗しても **低精度で位置を取得できていれば正常終了** するよう修正

## 主な機能

- 📍 現在地表示（高精度GPS）
- 🗺️ 地図タップで目的地設定
- 🔍 場所名検索（Nominatim API使用）
- 🧭 音声案内（Web Speech API）
- 🔖 お気に入り場所の保存
- 🚶/🚗 徒歩・車モード切替
- 🗺️ 地図スタイル切替（標準・衛星・地形）
