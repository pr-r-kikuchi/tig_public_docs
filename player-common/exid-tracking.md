---
title: ta_exid（外部 ID 連携）
parent: 埋め込み連携（プレイヤー共通）
nav_order: 120
# 「画面から探す」索引用（screen-index.md が自動集約）。リンク名は実画面に準拠。
surface: Player
nav_section: プレイヤー共通
nav_label: ta_exid（外部 ID 連携）
nav_path: 埋め込み連携（プレイヤー共通） ＞ ta_exid（外部 ID 連携）
---

# ta_exid（外部 ID 連携）
{: .no_toc }

自社が持つ識別情報（会員 ID のハッシュ値、キャンペーン名など）を、Tig プレイヤーの操作ログに紐づける仕組み「外部 ID 連携」の説明です。

## 目次
{: .no_toc .text-delta }

1. TOC
{:toc}

## ta_exid とは

ta_exid は、Tig の動画プレイヤーの操作ログに「誰が見たか」「どの経路から来たか」といった外部の識別情報を紐づける仕組みです。

プレイヤーの URL に `ta_exid`・`ta_ex1`・`ta_ex2`・`ta_ex3` というパラメータを付けると、動画の再生・停止・アイテムのタップなど、すべての操作ログに指定した値が自動で記録されます。使える識別値は、この 4 つです。

## 埋め込み形式ごとにできること

### LIVE の埋め込み（アーカイブ・ライブ）

- アーカイブ動画の視聴ログに、会員 ID やキャンペーンコードなどの外部情報を紐づけられます。
- ライブ配信とアーカイブ配信で、別々の識別値を設定できます。例: ライブ視聴時は「live_view」、アーカイブ視聴時は「archive_view」と記録して区別する。
- 動画グループ内で複数のコンテンツを切り替えても、すべての操作ログに同じ識別値が記録されます。

| やりたいこと | ta_exid に設定する値の例 |
|---|---|
| 会員ごとの視聴行動を分析したい | 会員 ID のハッシュ値 |
| メルマガ経由の視聴を把握したい | `mail_202404` |
| ライブとアーカイブの視聴者を比較したい | ライブ: `live_member123` ／ アーカイブ: `archive_member123` |

設定には、埋め込みコード（JavaScript）の初期化オプションに `urlParam`（ライブ用は `urlParamForLive`）として値を追加します。エンジニアによるコードの修正が必要です。

```html
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
<script>
  initTigArchiveEmbed({
    groupId: '{gid}',
    urlParam: {
      ta_exid: encodeURIComponent('member_123'),
      ta_ex1:  encodeURIComponent('archive_view')
    },
    urlParamForLive: {
      ta_exid: encodeURIComponent('member_123'),
      ta_ex1:  encodeURIComponent('live_view')
    }
  });
</script>
```

アーカイブの視聴は `urlParam`、ライブ配信中は `urlParamForLive` が使われます。両方を計測したいときは、両方に指定してください（`urlParamForLive` を省略すると `urlParam` が使われます）。

### Short の埋め込み

- Tig Short の動画の視聴ログに、外部の識別情報を紐づけられます。
- コンテンツ一覧から別の動画に切り替えても、識別値は引き継がれます。複数の動画をまたいだ視聴行動を追跡できます。
- `ta_exid` と `ta_ex1`〜`ta_ex3` の計 4 つを同時に設定できます。例: `ta_exid` に会員 ID、`ta_ex1` にキャンペーン名、`ta_ex2` にユーザー属性。

| やりたいこと | 設定する値の例 |
|---|---|
| 会員ごとにどの動画を見たか追跡したい | ta_exid: 会員 ID のハッシュ値 |
| キャンペーンごとの効果を測りたい | ta_ex1: `campaign_spring` |
| 会員ランク別の視聴傾向を見たい | ta_exid: 会員 ID ／ ta_ex2: `premium` |

設定には、埋め込みコード（JavaScript）の初期化オプションに `urlParam` として値を追加します。エンジニアによるコードの修正が必要です。

```html
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
<script>
  initTigCreatorEmbed({
    type: "float",
    groupId: "{gid}",
    urlParam: {
      ta_exid: encodeURIComponent('member_123'),
      ta_ex1:  encodeURIComponent('campA')
    }
  });
</script>
```

パラメータの詳細は[Short の埋め込みスクリプト パラメータ]({{ '/player/tig-short/embed-params.html' | relative_url }})をご覧ください。

### ページテンプレート

- ページテンプレート上の動画視聴ログに、外部の識別情報を紐づけられます。
- コードの修正は不要です。ページの URL の末尾にパラメータを足すだけで動きます。
- URL に含まれたパラメータは、テンプレートの中で表示されるプレイヤーへ自動で渡されます。

| やりたいこと | URL の作り方 |
|---|---|
| メール配信で誰が見たか把握したい | `https://〜/page.html?ta_exid=member_123` |
| キャンペーンごとの流入を分けたい | `https://〜/page.html?ta_exid=campaign_spring` |
| 店舗ごとの QR コードの効果を測りたい | `https://〜/page.html?ta_exid=store_shibuya` |
| 複数の軸で分析したい | `https://〜/page.html?ta_exid=member_123&ta_ex1=campaign_spring` |

エンジニアの作業は不要です。次のいずれかの方法で、URL にパラメータを足します。

- メール配信ツールでリンクの URL を編集する
- バナーのリンク先 URL を編集する
- QR コードを作るときに、パラメータ付きの URL を指定する
- MA ツールや CDP で URL パラメータを動的に付ける

ページテンプレートの詳細は[ページテンプレート利用の埋め込みコード パラメータ]({{ '/player-common/page-template.html' | relative_url }})をご覧ください。

## 3 つの形式の比較

| 項目 | LIVE の埋め込み | Short の埋め込み | ページテンプレート |
|---|---|---|---|
| 主な用途 | アーカイブ・ライブ配信 | Tig Short の動画 | ページテンプレート |
| 設定方法 | JavaScript で `urlParam` を指定 | JavaScript で `urlParam` を指定 | URL にパラメータを足すだけ |
| エンジニアの作業 | 必要 | 必要 | 不要 |
| ライブとアーカイブで別の値 | 設定できる | - | - |
| 使える識別値の数 | 最大 4 つ（exid、ex1〜ex3） | 最大 4 つ | 最大 4 つ |

## 共通の注意事項

- **個人情報は入れないでください。** メールアドレスや ID をそのまま設定せず、ハッシュ化・匿名化した値を使ってください。
- **日本語・`&`・`=`・空白を含む値は、`encodeURIComponent` でエンコードしてください。** `urlParam` の値はそのまま URL に連結されます。
- **値は最初に決めてください。** ページを読み込んだときに 1 回だけ読み取られるため、途中で値を変えることはできません。
- **確認方法:** Tig LIVE の CSV ダウンロードでは、`exid`・`ex1`・`ex2`・`ex3` の列に記録されます。

## 用語

専門用語は[用語集]({{ '/glossary.html' | relative_url }})を参照してください。

---

最終更新: 2026-10-14 ／ 本ページは Tig プラットフォーム 顧客向け機能仕様の一部です。
