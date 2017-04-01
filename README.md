# 何のためのもの？

ローカルでPHPアプリ(※)を動かす為の環境サンプルです。
※SPA前提なのでフロントエンドとバックエンドを別コンテナにしてます。

# やってること(ざっくり)

1. VagrantでVirtualBox上にゲストOS(CentOS7)を立てる
1. ゲストOSをにDockerをインストールする
1. 以下3つのDockerコンテナを立てる
    - フロントエンド用コンテナ(nginx)
    - バックエンド用コンテナ(nginx+php-fpm)※phpは7
    - DB(mysql5.7)を立てる

# 各ファイルの定義

## Vagrantfile

ゲストOSの設定と、ゲストOSに必要なパッケージのインストールを行っています。
Dockerの他にもいくつかインストールしてますが、元々想定していたのが、

- フロントエンド:Vue.js
- バックエンド:Laravel

だったので、ゲストOS上から

- artisan
- composer
- npm(vue-cli含む)

を使えるようにするための設定を入れてます。

Dockerを使ってるのでCentOS上には極力何も入れない方が良いと思いますがお好みで書き換えて下さい。

## frontend

フロントエンドアプリのルートディレクトリ(ドキュメントルートもここ)です。

## backend

バックエンドアプリのルートディレクトリ(ドキュメントルートもここ)です。

## docker-compose

docker-compose関連ファイルが入ってます。

## docker-compose/docker-compose.yml

docker-compose用のymlです。

## docker-compose/frontend

フロントエンドアプリのnginx設定ファイルが入ってます。
ドキュメントルートを変える時などはこちらのファイルを変更してください。

## docker-compose/backend

バックエンドアプリのnginx、php-fpm設定ファイルが入ってます。
ドキュメントルートを変える時などはこちらのファイルを変更してください。

# 使い方

1. vagrantが使える状態にする

    念のため

    ```
    vagrant plugin install vagrant-vbguest
    ```

    もしておいたほうが良いです。

1. ルートディレクトリにパスを合わせて

    ```
    vagrant up
    ```

    を実行する
    ※初回実行時は色々インストールするので15分くらいかかります

1. それぞれアクセスしてみる

    - フロントエンド
        - http://192.168.33.120
    - バックエンド
        - http://192.168.33.120:81
    - DB

        項目 | 値
        --- | ---
        host | 192.168.33.120
        user| root
        pass | root

以上