# vege-fru-cert（認定証デジタル発行システム）

**cert.vege-fru.com** — 日本野菜ソムリエ協会 認定証QRリダイレクトページ  
認定証の発行・管理は matching-OS リポジトリの管理画面から行う。

---

## 概要

QRコードを読み取ると `cert.vege-fru.com/?id=UUID` にアクセスされ、
Supabaseから認定証情報を取得して店舗ページへリダイレクトする。

---

## 技術スタック

| 項目 | 内容 |
|------|------|
| 実装 | Vanilla HTML / JavaScript（フレームワークなし） |
| DB参照 | Supabase JS（CDN経由） |
| ホスティング | Vercel（vege-fru-cert プロジェクト） |
| ドメイン | cert.vege-fru.com（お名前.com CNAME → Vercel） |

---

## 環境構築手順

### 1. クローン

```bash
git clone https://github.com/yumoto54/vege-fru-cert.git
cd vege-fru-cert
```

### 2. 環境変数

**このリポジトリに環境変数は不要。**  
Supabaseの接続情報（anon key）は `index.html` 内に直接記載されている。  
anon keyは公開しても安全（Supabaseの設計上、RLSで保護されている）。

### 3. ローカル確認

```bash
npx serve .
# または index.html をブラウザで直接開く
```

---

## デプロイ

mainブランチへのpushで自動デプロイ（Vercel連携済み）

```bash
git add -A
git commit -m "feat: 変更内容"
git push
```

---

## ファイル構成

```
vege-fru-cert/
└── index.html   # 全処理をここで完結（リダイレクト・表示・期限チェック）
```

---

## 動作フロー

1. QRコード読み取り → `cert.vege-fru.com/?id=UUID`
2. Supabase `restaurant_certificates` テーブルを参照
3. `redirect_url` 設定あり → 店舗ページへリダイレクト
4. 未設定 → 認定証情報を画面表示
5. 期限切れ → 警告を表示

---

## Supabaseテーブル（参照のみ）

```sql
restaurant_certificates (
  id          UUID PRIMARY KEY,
  restaurant_name TEXT,
  cert_type   TEXT,   -- 'cafe' or 'restaurant'
  cert_number TEXT,
  valid_from  DATE,
  valid_until DATE,
  redirect_url TEXT,
  created_at  TIMESTAMPTZ
)
```

---

## 注意事項

- `index.html` 内のSupabase URLとanon keyは変更しない
- Service Role Keyは使用しない・記載しない
- 認定証の発行・管理は matching-OS の管理画面（/admin/certificates）から行う
- レイアウト設定は matching-OS の /admin/cert-layout から調整する
