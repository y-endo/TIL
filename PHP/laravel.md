## Laravel
### インストール
composerコマンドからインストールできる。  
```
composer create-project laravel/laravel プロジェクト名 --prefer-dist
```
--prefer-dist オプションをつけると圧縮版がインストールされるのでオススメ。  

### 初期設定
諸々英語になっているので日本語に言語設定を変更する。  
config が設定ファイルになっている。  
**config/app.php**
timezone => 'Asia/Tokyo'
locale => 'ja'

**config/database.php**
charset => 'utf8'

デバッグバーをインストールする。  
```
composer require barryvdh/laravel-debugbar --dev
```

デバッグ中はデバッグバーが出るので本番公開するとやばい。  
.envファイルの切り替えをわすれない。  
APP_DEBUG=true → false

**データベースの設定**
.envの「DB_**」を変更する。
.envに設定したとおりにDBやユーザーを作成したら、以下コマンドを実行。  
```
php artisan migrate
```

### キャッシュの削除
コマンドからlaravelのキャッシュを削除できる。  
```
php artisan cache:clear
php artisan config:clear
```

### artisanコマンド
artisanコマンドで色々できる。  
キャッシュのクリアだったり、簡易サーバーを立ち上げたり。  
Modelを作成するコマンドとか
```
php artisan make:model Models/Test
```

### マイグレーション
DBテーブルの履歴管理。  
テーブルを作成したり、または削除したりなどの履歴を確認できる。  
**命名規則** モデルは単数形、マイグレーションは複数形で作成する。  
Testモデル testsマイグレーション  
Personモデル peopleマイグレーション  
作成したマイグレーションは database/migrations の中に入っている。  
マイグレーションからDBにテーブルを作成できる。  
```
php artisan migrate
```