# Monaca クイックスタート

このページでは、mobile backendをMonacaアプリと連携させる手順を紹介します

* [Monaca](https://ja.monaca.io/)とは、HTML5とJavaScriptでハイブリッドアプリがクラウド上で開発できる統合開発環境です。
* Monacaクラウドでの開発には下記のブラウザ環境が必要です
 * Google Chrome 最新版
* JavaScript、Adobe Flash が利用可能である必要があります

## 目次
* アプリの新規作成
* SDKのインストールと読み込み
* APIキーの設定とSDKの初期化
* サンプルコードの実装
 * サンプルコード（データストア）
 * アプリを実行してmBaaSのダッシュボードを確認する

<div style="page-break-before:always"></div>

## アプリの新規作成

![ダッシュボード](/common_image/icon_dashboard.png)

* ニフティクラウドmobile backendに[ログイン](https://console.mb.cloud.nifty.com)します
* ダッシュボードが表示されたら、「アプリの新規作成」を行います
* すでに別のアプリを作成済みの場合は、ヘッダーの「＋新しいアプリ」をクリックします

 ![新規アプリケーション作成](/common_image/create_app.png)


* 「アプリ名」を入力し「新規作成」をクリックすると、APIキー（アプリケーションキーとクライアントキー）が発行されます

 ![APIキー発行](/common_image/create_app2.png)

* APIキーは後ほどXcodeアプリで使います

<div style="page-break-before:always"></div>

![Monaca](/common_image/icon_monaca.png)

* [Monaca](https://ja.monaca.io/)にログインし、プロジェクトを作成します
 * はじめての方は、上記リンクより会員登録（無料）を行ってください。
* 「新規プロジェクトの作成」＞「No Framework」を選択して、最小限のテンプレートを「作成」をクリックします
* 「新規プロジェクト」画面で、プロジェクト名と説明を入力（任意）し、「プロジェクトを作成する」をクリックします

 ![Monacaでプロジェクトを作成](/quickstart_Monaca/image/monaca01.png)

* プロジェクトが作成されたら「開く」をクリックすると、開発環境が表示されます

 ![Monaca開発環境](/quickstart_Monaca/image/monaca02.png)

<div style="page-break-before:always"></div>

## SDKのインストールと読み込み

![Monaca](/common_image/icon_monaca.png)

* 「設定」＞「JS/CSSコンポーネントの追加と削除...」をクリックします
* 「JS/CSSコンポーネントの追加と削除」画面で「ncmb」と入力して「検索」をクリックします
* 「ncmb インストールされていません。」と出ますので、「追加」をクリックします
* 「JS/CSSコンポーネントの追加」アラート画面が出ますので、「インストール開始」をクリックします
 * バージョンはデフォルトで最新版が選択されていますので、操作は不要です。
 * 画像は最新版のバージョンがv2.1.1の場合です。
* 「ローダーの設定」アラート画面が出ますので、「`components/ncmb/ncmb.min.js`」にチェックを入れ、「OK」をクリックします

 ![SDKの導入](/quickstart_Monaca/image/monaca03.png)

<div style="page-break-before:always"></div>

* 「ncmb」が下図のように追加されればSDKのインストールが完了です

 ![SDK導入結果](/quickstart_Monaca/image/monaca04.png)

## APIキーの設定とSDKの初期化

![Monaca](/common_image/icon_monaca.png)

* コードを書いていく前に、必ずmBaaSで発行されたAPIキーの設定とSDKの初期化を行う必要があります
* `index.html`の`<script>`と`</script>`の間に次のコードを書きます

 ![SDKの初期化](/quickstart_Monaca/image/sdk_init.png)

<div style="page-break-before:always"></div>

![ダッシュボード](/common_image/icon_dashboard.png)

* 上の「`YOUR_APPLICATION_KEY`」と「`YOUR_CLIENT_KEY`」は、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

   ![APIキー確認](/common_image/check_apikey.png)

* これで連携作業は完了です！サンプルコードを書いて実際にmBaaSを使ってみましょう

<div style="page-break-before:always"></div>

## サンプルコードの実装

![Monaca](/common_image/icon_monaca.png)

* `index.html`の`<script>`と`</script>`の間に書いた処理は、アプリの起動時に実行されます
* APIキーの設定とSDK初期化コードの下にサンプルコードを書くと、すぐに動作確認が可能です

 ![sample1](/quickstart_Monaca/image/sample1.png)


<div style="page-break-before:always"></div>

### サンプルコード（データストア）

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです

 ![sample2](/quickstart_Monaca/image/sample2.png)

<div style="page-break-before:always"></div>

### アプリを実行してmBaaSのダッシュボードを確認する

* 「保存」をクリックして保存します
* アプリを実行します
 * ブラウザ画面上で実行する場合は「プレビュー」をクリックします
 * [Monaca](https://ja.monaca.io/debugger.html)デバッガーで実行する場合は「実機デバッグ」をクリックします

![ダッシュボード](/common_image/icon_dashboard.png)

* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

 ![DBサンプル結果](/common_image/dbdemo.png)
