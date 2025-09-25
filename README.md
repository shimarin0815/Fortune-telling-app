# 一行おみくじ (Ichigo Omikuji)

> きょうのヒント、そっと置いときます。 — *ワンタップ30秒*で「やさしい一行」を届けるシングルページアプリ（SPA）。

![cover](./docs/cover.png)

---

## ✨ なにこれ

* その日の気分に寄り添う**一行メッセージ**をカードで表示
* **ワンタップで占い**→必要なら**1回だけ引き直し**
* **画像としてシェア/保存**（Web Share API / PNGダウンロード）
* **テキストコピー**（SNS投稿に◎）
* **ローカル保存のアーカイブ**（直近7件）
* **テーマ切替**：Sakura / Mizu
* **プライバシー重視**：サーバー送信なし、端末ローカルのみ
* **A11y配慮**：十分なコントラスト・aria・キーボード操作

> ネガティブ断定や不安煽りはしません。\*\*今日が“少しだけ軽くなる”\*\*ことを目指しています。

---

## 🧱 技術スタック

* **React + Vite + TypeScript**（軽量・高速な開発体験）
* **Tailwind CSS**（ミニマルでやさしいUI）
* **Framer Motion**（控えめアニメーション）
* **html-to-image**（結果カードをPNG化）
* **lucide-react**（アイコン）

> 状態管理はローカルの `useState` / LocalStorage を使用。バックエンド不要の**完全クライアント**です。

---

## 🚀 クイックスタート（推奨：React 版）

### 前提

* Node.js **18+**

### 1) プロジェクトを作成

```bash
npm create vite@latest ichigo-omikuji -- --template react-ts
cd ichigo-omikuji
npm i
```

### 2) 依存をインストール

```bash
npm i framer-motion html-to-image lucide-react
```

### 3) Tailwind を設定

```bash
npm i -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

`tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{ts,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
```

`src/index.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

`src/main.tsx`（テンプレ既定のまま）

```ts
import './index.css';
```

### 4) アプリ本体を配置

* このリポジトリの **`src/App.tsx`** をあなたのプロジェクトの同名ファイルに**貼り替え**ます。

  * 既存の中身を**全削除**→`App.tsx` のコードを**丸ごとコピペ**でOK

### 5) 起動

```bash
npm run dev
```

ブラウザで [http://localhost:5173](http://localhost:5173) を開きます。

---

## 🔁 代替：ビルドなしの「バニラHTML版」

**1枚の `index.html` だけで動くデモ**も同梱できます（`/docs/vanilla-demo.html` を参照）。

* React/ビルドが難しい環境でも動作
* 実装差分：UI/機能は同等、DOM操作で簡潔に記述

> 使い方：`vanilla-demo.html` をブラウザで開くか、VS Code の Live Server でプレビュー。

---

## 🗂️ 構成（React 版）

```
├─ public/
├─ src/
│  ├─ App.tsx           # アプリ本体（カードUI・シェア機能・履歴）
│  ├─ main.tsx          # エントリ
│  └─ index.css         # Tailwind
├─ index.html
├─ tailwind.config.js
├─ package.json
└─ README.md
```

---

## ⚙️ 設定・拡張ポイント

### 文面テンプレート

* `App.tsx` 内の配列 `ONE_LINERS_YASASHIME`（一行メッセージ）と `ACTION_TIPS`（今日のひとこと）を編集
* 口調や長さは\*\*「やさしめ・短く・断定弱め」\*\*が基本

### カラーパレット

`PALETTE` に色名・HEX・短い説明を追加可能。

```ts
{ key: 'Mizu', ja: 'ミズイロ', hex: '#B7E3F9', alt: 'うがいレベルのさっぱり感' }
```

### テーマ

* `Sakura` / `Mizu` を切替（デフォルトは Sakura）
* Tailwind のトークンを増やせばテーマ追加も簡単

### 画像シェア

* `html-to-image` でカードDOMをPNG化 → Web Share API で共有
* 共有非対応環境では**自動でダウンロード**にフォールバック

### 乱数シード（“当たり”の仕組み）

* `xmur3` + `mulberry32` を用いた**擬似乱数**
* **日付 + ニックネーム**からseedを生成 → 同じ日に同じ入力なら**同じ結果**
* 引き直しは seed に `-reroll` を加算し**1回のみ**許可

### データ保存

* LocalStorage に `Reading[]` を保存（最大30件、画面表示は直近7件）
* 端末内のみ／**サーバー送信なし**

---

## ✅ アクセシビリティ

* コントラスト/フォーカスリング/aria属性
* 操作テキストを明示（例：「もう一回だけ」など）
* 動きは短く控えめ（100–150msのフェード+微スケール）

---

## 🔒 プライバシー

* 入力（ニックネーム）は**端末内保存のみ**
* 共有画像には**個人情報を含めません**
* 利用状況は（導入時に）匿名分析ツールで計測可能（例：Plausible）

---

## 🧪 スクリプト

```bash
npm run dev      # ローカル開発
npm run build    # 本番ビルド（dist/）
npm run preview  # ビルド結果の簡易サーバ
```

---

## ☁️ デプロイ

* **Vercel / Netlify / Cloudflare Pages**：`dist/` を置くだけ
* **GitHub Pages**：`npm run build` → `dist/` を `gh-pages` へ
* PWA化する場合は Service Worker（例：Workbox）を追加してください

---

## 🧩 よくある質問（Troubleshooting）

**Q. `Expression expected` / `';' expected` が出る**
A. TypeScript/Reactのコードを**そのまま `index.html` に貼っている**可能性。Vite で起動するか、バニラHTML版を使ってください。

**Q. Tailwind が効かない**
A. `tailwind.config.js` の `content` パスと、`src/index.css` の `@tailwind` 3行を確認。

**Q. 画像共有が開かない**
A. Web Share API は **HTTPS** か**ユーザー操作時**でのみ動く環境があります。未対応環境では**自動ダウンロード**に切り替わります。

---

## 🧭 ロードマップ（任意）

* [ ] PWA対応（オフライン・連続日数通知）
* [ ] OG画像自動生成（SNSカード）
* [ ] トーン追加（シュール/元気）
* [ ] 相性スナック（ニックネーム2つ）
* [ ] i18n（EN対応）

---

## 📝 ライセンス

MIT（予定）。用途に合わせて変更可。

---

## 🙌 クレジット

* 企画・文面・デザイン: **shimarin**
* 実装支援: ChatGPT（README/コード生成）

---

### コントリビュート

PR/Issue歓迎です。ポジティブに、やさしく ✨
