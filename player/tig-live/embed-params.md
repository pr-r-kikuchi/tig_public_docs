---
title: LIVE の埋め込みスクリプト パラメータ
parent: 埋め込み連携（プレイヤー共通）
nav_order: 90
# 「画面から探す」索引用（screen-index.md が自動集約）。リンク名は実画面に準拠。
surface: Player
nav_section: プレイヤー共通
nav_label: LIVE の埋め込みスクリプト パラメータ
nav_path: 埋め込み連携（プレイヤー共通） ＞ LIVE の埋め込みスクリプト パラメータ
---

# LIVE の埋め込みスクリプト パラメータ
{: .no_toc }

Tig LIVE のアーカイブ動画・配信中のライブを自社サイトに埋め込む「埋め込みスクリプト」（`initTigArchiveEmbed`）で指定できるパラメータの一覧です。

## 目次
{: .no_toc .text-delta }

1. TOC
{:toc}

## 読み込みと呼び出し

```html
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
```

- 配列を渡すと、各要素を順に初期化します。
- ページの読み込みが終わる前に呼ばないよう、下のコード例のように `DOMContentLoaded` で包んでください。
- 表示方法の概要は[LIVE・アーカイブの埋め込み]({{ '/player/tig-live/ec-embed.html' | relative_url }})、フローティングの挙動は[LIVE・アーカイブのフローティング表示]({{ '/player/tig-live/floating.html' | relative_url }})をご覧ください。

## 表示モード（type）

