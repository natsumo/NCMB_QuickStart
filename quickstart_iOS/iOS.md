# iOS クイックスタート

このページでは、mobile backendをiOSアプリと連携させる手順を紹介します

## 目次
* アプリの新規作成
* SDKをインストールする
 * Carthageを利用する方法
 * CocoaPodsを利用する方法
 * SDKをダウンロードして利用する方法
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

* APIキーは後ほどXcodeアプリで使います

<div style="page-break-before:always"></div>

![Xcode](/common_image/icon_xcode.png)

* Xcodeでプロジェクトを作成します
 * 既存のプロジェクトを利用する場合はこの作業は不要です

 ![Xcodeプロジェクト](/quickstart_ios/image/test_xcodeproject.png)

* プロジェクトは一度閉じておきます

<div style="page-break-before:always"></div>

## SDKをインストールする
### Carthageを利用する方法

![ターミナル](/common_image/icon_terminal.png)

* Homebrewなどを利用してCarthageをインストールする必要があります
 1. iOS SDKを利用するプロジェクトのディレクトリへ移動します

   ![Carthage1](/quickstart_ios/image/Carthage1.png)

 2. touchコマンドでCartfileを作成します

   ![Carthage2](/quickstart_ios/image/Carthage2.png)

 3. Cartfileを編集します

   ![Carthage3](/quickstart_ios/image/Carthage3.png)

 4. carthageコマンドを使ってiOS SDKをビルドします

   ![Carthage4](/quickstart_ios/image/Carthage4.png)

   * プロジェクト内のCarthage/Build/iOS/ に NCMB.framework が作成されます

 5. プロジェクトに NCMB.Framework を追加します

<div style="page-break-before:always"></div>

![Xcode](/common_image/icon_xcode.png)

* xcodeでプロジェクトを開きます
* プロジェクト設定のGeneralタブから、「Linked Frameworks and Library」にある「＋」ボタンを押して、「Add Other...」を押して、「Carthage/Build/iOS/NCMB.framework」を選択します
* NCMB.Frameworkが表示されたら、Build Phasesタブに移動し＋ボタンを押して、「New Run Script Phase」を選択します
* 「shell/bin/sh」 と書かれている下のところに以下内容を記述します

 ![Carthage5](/quickstart_ios/image/Carthage5.png)

* その下にある「input Files」の「＋」ボタンを押して、NCMB.frameworkに情報を記述すれば、CarthageでのSDKインストールは完了です

 ![Carthage6](/quickstart_ios/image/Carthage6.png)

<div style="page-break-before:always"></div>


### CocoaPodsを利用する方法

![ターミナル](/common_image/icon_terminal.png)


#### （１）CocoaPodsをインストールする
* CocoaPodsがすでにインストールされている方はこちらの作業は不要です

1. cocoaPodsをインストールを行います

 ![cocoapods1](/quickstart_ios/image/cocoapods1.png)

2. cocoaPodsのセットアップを行います

 ![cocoapods2](/quickstart_ios/image/cocoapods2.png)

3. バージョン情報が表示されればインストール完了です

 ![cocoapods3](/quickstart_ios/image/cocoapods3.png)

#### （２）SDKライブラリのインストール
1. Xcodeプロジェクト内にある「プロジェクト名.xcodeproj」と同じディレクトリに移動します

 ![cocoapods4](/quickstart_ios/image/cocoapods4.png)

2. Podfile(インストールするライブラリを指定するファイル)を作成します

 ![cocoapods5](/quickstart_ios/image/cocoapods5.png)

3. podfileを開いて、以下の内容に書き換えてください

 ![podfile1](/quickstart_ios/image/podfile1.png)

 * 「`YOUR_APP_TARGET`」の部分は、作成しているXcodeプロジェクトのプロジェクト名に書き換えてください

4. 編集したpodfileを保存をします
5. podfileに書いたSDKをインストールします

 ![cocoapods6](/quickstart_ios/image/cocoapods6.png)

 * 基本は上記コマンドでインストールを行いますが、短時間でインストールが必要な場合は、`$ pod install --no-repo-update`が利用可能です

6. 「プロジェクト名.xcworkspace」が作成されます
 * 注意：元々ある「プロジェクト名.xcodeproj」からXcodeアプリを開いても、SDKが読み込まれませんので、必ず「プロジェクト名.xcworkspace」から開いて編集を行ってください

<div style="page-break-before:always"></div>

#### 参考：SDKのアップデートについて

