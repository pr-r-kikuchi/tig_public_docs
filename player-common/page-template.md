---
title: ページテンプレート利用の埋め込みコード パラメータ
parent: 埋め込み連携（プレイヤー共通）
nav_order: 110
# 「画面から探す」索引用（screen-index.md が自動集約）。リンク名は実画面に準拠。
surface: Player
nav_section: プレイヤー共通
nav_label: ページテンプレート利用の埋め込みコード パラメータ
nav_path: 埋め込み連携（プレイヤー共通） ＞ ページテンプレート利用の埋め込みコード パラメータ
---

# ページテンプレート利用の埋め込みコード パラメータ
{: .no_toc }

「ページテンプレート」は、`<tig-tpl-live-01>` などの専用タグと `data-tig-tpl-*` 属性を書くだけで、動画の一覧ページを組み立てられる部品です。ここでは Tig LIVE のアーカイブ・ライブ向けの `<tig-tpl-live-01>` の属性を説明します。

## 目次
{: .no_toc .text-delta }

1. TOC
{:toc}

## 用意するもの

| 種別 | URL |
|---|---|
| CSS | `https://cdn.tigmedia.jp/tig-page-template/tig-template.css` |
| アーカイブ再生用のスクリプト | `https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js` |
| テンプレートのスクリプト | `https://cdn.tigmedia.jp/tig-page-template/tig-template.js` |

## window.tigConfig

テンプレートのスクリプトより前に、ページ内で `window.tigConfig` を定義します。

```html
<script>
  window.tigConfig = {
    // 接続用の設定（項目名と値は納品時にご案内します）
  }
</script>
```

| プロパティ | 必須 | 意味 |
|---|---|---|
| 接続用の設定 | ○ | 一覧を表示するための設定です。項目名と値は納品時にご案内しますので、そのとおりに入れてください |
| `shopify` | - | EC サービス連携のデータ。後述のコールバック関数の戻り値が自動で入るため、直接書く必要はありません |

## `<tig-tpl-live-01>` の属性

`data-*` 属性で設定します。

### コンテンツの絞り込み・並び順

| 属性 | 型 | 既定値 | 意味 |
|---|---|---|---|
| `data-tig-tpl-cid` | 文字列 | なし | コンテンツの識別子で絞り込みます |
| `data-tig-tpl-ctag` | 文字列 | なし | コンテンツタグで絞り込みます。`&&` で AND、`\|\|` で OR。空文字（`""`）は絞り込みなし |

グループの指定（`data-tig-tpl-gid`）と並び順（`data-tig-tpl-order`）は、導入時にご案内する値を指定してください。

### 表示レイアウト

| 属性 | 型 | 既定値 | 意味 |
|---|---|---|---|
| `data-tig-tpl-live-layout` | 文字列 | `portrait` | カードの向き。`portrait` は縦型、`landscape` は横型 |
| `data-tig-tpl-live-thumbnail-ratio` | 文字列 | なし（縦長 9:16） | `"2"` を指定するとサムネイルが 1:1 の正方形になります。それ以外は縦長（9:16）です |

### 表示要素の出し分け

値が `"false"` のとき非表示になります。それ以外（属性なしを含む）は表示されます。

| 属性 | 既定 | 対象 |
|---|---|---|
| `data-tig-tpl-live-display-logo` | 表示 | ロゴ画像 |
| `data-tig-tpl-live-display-status` | 表示 | 配信ステータスのアイコン（配信前・配信中・アーカイブ・終了） |
| `data-tig-tpl-live-display-daytime` | 表示 | 配信日時 |
| `data-tig-tpl-live-display-title` | 表示 | タイトル |
| `data-tig-tpl-live-display-tags` | 表示 | コンテンツタグ（最大 5 件） |

### 計測タグ

| 属性 | 型 | 既定値 | 意味 |
|---|---|---|---|
| `data-tig-tpl-ta-tag` | 文字列 | なし | アーカイブ公開済みのコンテンツを開いたときの計測タグ（`ta_tag`） |
| `data-tig-tpl-ta-tag-for-live` | 文字列 | なし | それ以外（配信前・配信中・終了済みでアーカイブ未公開）のコンテンツを開いたときの計測タグ |

