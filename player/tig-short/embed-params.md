---
title: Short の埋め込みスクリプト パラメータ
parent: 埋め込み連携（プレイヤー共通）
nav_order: 100
# 「画面から探す」索引用（screen-index.md が自動集約）。リンク名は実画面に準拠。
surface: Player
nav_section: プレイヤー共通
nav_label: Short の埋め込みスクリプト パラメータ
nav_path: 埋め込み連携（プレイヤー共通） ＞ Short の埋め込みスクリプト パラメータ
---

# Short の埋め込みスクリプト パラメータ
{: .no_toc }

Tig Short のコンテンツを自社サイトに埋め込む「埋め込みスクリプト」（`initTigCreatorEmbed`）で指定できるパラメータの一覧です。

## 目次
{: .no_toc .text-delta }

1. TOC
{:toc}

## 読み込みと呼び出し

```html
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
```

`type="module"` を付けても読み込めます。

- 配列を渡すと、各要素を順に初期化します。
- `type` を省略した場合、または下表以外の値を渡した場合は `"embed"`（一覧表示）として扱います。
- 埋め込みの概要は[Short の埋め込み・連携API]({{ '/player/tig-short/embed.html' | relative_url }})、フローティングの挙動は[Short のフローティング表示]({{ '/player/tig-short/floating.html' | relative_url }})をご覧ください。

## 表示モード（type）