* コマンドSDKのアップデートが可能です

 ![cocoapods7](/quickstart_ios/image/cocoapods7.png)

 * Cocoapodsを利用して導入したSDKの場合は上記コマンドの実行だけで更新可能です。

* ローカルに置いたSDKのリポジトリを指定していた場合は以下の方法で更新できます

 ![podfile2](/quickstart_ios/image/podfile2.png)

 * 「`YOUR_APP_TARGET`」の部分は、作成しているXcodeプロジェクトのプロジェクト名に書き換えてください

* 上記のようにpodfileを編集（GitHubリポジトリを指定）して、下記コマンドを実行します

 ![cocoapods8](/quickstart_ios/image/cocoapods8.png)

<div style="page-break-before:always"></div>

### SDKをダウンロードして利用する方法
#### （１）SDKをダウンロードする
* [GitHubのiOS SDKページ](https://github.com/NIFTYCloud-mbaas/ncmb_ios/)で「Clone or download ▼」＞「Download ZIP」をクリックし、masterブランチのzipファイルをダウンロードします
* ダウンロードしたzipファイルを解凍してフォルダを開きます
 * フォルダの中には「NCMB」というフォルダがあります。その中のファイルがSDKです。

#### （２）SDKをインストールする

![Xcode](/common_image/icon_xcode.png)

* Xcodeプロジェクトを開きます
* （1）で確認した「NCMB」フォルダをXcodeプロジェクトのターゲットグループ直下（AppDelegateクラスと同じ階層）にコピーします
* フォルダをコピーするときに、Xcodeでポップアップが開くので、次の様に設定します
 - 「Destination」の項目で「Copy items if needed」にチェックを入れる
 - 「Added folders」の項目で「Create groups」を選択する
 - 「Add to targets」の項目でSDKを利用するターゲットを選択する

<div style="page-break-before:always"></div>

#### 参考：SDKのアップデートについて

* 最新のSDKをダウンロードし、同様の操作で「NCMB」フォルダを置き換えることで、SDKのアップデートが可能です

#### 参考：ARCが無効な環境でSDKを利用する場合

* ARCが無効な環境でSDKを利用する場合は、以下の手順でSDKのみARCを有効にする設定を行います
 - ターゲットの一覧から対象のターゲットを選択
 - 「Build Phases」のタブにある「Compile Sources」を開く
 - ニフティクラウド mobile backendのiOS SDKを構成する全ファイルを選択
 - ダブルクリックして「Compiler Flags」に「-fobjc-arc」を設定

<br>
## SDKの読み込み

![Xcode](/common_image/icon_xcode.png)

* `AppDelegate.m`の冒頭に次のコードを追記して、インストールしたSDKを読み込みます
 * 追記するコードは、SDKのインストール方法によって異なります

   ![loading_sdk](/quickstart_ios/image/loading_sdk.png)

<div style="page-break-before:always"></div>

## APIキーの設定とSDKの初期化

![Xcode](/common_image/icon_xcode.png)

* コードを書いていく前に、必ずmBaaSで発行されたAPIキーの設定とSDKの初期化を行う必要があります
* `AppDelegate.m`の`application:didFinishLaunchingWithOptions:`メソッドに次のコードを書きます

 ![sdk_init](/quickstart_ios/image/sdk_init.png)

![ダッシュボード](/common_image/icon_dashboard.png)

* 上の「`YOUR_APPLICATION_KEY`」と「`YOUR_CLIENT_KEY`」は、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

   ![APIキー確認](/common_image/check_apikey.png)

* これで連携作業は完了です！サンプルコードを書いて実際にmBaaSを使ってみましょう

<div style="page-break-before:always"></div>

## サンプルコードの実装

![Xcode](/common_image/icon_xcode.png)

* `AppDelegate.m`の`application:didFinishLaunchingWithOptions:`メソッド内に書いた処理は、アプリの起動時に実行されます
* APIキーの設定とSDK初期化コードの下にサンプルコードを書くと、すぐに動作確認が可能です

 ![sample1](/quickstart_ios/image/sample1.png)

### サンプルコード（データストア）

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです

 ![sample2](/quickstart_ios/image/sample2.png)

<div style="page-break-before:always"></div>

### アプリを実行してmBaaSのダッシュボードを確認する

* アプリを実機またはシュミレーターで実行します

![ダッシュボード](/common_image/icon_dashboard.png)

* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

 ![DBサンプル結果](/common_image/dbdemo.png)
