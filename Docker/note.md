## Docker入門系記事
読んだ。  
https://www.enisias.cloud/docker/33/  
https://knowledge.sakura.ad.jp/13265/  

## イメージ
コンテナを作る元、自分で作るかDockerHubからダウンロードする  

## コンテナ
イメージから作成するコンテナ、サーバ的なもん？  

## Dockerfile
イメージの設計書てきなもの？  
このファイルの設定を元にイメージを作成できる  

## コマンド
```
#イメージ一覧
docker images

#コンテナ一覧
docker ps
#コンテナ一覧（停止中も含める）
docker ps -a

#コンテナを作成
docker create イメージ名
#コンテナ名を指定して作成
docker create --name コンテナ名 イメージ名

#コンテナ起動
docker start ID or コンテナ名

#コンテナにログイン
docker exec -it コンテナ名 bash

#コンテナ終了
docker stop ID or コンテナ名
docker kill ID or コンテナ名
killは即時終了、多分

#コンテナ削除
docker rm ID or コンテナ名

#イメージからコンテナを起動
docker run hello-world

#イメージのダウンロードだけ
docker pull?hello-world

#イメージの削除
docker rmi ID or イメージ名

#Dockerfileからイメージを作成
docker build Dockerfileのパス

#コンテナからホストへのファイルコピー
docker cp <コンテナID>:/etc/my.cnf my.cnf
```

### docker run オプション
- -d: バックグラウンドで行う。
- -p: ポートの指定。ローカルポート:コンテナポート
- -v: ローカルマシンのディレクトリマウント
	- docker run -v "マウントするディレクトリ:マウント先のパス"

## Docker Compose
複数のコンテナをまとめる。  

```
#docker-compose.ymlの設定に従ってコンテナを作成・起動する
docker-compose up -d

#コンテナ削除
docker-compose down

#Dockerfileから作成する場合
docker-compose up -d --build

#開始
docker-compose start

#終了
docker-compose stop
```