# Unity クイックスタート
_2017/05/15作成_

このページでは、mobile backendをUnityアプリと連携させる手順を紹介します

## 目次
* アプリの新規作成
* SDKのダウンロード
* SDKをインストールする
* SDKの読み込み
* APIキーの設定とSDKの初期化
* サンプルコードの実装
 * サンプルコード（データストア）
 * アプリを実行してmBaaSのダッシュボードを確認する
* 注意事項

<div style="page-break-before:always"></div>

## アプリの新規作成

![ダッシュボード](/common_image/icon_dashboard.png)

* ニフティクラウドmobile backendに[ログイン](https://console.mb.cloud.nifty.com)します
* ダッシュボードが表示されたら、「アプリの新規作成」を行います
 * すでに別のアプリを作成済みの場合は、ヘッダーの「＋新しいアプリ」をクリックします

 ![新規アプリケーション作成](/common_image/create_app.png)

* 「アプリ名」を入力し「新規作成」をクリックすると、APIキー（アプリケーションキーとクライアントキー）が発行されます

 ![APIキー発行](/common_image/create_app2.png)

* APIキーは後ほどUnityアプリで使います

<div style="page-break-before:always"></div>

![Unity](/common_image/icon_unity.png)

* Unityでプロジェクトを作成します
 * 既存のプロジェクトを利用する場合はこの作業は不要です

 ![Unityプロジェクト](/quickstart_unity/image/new_unityproject.png)

 ![Unityプロジェクト](/quickstart_unity/image/unity_project.png)

<div style="page-break-before:always"></div>

## SDKのダウンロード

* 以下のリンクからGithubのリリースページを開き、NCMB.2.x.x.zip(xはバージョン番号)をダウンロードしてください

 [__Githubのリリースページ__](https://github.com/NIFTYCloud-mbaas/ncmb_unity/releases)

 ![Unity download](/quickstart_unity/image/sdk_download.png)

## SDKをインストールする

![Unity](/common_image/icon_unity.png)

* 先ほどダウンロードしたzipファイルを解凍します
* フォルダ内にある「NCMB.unitypackage」をダブルクリックして、インポートてください

<div style="page-break-before:always"></div>

 ![SDKを配置](/quickstart_unity/image/import.png)

 ![SDKを配置](/quickstart_unity/image/import_done.png)

 <div style="page-break-before:always"></div>

## SDKの読み込み

![Unity](/common_image/icon_unity.png)

* 空のGame Objectを作成します
 * わかりやすいように、作成したGame Objectをここでは「NCMBSettings」という名前に変更します。

 ![空のGame Objectを生成する](/quickstart_unity/image/create_empty.png)

<div style="page-break-before:always"></div>

* インポートした「NCMB」のフォルダ内にある「NCMBSettings.cs」を、先ほど作成した「NCMBSettings」にドラッグ＆ドロップでアタッチします

 ![NCMBSettingsをアタッチする](/quickstart_unity/image/attachNCMBSettings.png)

 ![NCMBSettingsを生成する](/quickstart_unity/image/create_ncmbsettings.png)

<div style="page-break-before:always"></div>

## APIキーの設定とSDKの初期化

![Unity](/common_image/icon_unity.png)

* コードを書いていく前に、必ずmBaaSで発行されたAPIキーの設定を行う必要があります
* アタッチした状態でヒエラルキー（Hierarchy）の「NCMBSettings」をクリックすると、インスペクタ（Inspector）に図のようなアプリケーションキーとクライアントキーの入力欄が表示されます

 ![APIキー設定](/quickstart_unity/image/key_setting.png)

- プッシュ通知機能を使用する場合、Use Pushにチェックを入れてください
 - Android向けにプッシュ通知を行う場合、さらにAndroid Sender IDを設定します。
 - 開封通知を行う場合、Use Analyticsにチェックを入れてください。
 - 詳しくは[プッシュ通知の基本的な使い方](/doc/current/push/basic_usage_unity.html)をご覧ください。
- レスポンスシグネチャの検証を行う場合はResponse Validationにチェックを入れてください

<div style="page-break-before:always"></div>

![ダッシュボード](/common_image/icon_dashboard.png)

* 上のアプリケーションキーとクライアントキーは、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

   ![APIキー確認](/common_image/check_apikey.png)


* これで連携作業は完了です！サンプルコードを書いて実際にmBaaSを使ってみましょう

<div style="page-break-before:always"></div>

## サンプルコードの実装

* ここまでの作業を行えば、プロジェクト中のどのシーン、どのスクリプトでもmobile backendの機能を使用することができます
* 新しく「空のGame Object」と「C#スクリプト」を作成し、そのスクリプトを空のGame Objectにアタッチします

 ![ncmb import](/quickstart_unity/image/ncmb_test_object.png)

<div style="page-break-before:always"></div>

* スクリプトを開き、最上部に以下の内容を追記します

 ![sample1](/quickstart_unity/image/sample1.png)

 ![ncmb import](/quickstart_unity/image/import_ncmb.png)

* Startメソッド内に書いた処理は、GameObjectの起動時に実行されます
* Startメソッドの中にサンプルコードを書くと、すぐに動作確認が可能です

 ![sample2](/quickstart_unity/image/sample2.png)

<div style="page-break-before:always"></div>

### サンプルコード（データストア）

![Unity](/common_image/icon_unity.png)

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです

 ![sample3](/quickstart_unity/image/sample3.png)

<div style="page-break-before:always"></div>

### アプリを実行してmBaaSのダッシュボードを確認する

* アプリを実機またはシュミレーターで実行します

![ダッシュボード](/common_image/icon_dashboard.png)

* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

 ![DBサンプル結果](/common_image/dbdemo.png)

## 注意事項
WindowsでMonoDevelopを使用しビルドする際、「.NET 4.0」以上を選択する必要があります。設定方法： MonoDevelop の[Project] -> [Assembly-CSharp options] -> [Build/General] -> Target Framework で Mono/.NET 4.0を選択します。
