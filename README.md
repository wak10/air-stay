# yadori — 旅館業 運営管理代行 LP

合同会社やどりの受託営業用ランディングページ。

- 公開URL: **https://management.yadori-llc.com/**
- ホスティング: GitHub Pages（このリポジトリ単独）
- コーポレートサイト: https://yadori-llc.com （別リポジトリ `yadori-hp`）

## 構成

```
index.html              LP本体（CSS/JSインライン、単一ファイル完結）
CNAME                   management.yadori-llc.com
.nojekyll               Jekyll処理を無効化
favicon.png
assets/
  brand/logo-nav.png    ナビ用ロゴ
  brand/logo.png        フッター用ロゴ
  img/*.jpg             施設写真 13点
```

依存パッケージ・ビルド工程はありません。`index.html` を直接編集します。
外部読み込みは Google Fonts（Noto Sans JP）のみです。

## ローカル確認

```bash
python3 -m http.server 5510 --directory ~/dev/yadori-management-lp
```

http://localhost:5510 で確認できます。

## 公開手順（初回）

### 1. GitHubリポジトリを作成してpush

```bash
cd ~/dev/yadori-management-lp
gh repo create yadori-management-lp --public --source=. --remote=origin --push
```

### 2. GitHub Pages を有効化

リポジトリの Settings → Pages で以下を設定します。

- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`
- Custom domain: `management.yadori-llc.com`
- Enforce HTTPS: 有効（証明書の発行まで数分〜1時間程度かかります）

### 3. DNS にCNAMEレコードを追加

`yadori-llc.com` を管理しているDNSに、次の1レコードを追加します。

| Type | Name | Value | TTL |
|---|---|---|---|
| CNAME | `management` | `wak10.github.io.` | 3600（自動） |

> apexドメイン（`yadori-llc.com`）側のAレコードは変更しないでください。コーポレートサイトが停止します。

反映確認:

```bash
dig +short management.yadori-llc.com
curl -sI https://management.yadori-llc.com/ | head -1
```

## 更新手順（2回目以降）

```bash
cd ~/dev/yadori-management-lp
git add -A && git commit -m "変更内容" && git push
```

push後 1〜2分で反映されます。

## 問い合わせ窓口

コーポレートサイトと共通の Google フォームとメールアドレスを使用しています。
LP側に独自のフォームやサーバーは持っていません。

- フォーム: `https://docs.google.com/forms/d/e/1FAIpQLScDaAVj559YMA7w5Mh1OjoHzzynQUfBV-Idlr74y2hrEewQeA/viewform`
- メール: `yadori.2026@gmail.com`

変更する場合は `index.html` 内の該当URL（9箇所）とメールアドレス（2箇所）を置換してください。

## 内容を変更するときの主な箇所

| 変更したいもの | 場所 |
|---|---|
| 手数料・最低手数料 | `.plan__rate` / `.plan__min`、比較表の最下部、シミュレーターの `data-rate` `data-min` |
| 複数棟割引 | JS内 `DISCOUNT = { 1:0, 3:1, 5:2 }` と `.sim__tabs--units` の表記 |
| OTA手数料率 | JS内 `OTA_RATE = 0.03` |
| 駆けつけの提供状況 | フルプランの `.plan__note`、比較表「現地駆けつけ対応」の行 |
| 実績数値 | `#result` セクション |

設計の判断根拠と残論点は `~/dev/yadori-lodging-plan-design.md` にまとめてあります
（公開リポジトリには含めないでください）。

## 関連する変更（`yadori-hp` 側）

- 全ページのナビ／フッターに「運営管理代行」を追加し、リンク先を本サブドメインに設定
- 旧URL `yadori-llc.com/lodging-management.html` は本サブドメインへのリダイレクトページに置き換え済み
