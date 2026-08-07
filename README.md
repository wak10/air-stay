# AirStay — LP

東京の民泊・簡易宿所に特化した**運営代行**のランディングページ。
運営会社は合同会社やどり（フッターにのみ記載）。

## 提供内容

本編は**運営代行のみ**。3プラン（ライト10% / ミドル16% / フル20%）。
無料の稼働率診断が入口。

内装改装・開業支援・駆けつけ・資金調達は**オプション扱い**で、
`#scope` の「オプションで選べること」に列挙するに留める（個別見積）。
「なんでもできます」に見せないため、同じセクションに
「やらないこと」と「向いていないケース」を必ず併記すること。

## ブランド

### ロゴ

支給された `AirStayロゴ 1.png`（1254×1254）をWeb用に加工したPNG5種を使用。

| ファイル | 用途 | 実寸 | 表示 |
|---|---|---|---|
| `airstay-logo-h.png` | ナビ（デスクトップ） | 897×120 | 30px高（4.0x） |
| `airstay-logo.png` | フッター（縦組み） | 448×240 | 74px高（3.2x） |
| `airstay-mark.png` | ナビ（モバイル） | 261×104 | 26px高（4.0x） |
| `airstay-favicon-32.png` | ファビコン | 32×32 | — |
| `airstay-favicon-180.png` | Apple touch icon | 180×180 | — |

**加工内容**（原本をそのまま使うと破綻します）
- 余白をトリミング（原本は1254px角に対しロゴは中央の一部のみ）
- 白背景を透過。ただし原本の背景は純白でなく `#FDFDFD` のため、
  単純な輝度反転だと**全面にアルファ2/255の黒いベールが残る**。
  輝度240以上を完全透明にするレベル補正で除去（透明率 2.8% → 88%）
- 「A」「a」のカウンター（穴）も同じ処理で透明化。ダーク背景で穴が背景色に追従
- 縦組みのままではナビでワードマークが11px程度になり読めないため、
  マークとワードマークを分解して**横組みに再構成**（7.5:1）

**色の変え方**：インクは黒。白抜きは CSS の `filter: brightness(0) invert(1)`
で行う（フッターがこの方式）。穴は透明なので背景色に追従します。

**作り直すとき**は上記のレベル補正（しきい値240）を必ず入れること。

- やどりの立ち位置は**運営会社**。フッターにのみ記載

### カラー

Airbnb の現行ブランドカラー `#FF385C`（Rausch）とは**色相を意図的にずらしている**。
Air- 接頭の名称と組み合わせると出所混同のリスクが上がるため、オレンジ寄りのコーラルを採用。

| 変数 | 値 | 用途 | 白背景コントラスト |
|---|---|---|---|
| `--coral` | `#C9421C` | 小さい文字・アイコン・ボーダー | 4.91:1 ✓ AA |
| `--coral-vivid` | `#F4552B` | 装飾・グラデーション中間 | 装飾のみ |
| `--coral-lite` | `#FF8A4C` | グラデーション始点・暗背景上 | 装飾のみ |
| `--coral-dark` | `#A0330F` | hover | 7:1超 |
| `--grad` | `#FF8A4C → #F4552B → #DC4520` | ワードマーク・CTA・見出し強調 | — |
| `--grad-btn` | `#E84A20 → #C03A12` | ボタン・バッジ・番号 | 白文字で4.0〜5.5:1 |

**小さい文字に `--coral-vivid` 以降の明るい色を使わないこと**（AA を割ります）。

- 公開URL: **https://airstay.yadori-llc.com/**
- リポジトリ: `wak10/air-stay`
- ホスティング: GitHub Pages（このリポジトリ単独）
- コーポレートサイト: https://yadori-llc.com （別リポジトリ `yadori-hp`）

## 構成

```
index.html              LP本体（CSS/JSインライン、単一ファイル完結）
CNAME                   airstay.yadori-llc.com
.nojekyll               Jekyll処理を無効化
favicon.png
assets/
  brand/airstay-*.svg   ロゴ4種（上記「ロゴ」参照）
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

### 1. push

```bash
cd ~/dev/yadori-management-lp
# リポジトリは wak10/air-stay として作成済み
git push -u origin main
```

### 2. GitHub Pages を有効化

リポジトリの Settings → Pages で以下を設定します。

- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`
- Custom domain: `airstay.yadori-llc.com`
- Enforce HTTPS: 有効（証明書の発行まで数分〜1時間程度かかります）

### 3. DNS にCNAMEレコードを追加

`yadori-llc.com` を管理しているDNSに、次の1レコードを追加します。

| Type | Name | Value | TTL |
|---|---|---|---|
| CNAME | `airstay` | `wak10.github.io.` | 3600（自動） |

> apexドメイン（`yadori-llc.com`）側のAレコードは変更しないでください。コーポレートサイトが停止します。

反映確認:

```bash
dig +short airstay.yadori-llc.com
curl -sI https://airstay.yadori-llc.com/ | head -1
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
| OTA手数料率 | JS内 `OTA_RATE = 0.155`（FAQの試算例と分岐点も要連動） |
| 駆けつけの提供状況 | 比較表「現地駆けつけ対応」の行、`#scope` のオプション |
| 実績数値 | `#result` セクション |
| 診断の内容・所要時間 | `#diagnosis` の `.diag__list` と `.diag__spec` |
| オプションの内容 | `#scope` の `.scope__opt` |
| やらないこと | `#scope` の `.scope__box--cant` |
| 実績ギャラリー | `#works` の `.wgal` と `.wsum` |
| **建設業許可を取得したとき** | `#scope` のオプション「内装改装・インテリア」。500万円未満の制約が外れるので、施設改善を本編セクションに昇格させる判断も可 |
| 提携パートナーの社名を出すとき | `#scope` のオプション「資金調達のご相談」 |

## 表記のルール（変更時に必ず守ること）

法務上の制約から、以下は勝手に強めないこと。

1. **内装工事** — 建設業許可が未取得のため、自社請負は請負代金500万円未満まで。それを超える規模は「設計・ディレクション・施工管理を当社が担い、施工は建設業許可を持つ協力会社に発注」と書く。「設計から施工まで自社で一貫」とは書かない。
2. **資金調達** — 定款第2条に金融・資金調達の記載がないため、やどりの事業としては書けない。「提携パートナーをご紹介します」という取次に限定する。「資金調達を支援します」「融資をアレンジします」は使わない。
3. **成果** — 「稼働率が上がります」「満室になります」と断定しない。「試算です」「改善余地をお伝えします」に留める。
4. **実績の帰属** — 実績は代表が株式会社tokoroで手がけた案件。社名は非表示にしたが、
   商談で問われた際に説明できる状態にしておくこと（`~/dev/yadori-lodging-plan-design.md` 残論点1）。

設計の判断根拠と残論点は `~/dev/yadori-lodging-plan-design.md` にまとめてあります
（公開リポジトリには含めないでください）。

## 関連する変更（`yadori-hp` 側）

- 全ページのナビ／フッターに「**AirStay**」を追加し、リンク先を本サブドメインに設定
- 旧URL `yadori-llc.com/lodging-management.html` は本サブドメインへのリダイレクトページに置き換え済み

ブランド名を変更する場合、コーポレート側8ファイル（`index.html` `about.html` `services.html`
`work-detail.html` `news.html` `privacy.html` `SiteNav.html` `SiteFooter.html`）の
アンカーラベルも合わせて差し替えてください。リンク先URLは変更不要です。