| `type` | 用途 | 説明の節 |
|---|---|---|
| `"embed"`（既定） | スライダー／タイルの一覧 | [一覧表示](#一覧表示type-embed) |
| `"float"` | フローティング表示 | [フローティング表示](#フローティング表示type-float) |
| `"direct"` | 指定した場所にプレイヤーを直接埋め込み | [ダイレクト埋め込み](#ダイレクト埋め込みtype-direct) |
| `"fill"` | 全画面表示 | [全画面表示](#全画面表示type-fill) |
| `"custom"` | コンテンツ一覧を直接渡し、返されるコントローラーで操作 | [カスタム表示](#カスタム表示type-custom) |
| `"customwapi"` | コンテンツを自動で取得し、コントローラーで操作 | [カスタム連携表示](#カスタム連携表示type-customwapi) |

## 一覧表示（type: "embed"）

ページ内の指定した場所にスライダーまたはタイルで一覧を表示します。クリックするとモーダルでプレイヤーが開きます。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | - | `"embed"` | 省略できます |
| `target` | 文字列 | ○ | なし | 埋め込み先の CSS セレクタ（例: `"#tig-creator-frame"`） |
| `groupId` | 文字列 | △ | なし | コンテンツグループの ID。`groupId` か `collectionId` のどちらかは必須です |
| `collectionId` | 文字列 | △ | なし | コレクションの ID |
| `contentsId` | 文字列／文字列の配列 | - | なし | 特定のコンテンツの ID |
| `siteItemCode` | 文字列／文字列の配列 | - | なし | EC の商品コード。その商品に紐づくコンテンツに絞ります（`groupId` を指定したときのみ有効） |
| `order` | 文字列 | - | 下記 | 並び順（[order](#order)） |
| `limit` | 数値 | - | 10（コレクション指定時は 20） | 取得件数。最大 50 件です |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ |
| `embedParam` | オブジェクト | - | なし | 下表 |
| `listeners` | オブジェクト | - | なし | イベントリスナー（[listeners](#listeners)） |
| `isAutoOpenModal` | 真偽値 | - | 未指定 | 表示と同時にモーダルを開きます。未指定のときは、ページ URL の `te_autoopen` に従います |

`contentsId`・`collectionId`・`groupId` を複数渡した場合は、`contentsId`、`collectionId`、`groupId` の順で優先されます。ただし、`groupId` も `collectionId` も指定せず `contentsId` だけを渡した場合は、何も表示されません。

### embedParam（一覧表示）

| パラメータ | 型 | 既定値 | 意味 |
|---|---|---|---|
| `type` | 文字列 | `"slider"` | `"slider"` は横スクロール、`"tile"` はタイル状 |
| `frameFit` | 真偽値 | false | 一覧の高さに合わせて表示枠の高さを自動調整します |
| `styleVars` | オブジェクト | なし | CSS カスタムプロパティで見た目を変えます（下表） |

### styleVars（Short の一覧で効くもの）

キーの前後に空白を入れないでください。

| プロパティ | 意味 |
|---|---|
| `--tig-embed-theme-color` | 一覧の背景色 |
| `--tig-embed-margin-right` | アイテム間の余白（PC） |
| `--tig-embed-margin-right-sp` | アイテム間の余白（スマートフォン） |
| `--tig-embed-img-background-color` | 動画の余白部分（レターボックス）の色 |

値に `,` や `+` を含めることはできません（`rgba(0,0,0,0.5)` や、カンマ区切りのフォント指定は使えません）。

### コード例

```html
<div id="tig-creator-frame"></div>
<style>
  #tig-creator-frame iframe {
    width: 100%;
    height: 500px;
  }
  @media screen and (max-width: 767px) {
    #tig-creator-frame iframe {
      height: 300px;
    }
  }
</style>
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
<script>
(function () {
  function processTigCreatorEmbedFunc() {
    var urlParam = { ta_tag: "ec_embed_creator" };
    var ignoreParams = ['id'];
    if (window.location.search.length > 0) {
      var pairs = window.location.search.slice(1).split('&');
      for (var i = 0; i < pairs.length; i++) {
        var kv = pairs[i].split('=');
        if (ignoreParams.indexOf(kv[0]) === -1) {
          urlParam[kv[0]] = kv[1];
        }
      }
    }

    initTigCreatorEmbed({
      type: "embed",
      target: "#tig-creator-frame",
      collectionId: "{collection_id}",
      urlParam: urlParam,
      embedParam: {
        type: "slider",
        styleVars: {
          "--tig-embed-theme-color": "#fff",
          "--tig-embed-margin-right": "16px",
          "--tig-embed-margin-right-sp": "8px",
          "--tig-embed-img-background-color": "#000"
        }
      }
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processTigCreatorEmbedFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function () {
      processTigCreatorEmbedFunc();
    });
  }
})();
</script>
```

## フローティング表示（type: "float"）

商品ページなどにサムネイル動画を小窓で表示し、クリックするとモーダルでプレイヤーが開きます。同じページに 2 つ目の `float` を呼んでも無視されます（先に表示したものが優先されます）。小窓の ✕ は常に小窓を閉じる動作で、最小化（アイコン化）は選べません（`embedParam.minimizeFloating` は使われません）。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"float"` |
| `groupId` | 文字列 | △ | なし | `groupId` か `collectionId` のどちらかは必須です |
| `collectionId` | 文字列 | △ | なし | コレクションの ID |
| `contentsId` | 文字列／文字列の配列 | - | なし | 特定のコンテンツの ID |
| `siteItemCode` | 文字列／文字列の配列 | - | なし | EC の商品コード（`groupId` を指定したときのみ有効） |
| `order` | 文字列 | - | 下記 | 並び順（[order](#order)） |
| `limit` | 数値 | - | 10（コレクション指定時は 20） | 取得件数（最大 50） |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ |
| `isAutoOpenModal` | 真偽値 | - | 未指定 | 表示と同時にモーダルを開きます |
| `stockMemo` | オブジェクト | - | なし | お買い物メモの設定（[stockMemo](#stockmemo)） |
| `listeners` | オブジェクト | - | なし | イベントリスナー（[listeners](#listeners)） |

### 呼び出せる関数

| 関数 | 説明 |
|---|---|
| `window.disposeTigCreatorEmbedFloating()` | プログラムからフローティングを破棄します（SPA サイトの画面遷移用） |

### コード例

```html
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
<script>
(function () {
  function processTigCreatorEmbedFloatFunc() {
    var siteItemCode = location.pathname.split('/').reverse().find(function (elem) {
      if (elem != '') return elem;
    });
    if (!siteItemCode) return;

    var urlParam = { ta_tag: "ec_float_creator" };
    var ignoreParams = ['id'];
    if (window.location.search.length > 0) {
      var pairs = window.location.search.slice(1).split('&');
      for (var i = 0; i < pairs.length; i++) {
        var kv = pairs[i].split('=');
        if (ignoreParams.indexOf(kv[0]) === -1) {
          urlParam[kv[0]] = kv[1];
        }
      }
    }

    initTigCreatorEmbed({
      type: "float",
      groupId: "{gid}",
      siteItemCode: siteItemCode,
      urlParam: urlParam
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processTigCreatorEmbedFloatFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function() {
      processTigCreatorEmbedFloatFunc();
    });
  }
})();
</script>
<style>
  .tig-creator-embed {
    width: 16.2vh;
    left: 2vw;
    bottom: 3vw;
    height: 28.8vh;
    z-index: 2147483630;
  }
  .tig-creator-embed .preview-video {
    width: 16.2vh;
    height: 28.8vh;
  }
  .tig-creator-iframe-area {
    z-index: 2147483630;
  }
</style>
```

## ダイレクト埋め込み（type: "direct"）

指定した場所へプレイヤーを直接埋め込みます（iframe に近い見た目です）。取得するコンテンツは常に 1 件です。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"direct"` |
| `target` | 文字列 | ○ | なし | 埋め込み先の CSS セレクタ |
| `contentsId` | 文字列 | ○ | なし | 表示するコンテンツの ID。未指定だと何も表示されません |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ |
| `isAutoOpenModal` | 真偽値 | - | 未指定 | モーダルを自動で開きます |
| `isDirectInViewPip` | 真偽値 | - | false | 画面外へスクロールしたとき、縮小表示（PiP）で追従します |
| `isShowPipImmediately` | 真偽値 | - | false | スクロールを待たずに PiP を表示します |
| `pipPos` | 文字列 | - | `"right"` | PiP の位置。`"left"` か `"right"`（それ以外は右） |
| `listeners` | オブジェクト | - | なし | イベントリスナー（[listeners](#listeners)） |

```html
<div style="width: 500px; height: 500px;">
  <div id="tig-creator-frame-direct" style="width: 100%; height: 100%;"></div>
</div>
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
<script>
(function () {
  function processTigCreatorEmbedDirectFunc() {
    var urlParam = { ta_tag: "ec_direct_creator" };
    var ignoreParams = ['id'];
    if (window.location.search.length > 0) {
      var pairs = window.location.search.slice(1).split('&');
      for (var i = 0; i < pairs.length; i++) {
        var kv = pairs[i].split('=');
        if (ignoreParams.indexOf(kv[0]) === -1) {
          urlParam[kv[0]] = kv[1];
        }
      }
    }

    initTigCreatorEmbed({
      type: "direct",
      target: "#tig-creator-frame-direct",
      contentsId: "{content_id}",
      urlParam: urlParam
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processTigCreatorEmbedDirectFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function () {
      processTigCreatorEmbedDirectFunc();
    });
  }
})();
</script>
```

## 全画面表示（type: "fill"）

ページ全体をプレイヤーで埋めます。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"fill"` |
| `groupId` | 文字列 | ○ | なし | コンテンツグループの ID。未指定だと何も表示されません |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ |

表示するコンテンツは、埋め込み元ページの URL パラメータで決まります（初期化時に渡した `contentsId`・`collectionId`・`limit`・`order` は無視されます）。

| ページ URL のパラメータ | 意味 |
|---|---|
| `tig_id` | 表示するコンテンツの ID。なければ `id` を代わりに使います |
| `collection` | コレクションの ID |
| `limit` | 取得件数。未指定なら、コレクション指定は 20、ID 指定は 1 |
| `order` | 並び順 |

`tig_id`（または `id`）も `collection` もない場合は、何も表示されません。

```html
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
<script>
(function () {
  function processTigCreatorFillFunc() {
    initTigCreatorEmbed({
      type: "fill",
      groupId: "{gid}",
      urlParam: {
        ta_tag: "fill_creator"
      }
    });
  }

  if (document.readyState === 'interactive' || document.readyState === 'complete') {
    processTigCreatorFillFunc();
  } else {
    window.addEventListener('DOMContentLoaded', function() {
      processTigCreatorFillFunc();
    });
  }
})();
</script>
```

## カスタム表示（type: "custom"）

コンテンツ一覧を直接渡し、`customReadyCallback` で受け取るコントローラーで操作します。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"custom"` |
| `customContentsList` | 配列 | ○ | なし | コンテンツデータの配列。各要素は `identifier` を持ちます |
| `customReadyCallback` | 関数 | ○ | なし | コントローラーを受け取る関数 |
| `urlParam` | オブジェクト | - | なし | プレイヤーの URL パラメータ |
| `listeners` | オブジェクト | - | なし | イベントリスナー（[listeners](#listeners)） |

### コントローラーのメソッド

`custom` と `customwapi` で共通です。

| メソッド | 説明 |
|---|---|
| `open(identifier)` | 指定したコンテンツをモーダルで開きます。一覧にない識別子を渡すとエラーになります |
| `openFirst()` | 先頭のコンテンツを開きます |
| `close()` | モーダルを閉じます |
| `setContentsList(list)` | コンテンツ一覧を差し替えます |
| `addEventListener(type, listener)` | イベントリスナーを登録します（`ready`・`stock`・`addtocart`） |
| `play()` / `pause()` | 再生・一時停止 |
| `currentTime(秒)` | 再生位置を設定し、現在位置を返します（Promise）。プレイヤーの準備前に呼ぶとエラーになります |
| `hideStockAnimation()` | ストックのアニメーションを隠します |
| `hideTutorial()` | チュートリアルを隠します |
| `hidePlayButton()` / `hideVolumeButton()` / `hideStockButton()` / `hideShareButton()` | 各ボタンを隠します |
| `setBalloonAlways()` | 吹き出しを常時表示にします |

```html
<script src="https://cdn.tigmedia.jp/creator/embed/js/tig-creator-embed.js"></script>
<script>
initTigCreatorEmbed({
  type: "custom",
  customContentsList: [
    { identifier: "{content_id}" /* ...コンテンツのデータ... */ }
  ],
  customReadyCallback: function(controller) {
    // コントローラーでプログラムから操作する
    controller.open("{content_id}");

    // イベントリスナー登録
    controller.addEventListener("stock", function(data) {
      console.log("ストックされました", data);
    });
  },
  urlParam: { ta_tag: "custom_creator" }
});
</script>
```

## カスタム連携表示（type: "customwapi"）

`custom` と同様にコントローラーで操作しますが、コンテンツ一覧は自動で取得します。

| パラメータ | 型 | 必須 | 既定値 | 意味 |
|---|---|---|---|---|
| `type` | 文字列 | ○ | なし | `"customwapi"` |
| `customReadyCallback` | 関数 | ○ | なし | コントローラーを受け取る関数 |
| `groupId`／`collectionId`／`contentsId` | 文字列 | △ | なし | いずれか 1 つは必須。優先順位は `contentsId`、`collectionId`、`groupId` の順です |
| `order`・`limit` | - | - | - | 一覧表示と同じ |
| `urlParam`・`listeners` | - | - | - | 共通の設定と同じ |

コントローラーのメソッドは `custom` と同じです。

## 共通の設定

### order

並び順を指定します。

| 値 | 意味 | 備考 |
|---|---|---|
| `"created_at_desc"` | 作成日時の新しい順 | `groupId` を指定して `order` を省略したときの既定 |
| `"updated_at_desc"` | 更新日時の新しい順 | |
| `"random"` | ランダム | `contentsId` を指定した場合は、その先頭を固定して残りをシャッフルします |
| `"custom"` | コレクションで設定した並び順 | `collectionId` を指定したときのみ意味を持ちます |

上記以外の値は `"created_at_desc"` として扱われます。`collectionId` を指定して `order` を省略した場合は、コレクションに設定した並び順に従います。

### urlParam

プレイヤーの URL に付くクエリパラメータです。任意のキーを渡せます。値は連結されるだけなので、必要に応じて呼び出し側でエンコード（`encodeURIComponent`）してください。

| キー | 意味 |
|---|---|
| `ta_tag` | 流入経路を区別する計測タグ（例: `"ec_float_creator"`, `"ec_embed_creator"`） |
| `ta_exid`・`ta_ex1`〜`ta_ex3` | 自社の識別値を計測ログに残します（[ta_exid（外部 ID 連携）]({{ '/player-common/exid-tracking.html' | relative_url }})） |
| `id` | 各コンテンツの識別子に書き換えて使われます（指定しても上書きされます） |
| その他 | そのままプレイヤーへ渡されます |

### listeners

イベント名ごとに関数の配列を渡してイベントリスナーを登録します。

| イベント名 | タイミング |
|---|---|
| `ready` | プレイヤーの準備が完了したとき |
| `stock` | ストックの操作があったとき |
| `addtocart` | カートへ追加の操作があったとき |

```javascript
listeners: {
  "addtocart": [
    function(data) { console.log("カートに追加:", data); }
  ],
  "stock": [
    function(data) { console.log("ストック:", data); }
  ]
}
```

### stockMemo

| パラメータ | 型 | 意味 |
|---|---|---|
| `isShow` | 真偽値 | お買い物メモのアイコンの表示・非表示 |
| `title` | 文字列 | ストックインジケーターのタイトル |
| `showEvenZero` | 真偽値 | ストックが 0 件でもインジケーターを表示します |

### EC サービスとの連携（shopify）

EC サービスと連携する場合は、`shopify` パラメータに導入時にご案内する値を指定します。一覧表示・ダイレクト埋め込み・全画面表示・カスタム表示で指定でき、指定した値はプレイヤーにそのまま渡されます。

### ページ URL によるモーダルの自動展開

埋め込み元ページの URL に次のパラメータが付いていると、自動で処理されます。

| URL パラメータ | 意味 |
|---|---|
| `te_autoopen=true`（または `1`、値なし） | モーダルを自動で開きます。初期化時に `isAutoOpenModal` を渡している場合はそちらが優先されます |
| `te_cid={コンテンツ ID}` | 開くコンテンツを指定します。一覧にないときは開きません |
| `tig_id={コンテンツ ID}` | `type: "fill"` で表示するコンテンツ |

## エラーと終了時の挙動

### 呼び出し側で捕捉できるエラー

| エラーメッセージ | 発生条件 | 対象の type |
|---|---|---|
| `tig-creator-embed error: target is not set.` | `target` が未指定 | embed, direct |
| `tig-creator-embed error: targetElem is not exist.` | `target` の要素が見つからない | embed, direct |
| `type ${type} is not supported.` | `listeners` に未対応のイベント名を指定 | 全 type |
| `identifier ${identifier} is not exist in the customContentsList.` | `controller.open()` に一覧にない識別子を渡した | custom, customwapi |

### 呼び出しの `try/catch` では捕捉できないエラー

コンテンツの取得は非同期で行われるため、次のエラーは `initTigCreatorEmbed` の呼び出しを囲む `try/catch` では捕捉できません。

| エラーメッセージ | 発生条件 | 対象の type |
|---|---|---|
| `failed to load contents info.` | コンテンツの取得でエラー（見つからない場合を除く）が返った | embed, float, direct, customwapi, fill |
| `contents not found.` | コンテンツが見つからない（プレイヤーに「現在このコンテンツはご視聴できません」と表示されます） | embed, float, direct, customwapi, fill |
| `contents list is empty.` | 取得した一覧が空だった | embed, float, direct, customwapi, fill |
| `player is not ready.` | プレイヤーの準備前に `controller.currentTime()` を呼んだ（Promise がエラーになります） | custom, customwapi |

### エラーを出さずに何も表示しないケース

ブラウザのコンソールにメッセージが出力されるだけで、画面にエラーは表示されません。

| コンソールのメッセージ | 発生条件 | 対象の type |
|---|---|---|
| `tig-creator-embed info: contentsId is not set.` | `contentsId` が未指定 | direct |
| `tig-creator-embed info: customContentsList or customReadyCallback is not set.` | どちらかが未指定 | custom |
| `tig-creator-embed info: customReadyCallback is not set.` | 未指定 | customwapi |
| `tig-creator-embed info: groupId or collectionId or contentsId is not set.` | いずれも未指定 | customwapi |
| `tig-creator-embed info: groupId or collectionId is not set.` | どちらも未指定 | embed, float |
| `tig-creator-embed info: groupId is not set.` | 未指定 | fill |
| `tig-creator-embed info: parent's url parameter 'id' or 'collection' is required.` | ページ URL に `tig_id`（または `id`）も `collection` もない | fill |
| `Type-float already exists! return.` | 同じページに float が既にある | float |

## 関連ページ

- [Short の埋め込み・連携API]({{ '/player/tig-short/embed.html' | relative_url }})
- [Short のフローティング表示]({{ '/player/tig-short/floating.html' | relative_url }})
- [LIVE の埋め込みスクリプト パラメータ]({{ '/player/tig-live/embed-params.html' | relative_url }})
- [埋め込み連携の全体像]({{ '/player-common/embed-overview.html' | relative_url }})

## 用語

専門用語は[用語集]({{ '/glossary.html' | relative_url }})を参照してください。

---

最終更新: 2026-10-14 ／ 本ページは Tig プラットフォーム 顧客向け機能仕様の一部です。
