## Unity クイックスタート

このページでは、mobile backendをUnityアプリと連携させる手順を紹介します

### アプリの新規作成

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* ニフティクラウドmobile backendに[ログイン](https://console.mb.cloud.nifty.com)します
* ダッシュボードが表示されたら、「アプリの新規作成」を行います
 * すでに別のアプリを作成済みの場合は、ヘッダーの「＋新しいアプリ」をクリックします

<img src="/quickstart/create_app.png" width="600" alt="新規アプリケーション作成" />

* 「アプリ名」を入力し「新規作成」をクリックすると、APIキー（アプリケーションキーとクライアントキー）が発行されます

<img src="/quickstart/create_app2.png" width="600" alt="APIキー発行" />

* APIキーは後ほどUnityアプリで使います

<img src="/quickstart/icon_unity.png" width="100" height="27" alt="Unity" />

* Unityでプロジェクトを作成します
 * 既存のプロジェクトを利用する場合はこの作業は不要です

<img src="/quickstart_unity/new_unityproject.png" width="500"  alt="Unityプロジェクト" />

<img src="/quickstart_unity/unity_project.png" width="500" alt="Unityプロジェクト" />

### SDKのダウンロード

* 以下のリンクからGithubのリリースページを開き、NCMB.2.x.x.zip(xはバージョン番号)をダウンロードしてください

[Githubのリリースページ](https://github.com/NIFTYCloud-mbaas/ncmb_unity/releases)

<img src="/quickstart_unity/sdk_download.png" width="300" alt="Unity download" />

### SDKをインストールする

<img src="/quickstart/icon_unity.png" width="100" height="27" alt="Unity" />

* 先ほどダウンロードしたzipファイルを解凍します
* フォルダ内にある「NCMB.unitypackage」をダブルクリックして、インポートてください

<img src="/quickstart_unity/import.png" width="300" alt="SDKを配置" />


<img src="/quickstart_unity/import_done.png" width="300" alt="SDKを配置" />


### SDKの読み込み

<img src="/quickstart/icon_unity.png" width="100" height="27" alt="Unity" />

* 空のGame Objectを作成します
 * わかりやすいように、作成したGame Objectをここでは「NCMBSettings」という名前に変更します。

<img src="/quickstart_unity/create_empty.PNG" width="600" alt="空のGame Objectを生成する" />

* インポートした「NCMB」のフォルダ内にある「NCMBSettings.cs」を、先ほど作成した「NCMBSettings」にドラッグ＆ドロップでアタッチします

<img src="/quickstart_unity/attachNCMBSettings.PNG" width="600" alt="NCMBSettingsをアタッチする" />

<img src="/quickstart_unity/create_ncmbsettings.PNG" width="600" alt="NCMBSettingsを生成する" />

### APIキーの設定とSDKの初期化

<img src="/quickstart/icon_unity.png" width="100" height="27" alt="Xcode" />

* コードを書いていく前に、必ずmBaaSで発行されたAPIキーの設定を行う必要があります
* アタッチした状態でヒエラルキー（Hierarchy）の「NCMBSettings」をクリックすると、インスペクタ（Inspector）に図のようなアプリケーションキーとクライアントキーの入力欄が表示されます

<img src="/quickstart_unity/key_setting.png" width="300" alt="APIキー設定" />

- プッシュ通知機能を使用する場合、Use Pushにチェックを入れてください
 - Android向けにプッシュ通知を行う場合、さらにAndroid Sender IDを設定します。
 - 開封通知を行う場合、Use Analyticsにチェックを入れてください。
 - 詳しくは[プッシュ通知の基本的な使い方](/doc/current/push/basic_usage_unity.html)をご覧ください。
- レスポンスシグネチャの検証を行う場合はResponse Validationにチェックを入れてください

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* 上のアプリケーションキーとクライアントキーは、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

<img src="/quickstart/check_apikey.png" width="700" height="417" alt="APIキー確認" />

* これで連携作業は完了です！
* サンプルコードを書いて実際にmBaaSを使ってみましょう

### サンプルコードの実装

* ここまでの作業を行えば、プロジェクト中のどのシーン、どのスクリプトでもmobile backendの機能を使用することができます
* 新しく「空のGame Object」と「C#スクリプト」を作成し、そのスクリプトを空のGame Objectにアタッチします

<img src="/quickstart_unity/ncmb_test_object.png" width="700" alt="ncmb import" />

* スクリプトを開き、最上部に以下の内容を追記します

```csharp
using NCMB;
```

<img src="/quickstart_unity/import_ncmb.png" width="400"  alt="ncmb import" />

* Startメソッド内に書いた処理は、GameObjectの起動時に実行されます
* Startメソッドの中にサンプルコードを書くと、すぐに動作確認が可能です

```csharp
// Use this for initialization
void Start () {
  // ↓　ここにサンプルコードを実装　↓

}
```

#### サンプルコード（データストア）

<img src="/quickstart/icon_unity.png" width="100" height="27" alt="Unity" />

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです

```csharp
// クラスのNCMBObjectを作成
NCMBObject testClass = new NCMBObject("TestClass");

// オブジェクトに値を設定

testClass["message"] = "Hello, NCMB!";
// データストアへの登録
testClass.SaveAsync();
```


#### アプリを実行してmBaaSのダッシュボードを確認する

* アプリを実機またはシュミレーターで実行します

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />


* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

<img src="/quickstart/dbdemo.png" width="700" height="342" alt="DBサンプル結果" />

### 注意事項

- WindowsでMonoDevelopを使用しビルドする際、「.NET 4.0」以上を選択する必要があります。設定方法はMonoDevelopの[Project] -> [Assembly-CSharp options] -> [Build/General] -> Target Framework で Mono/.NET 4.0を選択します。

### Unityアプリのチュートリアルについて

* 他にもさまざまなチュートリアルをご用意しています
 * [チュートリアルを見る](/doc/current/tutorial/tutorial_unity.html)

* ５分体験会
 * あっという間にmBaaS（データストア）を体験できます！ 興味がある方は[こちら](http://www.slideshare.net/mobilebackend/unity-60586717)

ぜひご活用ください！
