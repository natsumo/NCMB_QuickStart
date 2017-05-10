## iOS クイックスタート

このページでは、mobile backendをiOSアプリと連携させる手順を紹介します

### アプリの新規作成
<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* ニフティクラウドmobile backendに[ログイン](https://console.mb.cloud.nifty.com)します
* ダッシュボードが表示されたら、「アプリの新規作成」を行います
 * すでに別のアプリを作成済みの場合は、ヘッダーの「＋新しいアプリ」をクリックします

<img src="/quickstart/create_app.png" width="600" height="370" alt="新規アプリケーション作成" />

* 「アプリ名」を入力し「新規作成」をクリックすると、APIキー（アプリケーションキーとクライアントキー）が発行されます

<img src="/quickstart/create_app2.png" width="600" height="399" alt="APIキー発行" />

* APIキーは後ほどXcodeアプリで使います

<img src="/quickstart/icon_xcode.png" width="100" height="27" alt="Xcode" />

* Xcodeでプロジェクトを作成します
 * 既存のプロジェクトを利用する場合はこの作業は不要です

<img src="/quickstart_ios/test_xcodeproject.png" width="800" height="399" alt="Xcodeプロジェクト" />


* プロジェクトは一度閉じておきます

### SDKをインストールする

#### Carthageを利用する方法

<img src="/quickstart/icon_terminal.png" width="100" height="27" alt="ターミナル" />

* Homebrewなどを利用してCarthageをインストールする必要があります

1.iOS SDKを利用するプロジェクトのディレクトリへ移動します

```bash
$ cd 'プロジェクトパス'
```

2.touchコマンドでCartfileを作成します

```bash
$ touch Cartfile
```

3.Cartfileを編集します

```text
github "NIFTYCloud-mbaas/ncmb_ios"
```

4.carthageコマンドを使ってiOS SDKをビルドします

```bash
$ carthage update --platform iOS
```

* プロジェクト内のCarthage/Build/iOS/　にNCMB.frameworkが作成されます

5.プロジェクトにNCMB.Frameworkを追加します

<img src="/quickstart/icon_xcode.png" width="100" height="27" alt="Xcode">

* xcodeでプロジェクトを開きます
* プロジェクト設定のGeneralタブから、「Linked Frameworks and Library」にある「＋」ボタンを押して、「Add Other...」を押して、「Carthage/Build/iOS/NCMB.framework」を選択します
* NCMB.Frameworkが表示されたら、Build Phasesタブに移動し＋ボタンを押して、「New Run Script Phase」を選択します
* 「shell/bin/sh」 と書かれている下のところに以下内容を記述します

```bash
$ /usr/local/bin/carthage copy-frameworks
```

* その下にある「input Files」の「＋」ボタンを押して、NCMB.frameworkに情報を記述すれば、CarthageでのSDKインストールは完了です

```text
$(SRCROOT)/Carthage/Build/iOS/NCMB.framework
```

#### CocoaPodsを利用する方法

<img src="/quickstart/icon_terminal.png" width="100" height="27" alt="ターミナル" />


__（１）CocoaPodsをインストールする__

* CocoaPodsがすでにインストールされている方はこちらの作業は不要です

1.cocoaPodsをインストールを行います

```bash
$ sudo gem install cocoapods
```

2.cocoaPodsのセットアップを行います

```bash
$ pod setup
```

3.バージョン情報が表示されればインストール完了です

```bash
$ pod --version
```

__（２）SDKライブラリのインストール__

1.Xcodeプロジェクト内にある「プロジェクト名.xcodeproj」と同じディレクトリに移動します

```bash
$ cd 'プロジェクトパス'
```

2.Podfile(インストールするライブラリを指定するファイル)を作成します

```bash
$ pod init
```

3.podfileを開いて、以下の内容に書き換えてください

```ruby
# Uncomment this line to define a global platform for your project
platform :ios, '8.0'
# Uncomment this line if you're using Swift
# use_frameworks!

target "YOUR_APP_TARGET" do
    pod 'NCMB', :git => 'https://github.com/NIFTYCloud-mbaas/ncmb_ios.git'
end
```

※「`YOUR_APP_TARGET`」の部分は、作成しているXcodeプロジェクトのプロジェクト名に書き換えてください

4.編集したpodfileを保存をします
5.podfileに書いたSDKをインストールします

```bash
$ pod install
```

※基本は上記コマンドでインストールを行いますが、短時間でインストールが必要な場合は、`$ pod install --no-repo-update`が利用可能です

6.「プロジェクト名.xcworkspace」が作成されます

* 注意：元々ある「プロジェクト名.xcodeproj」からXcodeアプリを開いても、SDKが読み込まれませんので、必ず「プロジェクト名.xcworkspace」から開いて編集を行ってください


__　参考：SDKのアップデートについて__

* コマンドSDKのアップデートが可能です

```bash
$ pod update
```

 * Cocoapodsを利用して導入したSDKの場合は上記コマンドの実行だけで更新可能です。

* ローカルに置いたSDKのリポジトリを指定していた場合は以下の方法で更新できます

```ruby
# Uncomment this line to define a global platform for your project
platform :ios, '8.0'
# Uncomment this line if you're using Swift
# use_frameworks!

target "YOUR_APP_TARGET" do
#  以下のようにローカルのpathを指定していた場合はPodfileを変更する
#  pod 'NCMB', :path =>'your directory path'
#
#  変更後のpod指定方法
   pod 'NCMB', :git => 'https://github.com/NIFTYCloud-mbaas/ncmb_ios.git'
end
```

※「`YOUR_APP_TARGET`」の部分は、作成しているXcodeプロジェクトのプロジェクト名に書き換えてください

* 上記のようにpodfileを編集（GitHubリポジトリを指定）して、下記コマンドを実行します

```bash
$ pod update
```


#### SDKをダウンロードして利用する方法


__（１）SDKをダウンロードする__

* [GitHubのiOS SDKページ](https://github.com/NIFTYCloud-mbaas/ncmb_ios/)で「Clone or download ▼」＞「Download ZIP」をクリックし、masterブランチのzipファイルをダウンロードします
* ダウンロードしたzipファイルを解凍してフォルダを開きます
 * フォルダの中には「NCMB」というフォルダがあります。その中のファイルがSDKです。

__（２）SDKをインストールする__

<img src="/quickstart/icon_xcode.png" width="100" height="27" alt="Xcode" />

* Xcodeプロジェクトを開きます
* (1)で確認した「NCMB」フォルダをXcodeプロジェクトのターゲットグループ直下（AppDelegateクラスと同じ階層）にコピーします
* フォルダをコピーするときに、Xcodeでポップアップが開くので、次の様に設定します
 - 「Destination」の項目で「Copy items if needed」にチェックを入れる
 - 「Added folders」の項目で「Create groups」を選択する
 - 「Add to targets」の項目でSDKを利用するターゲットを選択する


__　参考：SDKのアップデートについて__

* 最新のSDKをダウンロードし、同様の操作で「NCMB」フォルダを置き換えることで、SDKのアップデートが可能です

__　参考：ARCが無効な環境でSDKを利用する場合__

* ARCが無効な環境でSDKを利用する場合は、以下の手順でSDKのみARCを有効にする設定を行います
 - ターゲットの一覧から対象のターゲットを選択
 - 「Build Phases」のタブにある「Compile Sources」を開く
 - ニフティクラウド mobile backendのiOS SDKを構成する全ファイルを選択
 - ダブルクリックして「Compiler Flags」に「-fobjc-arc」を設定


### SDKの読み込み

<img src="/quickstart/icon_xcode.png" width="100" height="27" alt="Xcode" />

* `AppDelegate.m`の冒頭に次のコードを追記して、インストールしたSDKを読み込みます
 * 追記するコードは、SDKのインストール方法によって異なります

```objc
// CocoaPodsを利用する方法
#import <NCMB/NCMB.h>

// SDKをダウンロードして利用する方法
#import "NCMB/NCMB.h"
```

### APIキーの設定とSDKの初期化

<img src="/quickstart/icon_xcode.png" width="100" height="27" alt="Xcode" />

* コードを書いていく前に、必ずmBaaSで発行されたAPIキーの設定とSDKの初期化を行う必要があります
* `AppDelegate.m`の`application:didFinishLaunchingWithOptions:`メソッドに次のコードを書きます

```objc
// APIキーの設定とSDK初期化
[NCMB setApplicationKey:@"YOUR_APPLICATION_KEY" clientKey:@"YOUR_CLIENT_KEY"];
```

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* 上の「`YOUR_APPLICATION_KEY`」と「`YOUR_CLIENT_KEY`」は、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

<img src="/quickstart/check_apikey.png" width="700" height="417" alt="APIキー確認" />


* これで連携作業は完了です！
* サンプルコードを書いて実際にmBaaSを使ってみましょう


### サンプルコードの実装

<img src="/quickstart/icon_xcode.png" width="100" height="27" alt="Xcode" />

* `AppDelegate.m`の`application:didFinishLaunchingWithOptions:`メソッド内に書いた処理は、アプリの起動時に実行されます
* APIキーの設定とSDK初期化コードの下にサンプルコードを書くと、すぐに動作確認が可能です

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
// APIキーの設定とSDK初期化
[NCMB setApplicationKey:@"YOUR_APPLICATION_KEY" clientKey:@"YOUR_CLIENT_KEY"];
// ↓　ここにサンプルコードを実装　↓


return YES;
}
```

#### サンプルコード（データストア）

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです

```objc
// クラスのNCMBObjectを作成
NCMBObject *object = [NCMBObject objectWithClassName:@"TestClass"];
// オブジェクトに値を設定
[object setObject:@"Hello, NCMB!" forKey:@"message"];
// データストアへの登録
[object saveInBackgroundWithBlock:^(NSError *error) {
    if (error){
        // 保存に失敗した場合の処理

    } else {
        // 保存に成功した場合の処理

    }
}];
```

#### アプリを実行してmBaaSのダッシュボードを確認する

* アプリを実機またはシュミレーターで実行します

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />


* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

<img src="/quickstart/dbdemo.png" width="700" height="342" alt="DBサンプル結果" />


### iOSアプリのチュートリアルについて

* 他にもさまざまなチュートリアルをご用意しています
 * [チュートリアルを見る](/doc/current/tutorial/tutorial_ios.html)

* ５分体験会
 * あっという間にmBaaS（データストア）を体験できます！ 興味がある方は[こちら](http://www.slideshare.net/mobilebackend/ios-objective-cmobile-backend)

ぜひご活用ください！