| `type` | 表示モード | 説明の節 |
|---|---|---|
| 未指定 | フローティング表示 | [フローティング表示](#フローティング表示type-未指定) |
| `"embed"` | スライダー／タイルの一覧 | [一覧表示](#一覧表示type-embed) |
| `"direct"` | ページ内への直接埋め込み | [直接埋め込み](#直接埋め込みtype-direct) |
| `"fill"` | 全画面表示 | [全画面表示](#全画面表示type-fill) |
| `"custom"` | 取得済みのコンテンツデータを渡して表示 | [カスタム表示](#カスタム表示type-custom) |

## フローティング表示（type 未指定）

商品ページなどの左下に、サムネイル動画を小窓で表示します。クリックするとモーダルでプレイヤーが開きます。`siteItemCode` を指定しない場合は、`embedParam.target` で指定した場所にサムネイル（最新 1 件）を埋め込みます。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `groupId` | 文字列 | ○ | なし | コンテンツグループの ID。未指定だと何も表示されません |
| `siteItemCode` | 文字列／文字列の配列 | - | なし | EC の商品コード。サイトの URL などから取り出して渡すと、その商品に紐づくアーカイブを表示します |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL に付けるパラメータ（例: `{ ta_tag: "float_archive" }`）。値はそのまま連結されるため、必要に応じてエンコードしてください |
| `urlParamForLive` | オブジェクト | - | なし | 配信中のライブを開くときの URL パラメータ。未指定なら `urlParam` を使います |
| `isLiveAnnounce` | 真偽値 | - | false | 配信中に、アーカイブの代わりにライブの告知を表示します |
| `isLiveAnnounceWithNoArchive` | 真偽値 | - | false | 配信中は、アーカイブの有無にかかわらずライブの告知を表示します |
| `isFloatingLiveStreaming` | 真偽値 | - | false | 配信中のライブ映像をそのままフローティングで再生します。`isLiveAnnounce` より優先されます |
| `isAutoOpenModal` | 真偽値 | - | 未指定 | 表示と同時にモーダルを開きます。未指定のときは、ページ URL の `te_autoopen`（`true`／`1`／値なし）に従います |
| `isPip` | 真偽値 | - | false | モーダルを閉じたときに縮小して再生を続ける（PiP）ようにします |
| `pipCloseBehavior` | 文字列 | - | なし | `"hide"` を指定すると、PiP を閉じたときに非表示にします |
| `embedParam` | オブジェクト | - | なし | 埋め込みの挙動の設定（下表） |
| `callbacks` | オブジェクト | - | なし | 処理に差し込む関数（下表） |
| `ecCartUrl` | 文字列 | - | なし | EC のカート URL。指定するとモーダルにカートアイコンが表示され、クリックでこの URL に移動します |
| `start` | 数値 | - | なし | 再生の開始位置（秒） |
| `stockMemo` | オブジェクト | - | なし | お買い物メモの設定（[共通の設定](#共通の設定)） |

### embedParam（フローティング・サムネイル埋め込み）

| パラメータ | 型 | 既定値 | 意味 |
|---|---|---|---|
| `minimizeFloating` | 文字列 | `"minimize"` | `"close"` を指定すると、小窓の ✕ で小窓を消します。それ以外は最小化（アイコン化）します |
| `bufferTime` | 数値 | 72 | ライブ告知の猶予時間（時間）。`siteItemCode` なしのサムネイル埋め込みで、どの配信を出すかの判定に使います |
| `timeBasedOn` | 文字列 | `"latest"` | 猶予時間の基準。`"latest"` は最新配信の開始時刻の後、`"next"` は次回配信の開始時刻の前 |
| `embedStyle` | 文字列 | `"default"` | サムネイル埋め込み（`target` 指定）のみ有効。`"default"` はサムネイル画像、`"preview"` はプレビュー動画つき |
| `target` | 文字列 | なし | 埋め込み先の CSS セレクタ。`siteItemCode` を指定しないサムネイル埋め込みでは必須です |

### callbacks

| コールバック | 呼ばれるタイミング |
|---|---|
| `onShow` | フローティングが表示された直後 |
| `onMinimize` | 小窓が最小化されたとき |
| `onRestore` | 最小化から戻ったとき |
| `onJudgeLiveAnnounce` | ライブ告知を出す直前。`false` を返すと告知をやめます |

### 呼び出せる関数

| 関数 | 説明 |
|---|---|
| `window.tigEmbedFloatOpenModal(開始秒)` | 小窓からモーダルを開きます。開始秒を渡すとその位置から再生します |
| `window.tigCloseArchiveFloat()` | 小窓を閉じます |

### コード例

`{gid}` はご利用のグループの ID に置き換えてください。

```html
<style>
  .tig-archive-embed {
    width: 16.2vh;
    left: 2vw;
    bottom: 3vw;
    height: 28.8vh;
    z-index: 2147483630;
  }
  .tig-modal.tig-open.tig-modal-pip-enable.tig-modal-pip {
    width: 16.2vh;
    left: 2vw;
    bottom: 3vw;
    height: 28.8vh;
    z-index: 2147483630;
  }
  .tig-iframe-area {
    z-index: 2147483638;
  }
</style>
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
<script>
(function () {
  function processEmbedInitializeFunc() {
    // 商品コード取得（サイトに合わせてカスタマイズしてください）
    var pathSegments = location.pathname.split('/');
    var index = pathSegments.indexOf('item');
    var siteItemCode = (index !== -1 && index < pathSegments.length - 1) ? pathSegments[index + 1] : '';

    var urlParam = { ta_tag: "float_archive" };
    var urlParamForLive = { ta_tag: "float_live" };

    // 埋め込み元サイトの URL パラメータをプレイヤー側へ引き継ぎ
    var ignoreParams = ['id'];
    if (window.location.search.length > 0) {
      var pairs = window.location.search.slice(1).split('&');
      for (var i = 0; i < pairs.length; i++) {
        var kv = pairs[i].split('=');
        if (ignoreParams.indexOf(kv[0]) === -1) {
          urlParam[kv[0]] = kv[1];
          urlParamForLive[kv[0]] = kv[1];
        }
      }
    }

    initTigArchiveEmbed({
      groupId: "{gid}",
      siteItemCode: siteItemCode,
      urlParam: urlParam,
      urlParamForLive: urlParamForLive,
      isFloatingLiveStreaming: true,
      isLiveAnnounce: true,
      isLiveAnnounceWithNoArchive: true,
      isPip: true,
      embedParam: {
        minimizeFloating: "minimize"
      },
      stockMemo: {
        isShow: false
      }
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processEmbedInitializeFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function() {
      processEmbedInitializeFunc();
    });
  }
})();
</script>
```

## 一覧表示（type: "embed"）

ページ内の指定した場所に、アーカイブの一覧をスライダーまたはタイルで表示します。クリックするとモーダルでプレイヤーが開きます。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"embed"` |
| `target` | 文字列 | ○ | なし | 埋め込み先の CSS セレクタ（例: `"#tig-archive-frame"`）。未指定、または該当する要素がないと何も表示されません |
| `groupId` | 文字列 | ○ | なし | コンテンツグループの ID |
| `isPip` | 真偽値 | - | false | モーダルの PiP を有効にします |
| `pipCloseBehavior` | 文字列 | - | なし | PiP を閉じたときの挙動（フローティング表示と同じ） |
| `urlParam` | オブジェクト | - | なし | モーダルでプレイヤーを開くときの URL パラメータ |
| `embedParam` | オブジェクト | - | なし | 下表 |

この形式では `stockMemo`・`callbacks`・`urlParamForLive` は使われません。

### embedParam（一覧表示）

| パラメータ | 型 | 既定値 | 意味 |
|---|---|---|---|
| `type` | 文字列 | `"slider"` | `"slider"` は横スクロール、`"tile"` はタイル状。ほかの値はスライダーになります |
| `limit` | 数値 | 10 | 表示するアーカイブの件数。数値でない場合は 10 |
| `frameFit` | 真偽値 | false | 一覧の高さに合わせて表示枠の高さを自動調整します（スクロールバーなしで表示できます） |
| `styleVars` | オブジェクト | なし | CSS カスタムプロパティで見た目を変えます（下表） |

### styleVars

キーは `--tig-embed-` で始まる名前です（前後に空白を入れないでください）。

| プロパティ | 意味 |
|---|---|
| `--tig-embed-theme-color` | タイル部分の背景色 |
| `--tig-embed-margin-right` | タイル間の右マージン（PC） |
| `--tig-embed-margin-right-sp` | タイル間の右マージン（スマートフォン） |
| `--tig-embed-img-background-color` | サムネイルの背景色 |
| `--tig-embed-tile-margin-bottom` | タイル間の下マージン |
| `--tig-embed-title-font-family` | 動画タイトルのフォント |
| `--tig-embed-title-font-size` | 動画タイトルのフォントサイズ |
| `--tig-embed-title-color` | 動画タイトルの文字色 |
| `--tig-embed-title-line-height` | 動画タイトルの行の高さ |
| `--tig-embed-title-height` | 動画タイトルの高さ（`frameFit` 使用時は微調整向け。大きくするとスクロールバーが出ます） |
| `--tig-embed-time-font-family` | ライブ実施時刻のフォント |
| `--tig-embed-time-font-size` | ライブ実施時刻のフォントサイズ |
| `--tig-embed-time-color` | ライブ実施時刻の文字色 |

値に `;` や `+` を含めることはできません。`rgba(0, 0, 0, 0.5)` のようにカンマや空白を含む値は使えます。

### コード例（タイル表示 + frameFit）

```html
<div id="tig-archive-frame"></div>
<style>
  #tig-archive-frame {
    width: 50%;
    margin: auto;
  }
  @media screen and (max-width: 767px) {
    #tig-archive-frame iframe {
      height: 300px;
    }
  }
</style>
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
<script>
(function () {
  function processTigArchiveFrameFunc() {
    var urlParam = { ta_tag: "tile_modal" };

    // 埋め込み元サイトの URL パラメータをプレイヤー側へ引き継ぎ
    var ignoreParams = ['id', 'autoplay'];
    if (window.location.search.length > 0) {
      var pairs = window.location.search.slice(1).split('&');
      for (var i = 0; i < pairs.length; i++) {
        var kv = pairs[i].split('=');
        if (ignoreParams.indexOf(kv[0]) === -1) {
          urlParam[kv[0]] = kv[1];
        }
      }
    }

    initTigArchiveEmbed({
      type: "embed",
      target: "#tig-archive-frame",
      groupId: "{gid}",
      urlParam: urlParam,
      embedParam: {
        type: "tile",
        limit: 20,
        frameFit: true,
        styleVars: {
          "--tig-embed-theme-color": "#fff",
          "--tig-embed-margin-right": "16px",
          "--tig-embed-margin-right-sp": "8px",
          "--tig-embed-img-background-color": "#000",
          "--tig-embed-tile-margin-bottom": "86px",
          "--tig-embed-title-font-family": "sans-serif",
          "--tig-embed-title-font-size": "16px",
          "--tig-embed-title-color": "#000",
          "--tig-embed-title-line-height": "24px",
          "--tig-embed-title-height": "48px",
          "--tig-embed-time-font-family": "sans-serif",
          "--tig-embed-time-font-size": "16px",
          "--tig-embed-time-color": "#666"
        }
      }
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processTigArchiveFrameFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function() {
      processTigArchiveFrameFunc();
    });
  }
})();
</script>
```

## 直接埋め込み（type: "direct"）

商品コード（`siteItemCode`）で決まるアーカイブ（または配信中のライブ）を、フローティングではなくページ内の指定した場所に直接埋め込みます。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"direct"` |
| `groupId`・`siteItemCode`・`urlParam`・`urlParamForLive`・`isLiveAnnounce` など | - | - | - | フローティング表示と同じ |
| `embedParam.target` | 文字列 | ○ | なし | 埋め込み先の CSS セレクタ。ないと何も表示されません |
| `embedParam.pipPos` | 文字列 | - | `"left"` | スクロールで画面外に出たときの追従位置。`"left"` か `"right"` |
| `embedParam.useCloseBtn` | 真偽値 | - | true | 追従中の閉じるボタンを表示します |
| `embedParam.isShowPiPImmediately` | 真偽値 | - | false | スクロールを待たずに追従表示を始めます |
| `isPip` | 真偽値 | - | false | スクロール追従（PiP）を有効にします |

## 全画面表示（type: "fill"）

ページ全体をプレイヤーで埋めます。キャンペーンページや専用 LP に向いています。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"fill"` |
| `groupId` | 文字列 | ○ | なし | コンテンツグループの ID |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ。`id`・`gid`・`tig_id` のキーは無視されます |

表示するコンテンツは、埋め込み元ページの URL パラメータで決まります。`tig_id=` を見て、なければ `id=` を見ます。どちらもないと何も表示されません。

```html
<script src="https://cdn.tigmedia.jp/v3/latest/js/tig-archive-embed.js"></script>
<script>
(function () {
  function processTigFillFunc() {
    initTigArchiveEmbed({
      type: "fill",
      groupId: "{gid}",
      urlParam: {
        ta_tag: "fill_player"
      }
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processTigFillFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function() {
      processTigFillFunc();
    });
  }
})();
</script>
```

## カスタム表示（type: "custom"）

あらかじめ取得したコンテンツのデータを直接渡して表示します。`detail` で表示形式を選びます。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"custom"` |
| `detail` | 文字列 | ○ | なし | `"float"`／`"direct"`／`"thumbnail"` |
| `contentData` | オブジェクト | ○ | なし | コンテンツ 1 件分のデータ |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ |
| `urlParamForLive` | オブジェクト | - | なし | 配信中のときの URL パラメータ |
| `embedParam` | オブジェクト | - | なし | `target`（CSS セレクタ）を指定します。`direct`・`thumbnail` では必須です |
| `stockMemo`・`isPip`・`callbacks` ほか | - | - | - | フローティング表示と同じ |

| `detail` | 意味 |
|---|---|
| `"float"` | フローティング表示。プレビュー動画とサムネイルをコンテンツデータから表示します |
| `"direct"` | 指定した場所に動画を直接埋め込みます |
| `"thumbnail"` | 指定した場所にサムネイルを表示し、クリックでモーダルを開きます |

## 共通の設定

### urlParam

プレイヤーの URL の末尾に `&キー=値` として連結されます。任意のキーを渡せます。

| キー | 意味 |
|---|---|
| `ta_tag` | 流入経路を区別する計測タグ（例: `"float_live"`, `"tile_modal"`） |
| `ta_exid`・`ta_ex1`〜`ta_ex3` | 自社の識別値を計測ログに残します（[ta_exid（外部 ID 連携）]({{ '/player-common/exid-tracking.html' | relative_url }})） |
| その他 | そのままプレイヤーへ渡されます |

埋め込み元ページの URL パラメータを引き継ぐ書き方は、フローティング表示のコード例のとおりです。`ignoreParams` に挙げたキーは引き継ぎません。

### stockMemo

フローティング表示・カスタム表示（`float`）で、お買い物メモのアイコンを表示します。

| パラメータ | 型 | 意味 |
|---|---|---|
| `isShow` | 真偽値 | お買い物メモのアイコンの表示・非表示 |
| `title` | 文字列 | ストックの表示に付けるタイトル |
| `showEvenZero` | 真偽値 | ストックが 0 件でもアイコンを表示します |

### EC サービスとの連携（shopify）

EC サービスと連携する場合は、`shopify` パラメータに導入時にご案内する値を指定します。フローティング表示・一覧表示・全画面表示・カスタム表示で指定でき、指定した値はプレイヤーにそのまま渡されます。

## 埋め込み元ページへ通知されるイベント

次の `type` のメッセージ（`window.postMessage`）が埋め込み元ページに届きます。Google タグマネージャーなどで受け取れます。

| `type` | タイミング |
|---|---|
| `TigEmbed#event#onFloatingView` | フローティングが表示されたとき |
| `TigEmbed#event#onFloatingClose` | フローティングが閉じられたとき |
| `TigEmbed#event#onVideoStart` | 動画の再生が始まったとき |
| `TigEmbed#event#onModalClose` | モーダルが閉じられたとき |
| `TigEmbed#event#onReady` | サムネイル埋め込みで、配信状態が判明したとき |

## 関連ページ

- [LIVE・アーカイブの埋め込み]({{ '/player/tig-live/ec-embed.html' | relative_url }})
- [Short の埋め込みスクリプト パラメータ]({{ '/player/tig-short/embed-params.html' | relative_url }})
- [埋め込み連携の全体像]({{ '/player-common/embed-overview.html' | relative_url }})

## 用語

専門用語は[用語集]({{ '/glossary.html' | relative_url }})を参照してください。

---

最終更新: 2026-10-14 ／ 本ページは Tig プラットフォーム 顧客向け機能仕様の一部です。