ページ URL に `ta_tag` が付いている場合は、ページ URL 側が優先されます。

### EC サービス連携

| 属性 | 型 | 既定値 | 意味 |
|---|---|---|---|
| `data-tig-tpl-shopify-callback-func` | 文字列 | なし | EC サービスのデータを返す関数の名前（グローバル関数） |

## ページ URL パラメータの引き継ぎ

ページ内のテンプレートがすべて読み込まれたあと、**ページ URL のクエリパラメータ（`id` 以外）がすべてプレイヤーの URL に引き継がれます**。キーが重複した場合は、属性で指定した `ta_tag` よりページ URL が優先されます。

`ta_exid`・`ta_ex1`〜`ta_ex3` もこの経路で、コードの修正なしに計測ログへ残せます（[ta_exid（外部 ID 連携）]({{ '/player-common/exid-tracking.html' | relative_url }})）。

## コード例（通常版）

```html
<script>
  window.tigConfig = {
    // 接続用の設定（項目名と値は納品時にご案内します）
  }
</script>
<link rel="stylesheet" href="https://cdn.tigmedia.jp/tig-page-template/tig-template.css">
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
<script src="https://cdn.tigmedia.jp/tig-page-template/tig-template.js"></script>

<tig-tpl-live-01
  data-tig-tpl-live-layout="portrait"
  data-tig-tpl-live-thumbnail-ratio="2"
  data-tig-tpl-live-display-logo="false"
  data-tig-tpl-live-display-status="false"
  data-tig-tpl-live-display-daytime="false"
  data-tig-tpl-live-display-title="false"
  data-tig-tpl-live-display-tags="false"
  data-tig-tpl-ta-tag="modal_archive"
  data-tig-tpl-ta-tag-for-live="modal_live"
  data-tig-tpl-ctag=""
></tig-tpl-live-01>
```

## EC サービス連携

`data-tig-tpl-shopify-callback-func` に関数名を書き、その関数を定義するだけで動きます。テンプレート側のコード変更は不要です。

1. 専用タグが生成されるときに、属性で指定した関数が呼ばれます。
2. 戻り値が `window.tigConfig.shopify` に入ります（`window.tigConfig` を先に定義しておいてください）。
3. モーダルを開くときに、プレイヤーへ渡されます。

コールバック関数の戻り値は次のとおりです。

| プロパティ | 型 | 意味 |
|---|---|---|
| `storefront_access_token` | 文字列 | EC サービスの Storefront API のアクセストークン |
| `domain` | 文字列 | ストアのドメイン |

```html
<script>
  window.tigConfig = {
    // 接続用の設定（項目名と値は納品時にご案内します）
  }
  function onTigTplShopifyCallback() {
    return {
      storefront_access_token: "{storefront_access_token}",
      domain: "{store-domain}"
    }
  }
</script>
<link rel="stylesheet" href="https://cdn.tigmedia.jp/tig-page-template/tig-template.css">
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
<script src="https://cdn.tigmedia.jp/tig-page-template/tig-template.js"></script>

<tig-tpl-live-01
  data-tig-tpl-live-layout="portrait"
  data-tig-tpl-live-thumbnail-ratio="2"
  data-tig-tpl-ta-tag="modal_archive"
  data-tig-tpl-ta-tag-for-live="modal_live"
  data-tig-tpl-ctag=""
  data-tig-tpl-shopify-callback-func="onTigTplShopifyCallback"
></tig-tpl-live-01>
```

## 関連ページ

- [埋め込み連携の全体像]({{ '/player-common/embed-overview.html' | relative_url }})
- [LIVE の埋め込みスクリプト パラメータ]({{ '/player/tig-live/embed-params.html' | relative_url }})
- [ta_exid（外部 ID 連携）]({{ '/player-common/exid-tracking.html' | relative_url }})

## 用語

専門用語は[用語集]({{ '/glossary.html' | relative_url }})を参照してください。

---

最終更新: 2026-10-14 ／ 本ページは Tig プラットフォーム 顧客向け機能仕様の一部です。
