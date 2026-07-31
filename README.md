# REBUILD ／ 宿泊経営リビルド — LP

合同会社やどりの受託営業用ランディングページ。
稼働率が上がらない原因を「運営（ソフト）」か「建物（ハード）」かに切り分け、
診断 → 運営改善 → 施設改善 → 資金の4レイヤーを一本のプログラムとして提供する。

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
| 診断の内容・所要時間 | `#diagnosis` の `.diag__list` と `.diag__spec` |
| 施設改善のメニュー | `#renovation` の `.reno` |
| **建設業許可を取得したとき** | `#renovation` の `.reno__scope`「施工体制」、FAQ「内装工事はどこまで自社で対応するのですか？」、`.head` のリード文。**500万円未満の記述をすべて外し、許可番号を追記する** |
| 提携パートナーの社名を出すとき | `#finance` の `.fin__head` と `.fin__note`、`#program` の `.prog__aside`、FAQ「改装費用の資金調達も相談できますか？」 |

## 表記のルール（変更時に必ず守ること）

法務上の制約から、以下は勝手に強めないこと。

1. **内装工事** — 建設業許可が未取得のため、自社請負は請負代金500万円未満まで。それを超える規模は「設計・ディレクション・施工管理を当社が担い、施工は建設業許可を持つ協力会社に発注」と書く。「設計から施工まで自社で一貫」とは書かない。
2. **資金調達** — 定款第2条に金融・資金調達の記載がないため、やどりの事業としては書けない。「提携パートナーをご紹介します」という取次に限定する。「資金調達を支援します」「融資をアレンジします」は使わない。
3. **成果** — 「稼働率が上がります」「満室になります」と断定しない。「試算です」「改善余地をお伝えします」に留める。
4. **実績の帰属** — 設計・施工実績は代表が株式会社tokoroで手がけたもの。`#result` の注記を消さない。

設計の判断根拠と残論点は `~/dev/yadori-lodging-plan-design.md` にまとめてあります
（公開リポジトリには含めないでください）。

## 関連する変更（`yadori-hp` 側）

- 全ページのナビ／フッターに「**宿泊経営リビルド**」を追加し、リンク先を本サブドメインに設定
- 旧URL `yadori-llc.com/lodging-management.html` は本サブドメインへのリダイレクトページに置き換え済み

パッケージ名を変更する場合、コーポレート側8ファイル（`index.html` `about.html` `services.html`
`work-detail.html` `news.html` `privacy.html` `SiteNav.html` `SiteFooter.html`）の
アンカーラベルも合わせて差し替えてください。リンク先URLは変更不要です。
