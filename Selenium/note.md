# Selenium 

UI自動テストツール  
実行後のスクリーンショットを撮ることができるので、表示確認のエビデンスに使える。  

## 公式サイト(API)
https://www.selenium.dev/selenium/docs/api/javascript/index.html  

## 参考サイト 
入門系  
- https://ics.media/entry/5759/  
- https://saikeblog.com/2020/01/27/javascriptnode-js%E3%81%A7selenium%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E8%87%AA%E5%8B%95ui%E3%83%86%E3%82%B9%E3%83%88%E3%82%92%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/  

マルチブラウザ  
http://www.leenattress.com/multi-browser-automated-testing-with-selenium-and-mocha.html  

ベーシック認証がある場合  
https://qiita.com/mochi_yu2/items/ce598ec57afe44453e98  

### Blurイベント
ない。  
TABキーを押させたり、bodyをクリックしたり、JavaScriptを実行して対応。  
bodyクリックだと怪しいかも、変なところにクリックしている。

### 各ブラウザのビルド
macで使う場合。  
chrome: npm i -D chromedriver
safari: Safariの開発メニューからリモートオートメーションをチェック
firefox: npm i -D geckodriver or  https://github.com/mozilla/geckodriver/releases/ からドライバーをDL、PATHを通す。  
