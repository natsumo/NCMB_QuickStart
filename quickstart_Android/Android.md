## Android クイックスタート

このページでは、mobile backendをAndroidアプリを連携させる手順を紹介します

### アプリの新規作成

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* ニフティクラウドmobile backendに[ログイン](https://console.mb.cloud.nifty.com)します
* ダッシュボードが表示されたら、「アプリの新規作成」を行います
 * すでに別のアプリを作成済みの場合は、ヘッダーの「＋新しいアプリ」をクリックします

<img src="/quickstart/create_app.png" width="600" height="370" alt="新規アプリケーション作成" />

* 「アプリ名」を入力し「新規作成」をクリックすると、APIキー（アプリケーションキーとクライアントキー）が発行されます

<img src="/quickstart/create_app2.png" width="600" height="399" alt="APIキー発行" />

* APIキーは後ほどAndroidアプリで使います

<img src="/quickstart/icon_androidstudio.png" width="100" height="27" alt="AndroidStudio" />

* AndroidStudioでプロジェクトを作成します
 * 既存のプロジェクトを利用する場合はこの作業は不要です

<img src="/quickstart_android/create_androidstudio1.png" width="500" alt="Androidプロジェクト" />

<img src="/quickstart_android/create_androidstudio2.png" width="500" alt="Androidプロジェクト" />

### SDKのダウンロード

*  [Githubリリースページ](https://github.com/NIFTYCloud-mbaas/ncmb_android/releases)の NCMB.x.x.x.zip ボタンからダウンロードしてください
 *  最新版をダウンロードしてください。
 * zipファイルの中身に、NCMB.jarがあります。

### SDKのインストール

 * このSDKでは、以下のライブラリを使用しています。
  *  Gson

<img src="/quickstart/icon_androidstudio.png" width="100" height="27" alt="AndroidStudio" />

* Android Studioでプロジェクトを開き、以下の手順でSDKをインストールしてください
 * app/libsフォルダにNCMB.jarをコピーします

<img src="/quickstart_android/jar_file.png" width="500" alt="jarファイルをコピー" />

* app/build.gradleファイルに以下を追加します

```
dependencies {
    compile 'com.google.code.gson:gson:2.3.1'
    compile files('libs/NCMB.jar')
}
```

<img src="/quickstart_android/gradle_file.png" width="500" alt="jarファイルをコピー" />

### AndroidManifest.xmlの編集

<img src="/quickstart/icon_androidstudio.png" width="100" height="27" alt="AndroidStudio" />

* &lt;application&gt;タグの直前に以下のpermissionを追加します

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

<img src="/quickstart_android/AndroidManifest_xml.png" width="500" alt="jarファイルをコピー" />

* プロセスが存在しない状態からサービスなどでアプリを起動しAPIリクエストを行うためには、
&lt;application&gt;タグに以下を追加します。

```
android:name="com.nifty.cloud.mb.core.NCMBApplicationController"
```


### SDKの読み込み

<img src="/quickstart/icon_androidstudio.png" width="100" height="27" alt="AndroidStudio" />

* MainActivity.javaの冒頭に次のコードを追記して、インストールしたSDKを読み込みます

```java
import com.nifty.cloud.mb.core.NCMB;
```


### APIキーの設定とSDKの初期化

<img src="/quickstart/icon_androidstudio.png" width="100" height="27" alt="AndroidStudio" />

* ニフティクラウド mobile backendのアプリケーションキーとクライアントキーを利用して、 NCMBクラスのinitializeメソッドでAndroid SDKの初期化を行います
* アプリ起動時に表示するアクティビティのonCreateメソッドに下記の内容を追記します

```java
NCMB.initialize(this.getApplicationContext(),"APP_KEY","CLIENT_KEY");
```

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* 上の「`APP_KEY`」と「`CLIENT_KEY`」は、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
 * アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
 * 「コピー」ボタンを使用してコピーしてください。

 <img src="/quickstart/check_apikey.png" width="700" height="417" alt="APIキー確認" />

 * これで連携作業は完了です！
 * サンプルコードを書いて実際にmBaaSを使ってみましょう

### サンプルコードの実装

<img src="/quickstart/icon_androidstudio.png" width="100" height="27" alt="AndroidStudio" />

* 最初に、ファイルの先頭に利用するライブラリを追記します

```java
import com.nifty.cloud.mb.core.NCMB;
import com.nifty.cloud.mb.core.NCMBException;
import com.nifty.cloud.mb.core.NCMBObject;
```

* `MainActivity.java`の`onCreate`メソッド内に書いた処理は、アプリの起動時に実行されます
* APIキーの設定とSDK初期化コードの下にサンプルコードを書くと、すぐに動作確認が可能です

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    // APIキーの設定とSDK初期化
    NCMB.initialize(this.getApplicationContext(), "APP_KEY", "CLIENT_KEY");
    // ↓　ここにサンプルコードを実装　↓


    setContentView(R.layout.activity_main);
}
```

#### サンプルコード（データストア）

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです。

```java
// クラスのNCMBObjectを作成
NCMBObject obj = new NCMBObject("TestClass");

// オブジェクトの値を設定
obj.put("message", "Hello, NCMB!");

// データストアへの登録
obj.saveInBackground(new DoneCallback() {
    @Override
    public void done(NCMBException e) {
        if(e != null){
            //保存に失敗した場合の処理

        }else {
            //保存に成功した場合の処理

        }
    }
});

```

#### アプリを実行してmBaaSのダッシュボードを確認する

* アプリを実機またはシミュレーターで実行します

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />


* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

<img src="/quickstart/dbdemo.png" width="700" height="342" alt="DBサンプル結果" />

### Androidアプリのチュートリアルについて

* 他にもさまざまなチュートリアルをご用意しています
 * [チュートリアルを見る](/doc/current/tutorial/tutorial_android.html)

* ５分体験会
 * あっという間にmBaaS（データストア）を体験できます！ 興味がある方は[こちら](http://www.slideshare.net/mobilebackend/mbaas-android-datastore-demo)

ぜひご活用ください！
