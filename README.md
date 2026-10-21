# vege-fru-cert

**cert.vege-fru.com** — 日本野菜ソムリエ協会 認定証デジタル発行システム

## 概要
QRコードからアクセスされた認定店舗URLへリダイレクト、または認定証情報を表示する。

## 動作フロー
1. QRコード読み取り → `cert.vege-fru.com/?id=UUID`
2. Supabase `restaurant_certificates` テーブルを参照
3. `redirect_url` 設定あり → 店舗ページへリダイレクト
4. 未設定 → 認定証情報を表示 / 期限切れ → 警告表示

## デプロイ
mainブランチへのpushで自動デプロイ（Vercel連携済み）

## 認定証の発行・管理
管理画面（selection.vege-fru.com/admin/certificates）から行う。
レイアウト設定は /admin/cert-layout から調整可能。
