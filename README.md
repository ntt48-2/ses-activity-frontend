# symptom-checker-frontend

症状・薬チェッカーのフロントエンド。  
Next.js 15 + TypeScript + Tailwind CSS で構成。

---

## 技術スタック

| 種別 | 内容 |
|---|---|
| フレームワーク | Next.js 15（App Router） |
| 言語 | TypeScript |
| スタイリング | Tailwind CSS |
| 実行環境 | Node.js 20 |

---

## 開発環境構築

### 前提条件

- Docker / Docker Compose がインストール済みであること
- バックエンド（symptom-checker-backend）が起動済みであること

### 1. リポジトリをクローン

```bash
git clone https://github.com/your-org/symptom-checker-frontend.git
cd symptom-checker-frontend
```

### 2. Docker で起動

```bash
docker compose -f docker-compose.dev.yml up --build
```

初回起動時は `npm install` が走るため数分かかる。

### 3. 起動確認

ブラウザで http://localhost:3000 を開いて画面が表示されれば成功。

---

## VSCode でコード補完を有効にする（推奨）

Docker 内で `node_modules` が作成されるが、VSCode のコード補完はローカルの `node_modules` を参照するため、ローカルにも別途インストールが必要。

### nvm（Node バージョン管理）のインストール

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc  # zsh の場合は ~/.zshrc
```

### Node.js 20 をインストール

```bash
nvm install 20
nvm use 20
```

### ローカルに依存関係をインストール

```bash
# node_modules の権限エラーが出る場合
sudo chown -R $USER:$USER .

npm install --legacy-peer-deps
```

---

## URL一覧

| 環境 | URL |
|---|---|
| ローカル（フロントエンド） | http://localhost:3000 |
| バックエンドAPI | http://localhost:8080 |

---

## バックエンドとの起動順序

バックエンドを先に起動してからフロントエンドを起動する。

```bash
# ターミナル1：バックエンド
cd symptom-checker-backend
docker compose -f docker-compose.dev.yml up

# ターミナル2：フロントエンド
cd symptom-checker-frontend
docker compose -f docker-compose.dev.yml up
```

---

## ホットリロード

ファイルを保存するとブラウザが自動で更新される。  
Docker 環境では `WATCHPACK_POLLING=true` で動作させているため、保存から反映まで 1〜2 秒かかる場合がある。

---

## ディレクトリ構成

```
src/
└── app/
    ├── layout.tsx                   # 全ページ共通の外枠（ヘッダー・フッター）
    ├── page.tsx                     # トップページ（/）
    ├── globals.css                  # グローバルCSS（Tailwind読み込み）
    ├── symptoms/
    │   └── result/
    │       └── page.tsx             # 症状チェック結果（/symptoms/result）
    └── drugs/
        ├── page.tsx                 # 薬チェック（/drugs）
        └── result/
            └── page.tsx             # 薬チェック結果（/drugs/result）
```
