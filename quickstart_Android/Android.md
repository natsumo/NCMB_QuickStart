# Android クイックスタート

このページでは、mobile backendをAndroidアプリを連携させる手順を紹介します

## 目次
* アプリの新規作成
* SDKのダウンロード
* SDKのインストール
* AndroidManifest.xml の編集
* SDKの読み込み
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

* APIキーは後ほどAndroidアプリで使います

![AndroidStudio](/common_image/icon_androidstudio.png)

* AndroidStudioでプロジェクトを作成します
 * 既存のプロジェクトを利用する場合はこの作業は不要です

 ![Androidプロジェクト](/quickstart_android/image/create_androidstudio1.png)

 ![Androidプロジェクト](/quickstart_android/image/create_androidstudio2.png)

<div style="page-break-before:always"></div>

## SDKのダウンロード

*  [Githubリリースページ](https://github.com/NIFTYCloud-mbaas/ncmb_android/releases)の NCMB.x.x.x.zip ボタンからダウンロードしてください
 *  最新版をダウンロードしてください。
 * zipファイルの中身に、NCMB.jarがあります。

## SDKのインストール

 * このSDKでは、以下のライブラリを使用しています。
  * __Gson__

![AndroidStudio](/common_image/icon_androidstudio.png)

* Android Studioでプロジェクトを開き、以下の手順でSDKをインストールしてください
 * app/libsフォルダにNCMB.jar をコピーします

    ![jarファイルをコピー](/quickstart_android/image/jar_file.png)

* app/build.gradleファイルに以下を追加します

 ![SDKのダウンロード](/quickstart_android/image/sdk_dl.png)

 ![jarファイルをコピー](/quickstart_android/image/gradle_file.png)

<div style="page-break-before:always"></div>

## AndroidManifest.xmlの編集

![AndroidStudio](/common_image/icon_androidstudio.png)

* &lt;application&gt;タグの直前に以下のpermissionを追加します

 ![AndroidManifest1](/quickstart_android/image/AndroidManifest1.png)

 ![jarファイルをコピー](/quickstart_android/image/AndroidManifest_xml.png)

* プロセスが存在しない状態からサービスなどでアプリを起動しAPIリクエストを行うためには、
`<application>`タグに以下を追加します。

 ![AndroidManifest1](/quickstart_android/image/AndroidManifest1.png)

<div style="page-break-before:always"></div>

## SDKの読み込み

![AndroidStudio](/common_image/icon_androidstudio.png)

* MainActivity.javaの冒頭に次のコードを追記して、インストールしたSDKを読み込みます

 ![SDKの読込](/quickstart_android/image/loading_sdk.png)


## APIキーの設定とSDKの初期化

![AndroidStudio](/common_image/icon_androidstudio.png)

* ニフティクラウド mobile backendのアプリケーションキーとクライアントキーを利用して、 NCMBクラスのinitializeメソッドでAndroid SDKの初期化を行います
* アプリ起動時に表示するアクティビティのonCreateメソッドに下記の内容を追記します

 ![SDKの初期化](/quickstart_android/image/init_sdk.png)

<div style="page-break-before:always"></div>

![ダッシュボード](/common_image/icon_dashboard.png)

* 上の「`APP_KEY`」と「`CLIENT_KEY`」は、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

   ![APIキー確認](/common_image/check_apikey.png)


 * これで連携作業は完了です！
 * サンプルコードを書いて実際にmBaaSを使ってみましょう

<div style="page-break-before:always"></div>

## サンプルコードの実装

![AndroidStudio](/common_image/icon_androidstudio.png)

* 最初に、ファイルの先頭に利用するライブラリを追記します

 ![sample1](/quickstart_android/image/sample1.png)

* `MainActivity.java`の`onCreate`メソッド内に書いた処理は、アプリの起動時に実行されます
* APIキーの設定とSDK初期化コードの下にサンプルコードを書くと、すぐに動作確認が可能です

 ![sample2](/quickstart_android/image/sample2.png)

<div style="page-break-before:always"></div>

### サンプルコード（データストア）

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです。

 ![sample3](/quickstart_android/image/sample3.png)

<div style="page-break-before:always"></div>

### アプリを実行してmBaaSのダッシュボードを確認する

* アプリを実機またはシミュレーターで実行します

![ダッシュボード](/common_image/icon_dashboard.png)


* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

 ![DBサンプル結果](/common_image/dbdemo.png)
