## Monaca クイックスタート

このページでは、mobile backendをMonacaアプリと連携させる手順を紹介します

* [Monaca](https://ja.monaca.io/)とは、HTML5とJavaScriptでハイブリッドアプリがクラウド上で開発できる統合開発環境です。
* Monacaクラウドでの開発には下記のブラウザ環境が必要です
 * Google Chrome 最新版
* JavaScript、Adobe Flash が利用可能である必要があります

### アプリの新規作成

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* ニフティクラウドmobile backendに[ログイン](https://console.mb.cloud.nifty.com)します
* ダッシュボードが表示されたら、「アプリの新規作成」を行います
* すでに別のアプリを作成済みの場合は、ヘッダーの「＋新しいアプリ」をクリックします

<img src="/quickstart/create_app.png" width="600" height="370" alt="新規アプリケーション作成" />

* 「アプリ名」を入力し「新規作成」をクリックすると、APIキー（アプリケーションキーとクライアントキー）が発行されます

<img src="/quickstart/create_app2.png" width="600" height="399" alt="APIキー発行" />

* APIキーは後ほどXcodeアプリで使います

<img src="/quickstart/icon_monaca.png" width="100" height="27" alt="Monaca" />

* [Monaca](https://ja.monaca.io/)にログインし、プロジェクトを作成します
 * はじめての方は、上記リンクより会員登録（無料）を行ってください。
* 「新規プロジェクトの作成」＞「No Framework」を選択して、最小限のテンプレートを「作成」をクリックします
* 「新規プロジェクト」画面で、プロジェクト名と説明を入力（任意）し、「プロジェクトを作成する」をクリックします

<img src="/quickstart_javascript/monaca01.png" width="700" height="217" alt="Monacaでプロジェクトを作成" />

* プロジェクトが作成されたら「開く」をクリックすると、開発環境が表示されます

<img src="/quickstart_javascript/monaca02.png" width="700" height="325" alt="Monaca開発環境" />


### SDKのインストールと読み込み

<img src="/quickstart/icon_monaca.png" width="100" height="27" alt="Monaca" />

* 「設定」＞「JS/CSSコンポーネントの追加と削除...」をクリックします
* 「JS/CSSコンポーネントの追加と削除」画面で「ncmb」と入力して「検索」をクリックします
* 「ncmb インストールされていません。」と出ますので、「追加」をクリックします
* 「JS/CSSコンポーネントの追加」アラート画面が出ますので、「インストール開始」をクリックします
 * バージョンはデフォルトで最新版が選択されていますので、操作は不要です。
 * 画像は最新版のバージョンがv2.1.1の場合です。
* 「ローダーの設定」アラート画面が出ますので、「`components/ncmb/ncmb.min.js`」にチェックを入れ、「OK」をクリックします

<img src="/quickstart_javascript/monaca03.png" width="700" height="388" alt="SDKの導入" />

* 「ncmb」が下図のように追加されればSDKのインストールが完了です

<img src="/quickstart_javascript/monaca04.png" width="700" height="271" alt="SDK導入結果" />


### APIキーの設定とSDKの初期化

<img src="/quickstart/icon_monaca.png" width="100" height="27" alt="Monaca" />

* コードを書いていく前に、必ずmBaaSで発行されたAPIキーの設定とSDKの初期化を行う必要があります
* `index.html`の`<script>`と`</script>`の間に次のコードを書きます

```html
<script>
// APIキーの設定とSDK初期化
var ncmb = new NCMB("YOUR_APPLICATIONKEY","YOUR_CLIENTKEY");
</script>

```

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* 上の「`YOUR_APPLICATION_KEY`」と「`YOUR_CLIENT_KEY`」は、mBaaSのダッシュボードで「アプリの新規作成」を行ったときに発行されたAPIキーに置き換えます
* アプリ作成時のAPIキー発行画面を閉じてしまった場合は、「アプリ設定」＞「基本」で確認できます。
* 「コピー」ボタンを使用してコピーしてください。

<img src="/quickstart/check_apikey.png" width="700" height="417" alt="APIキー確認" />


* これで連携作業は完了です！
* サンプルコードを書いて実際にmBaaSを使ってみましょう

### サンプルコードの実装

<img src="/quickstart/icon_monaca.png" width="100" height="27" alt="Monaca" />

* `index.html`の`<script>`と`</script>`の間に書いた処理は、アプリの起動時に実行されます
* APIキーの設定とSDK初期化コードの下にサンプルコードを書くと、すぐに動作確認が可能です

```html
<script>
// APIキーの設定とSDK初期化
var ncmb = new NCMB("YOUR_APPLICATIONKEY","YOUR_CLIENTKEY");
// ↓　ここにサンプルコードを実装　↓



</script>

```

#### サンプルコード（データストア）

* 次のコードはmBaaSのデータストアに保存先の「TestClass」というクラスを作成し、「message」というフィールドへ「Hello, NCMB!」というメッセージ（文字列）を保存するものです

```js
// 保存先クラスの作成
var TestClass = ncmb.DataStore("TestClass");

// 保存先クラスのインスタンスを生成
var testClass = new TestClass();

// 値を設定と保存
testClass.set("message", "Hello, NCMB!")
         .save()
         .then(function(object){
             // 保存に成功した場合の処理

         })
         .catch(function(err){
             // 保存に失敗した場合の処理

         });
```

#### アプリを実行してmBaaSのダッシュボードを確認する

* 「保存」をクリックして保存します
* アプリを実行します
 * ブラウザ画面上で実行する場合は「プレビュー」をクリックします
 * [Monaca](https://ja.monaca.io/debugger.html)デバッガーで実行する場合は「実機デバッグ」をクリックします

<img src="/quickstart/icon_dashboard.png" width="100" height="27" alt="ダッシュボード" />

* アプリが起動されたら、mBaaSのダッシュボードで「データストア」から、データが保存されていることを確認できます

<img src="/quickstart/dbdemo.png" width="700" height="342" alt="DBサンプル結果" />

### Monacaのチュートリアルについて

* 他にもさまざまなチュートリアルをご用意しています
 * [チュートリアルを見る](/doc/current/tutorial/tutorial_monaca.html)

* ５分体験会
 * あっという間にmBaaS（データストア）を体験できます！ 興味がある方は[こちら](http://www.slideshare.net/mobilebackend/monaca-datastore-demo)

ぜひご活用ください！
