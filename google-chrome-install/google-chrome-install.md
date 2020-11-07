https://qiita.com/pyon_kiti_jp/items/e6032eb6061a4774aece

UbuntuにChromeをインストールする
Ubuntu
More than 1 year has passed since last update.
はじめに

UbuntuにChromeをインストールした手順書です。
Ubuntuには、デフォルトのWebブラウザとして「Mozilla Firefox」がプレインストールされているが、Chromeを使いたい場合は、多少手間ではあるが、新規にインストールする事になります。
環境

Utuntu16.04.5 LTS
手順

先ずは、自分の環境に、確かにChromeがインストールされていない事を確認する
```
apt list --installed google*
```
パッケージリストにChromeの情報が記述されていないため、自分で追加するところから行わないといけない
Bシェルを実行して、google.listの最終行に、deb～で始まる１行を追加している
```
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
```
上のコマンドで、確かに登録された事を確認する
```
ls -l /etc/apt/sources.list.d
cat /etc/apt/sources.list.d/google.list
```
公開鍵をダウンロードして、更に、公開鍵をapt-keyで登録する
wgetはHTTP経由でダウンロードするコマンド。ダウンロードした、公開鍵をapt-keyコマンドに受け渡している
```
sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```
ここで、パッケージリストを最新の状態にする
```
sudo apt update
```
ダウンロードをする前に、リモートリポジトリに、ダウンロードしたいパッケージが確かに存在する事を確認する
リモートリポジトリとは、ダウンロード元、アプリの配布元サーバーの事。ここにあるパッケージが、aptコマンドでダウンロードできるという事
```
apt list google*
```
ここまでが前準備で、いよいよ本番作業、インストールをする（結構時間が掛かります）
```
sudo apt-get install google-chrome-stable
```
自分の環境に、確かにインストールされた事を確認する
```
apt list --installed google*
```
参考記事

https://qiita.com/spiderx_jp/items/e6189a736ddec14ffa23
