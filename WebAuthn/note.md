## WebAuthn
Webで使える認証機能、FIDO。  

参考サイト  
https://tech.mercari.com/entry/2019/06/04/120000  
https://techblog.yahoo.co.jp/advent-calendar-2018/webauthn/  
https://qiita.com/okamoai/items/bee403e3311cda20fe03  

対応ブラウザ  
https://caniuse.com/#feat=webauthn  

自家製テストサイト（ちょうざっくり）  
https://ye-webauthn-test-app.herokuapp.com/

### メモ

### 検証結果
同じDB、同じブラウザ、同じ認証機でも公開しているページが違うと発行される認証データが別のものになった？  
localhostで登録した情報で、公開したページのログインを試したらセキュリティキーが別物ですと言われた。  

Yubikeyに関しては、指紋認識機能がそもそもついていない。  
認証機から発行されるユニークな情報が一致するか判定しているみたいで、どの指を使っても同じIDが発行される。  
物理的な鍵と同じイメージ。  
Yubikey BIOという製品は指紋が認識できるみたいだが2020/06現在まだ発売されていない。  

大まかにいうと生体認証がwebで使えるようになったとかではなく、認証機に生体認証機能がついていれば、それでパスが通せるということだと思う。  
windows helloやtouchIDの指紋の情報をサーバーに保存しているわけではない。  
サーバーが保持しているのは認証につかった端末の情報だけで、その端末を認証する処理にPINやら生体認証を使っている。  

#### macOS10.14.6 Safari13.1.1
セキュリティキーを挿すように求められるがYubikey5が反応せず。  
touchIDは使えなかった。  

#### macOS10.14.6 Chrome83
Yubikey5、touchIDどちらも特に問題なし。  

#### Windows10 IE
非対応。  

#### Windows10 Edge
Yubikeyとinternal(built-in)に対応。   
internalはPCにログインするときのPIN、もしくはwindows hello  
windows helloで登録した認証はPINでも通る、逆もまた然り。  
Yubikeyを使うか、internalを使うか処理を分けることができる模様。アルゴリズムを-7にするか-256?だっけにするか。    
Chromeと違い、Yubikeyを使った場合いきなりタッチを求められる(後述)

#### Windows10 Chrome
Yubikeyとinternal(built-in)に対応。  
Yubikeyを使う場合、最初にサイト専用？のPINを作成するよう求められる。  
PINの認証成功→Yubikeyの認証という流れになる。  

#### iPhoneXs iOS13.4.1 Safari
認証処理を実行しようとすると、認証機に接続してくださいと出る。  
iPhone対応の認証機を持っていないため、これ以上の検証は出来ていない。  
iPhone対応のYubikeyが使えるそう。  
顔認証機能は使えなさそう。  

#### iPad iOS13.5.1 Safari
iPhoneXS iOS13.4.1 Safari 同様。  

#### Android9 指紋認証付き端末 Chrome
- Bluetoothでセキュリティキーを使用する
- USBでセキュリティキーを使用する
- 指紋またはPINでこのデバイスを使用する

の3択が表示された。  
Android対応のBluetoothとUSB認証機を持っていないため未検証。  
指紋は使えるみたいだが、検証に使った端末の指紋機能が故障しているため検証できなかった。  
指紋以外に画面ロックのパターンを使うことができた。  
これは指紋を新たにサイトへ登録しているのではなく、端末を使っているのが本人なのか確認している。なので画面ロックパターンでもいける。   

#### Android4.4.4 Chrome81.0.4044.138 実機
非対応。  
navigator.credentialsは存在するみたいだが、登録時になにも反応せず。  