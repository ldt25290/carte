# CARTE
CARTEは、「KARTE for App」のSDKを検証するためのサンプルアプリケーションです。
このサンプルアプリケーションでは、KARTE for App SDKの基本的な機能を試す事が可能です。

## Requirements
* iOS 12.0+
* Xcode 10.2+
* Swift 5+
* Node.js 10.10+

## Installation (Firebase)
このサンプルアプリケーションでは、バックエンドに「Firebase」を採用しております。
そのため事前にFirebaseのセットアップが必要になります。

### Features of the required Firebase.
- [x] Authentication
- [x] Cloud Firestore
- [x] Cloud Messaging
- [x] Cloud Storage
- [x] Hosting

#### Authentication
以下のログインプロバイダを有効にしてください。
- [x] Email and password based authentication
- [x] Anonymous auth

#### Cloud Firestore
以下の通りセキュリティルールを設定してください。
```
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth.uid != null;
    }
  }
}
```

#### Cloud Messaging
「APNs Auth Key」を設定する必要があります。
詳細は[こちら](https://firebase.google.com/docs/cloud-messaging/ios/certs)をご覧ください。

#### Hosting
Hostingを利用するためには、`Firebase CLI`が必要になります。
[こちら](https://firebase.google.com/docs/hosting/quickstart)を参考に設定を行ってください。

また以下のHTMLファイルに、KARTEの計測タグを設置する必要があります。
計測タグの設置方法に関しては、[こちら](https://developers.karte.io/docs/setup-web)をご覧ください。
- hosting/public/order.html
- hosting/public/complete.html

### Set the initial data.
サンプルアプリケーションを利用するために、事前に初期データをFirebaseに投入する必要があります。
初期データ投入スクリプトは `node.js` で書かれており、いくつかのパッケージに依存しています。
スクリプト実行前に、依存パッケージをインストールしてください。
```bash
cd carte/firebase/fixture
npm install
```

以下のコマンドを実行して、初期データの投入を行ってください （コマンドの実行には Node.js v10.10 以上が必要です）

```bash
cd carte/firebase/fixture
node index.js
```

引き続き、以下のコマンドを実行して、HTMLコンテンツのホスティング処理を行なってください。
```bash
cd carte/firebase/hosting
firebase deploy
```

## Installation (Application)
サンプルアプリケーションをビルドするためには、CocoaPods等のツールを事前にインストールしておく必要があります。
以下のコマンドを実行してください。
```bash
cd carte/CARTE
bundle install --path vendor/bundler
```

サンプルアプリケーションは、いくつかのライブラリに依存しています。
以下のコマンドを実行して、依存ライブラリのインストールを行なってください。
```bash
cd carte/CARTE
bundle exec pod install
```

サンプルアプリケーションを実行するには、数カ所コードの修正を行う必要があります。
- `CARTE/CARTE/Configuration.swift` の修正 (ソースコードコメントを参考に以下3箇所を修正)
  - applicationKey
  - webContentHostingURL
  - deeplinkBaseURL

Xcodeを起動して、「⌘ + R」キーを押しビルドを行います。
各種設定が問題なく行われていれば、ビルドが成功し、アプリケーションが起動するはずです。
