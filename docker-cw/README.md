# Dockerメモ

## コンテナをビルド
```
$ docker image build -t flask .
```
<br>

## コンテナイメージを確認
```
$ docker image ls
```
<br>

## コンテナを起動する
```
$ docker run -p 5000:80 -v ${PWD}/app:/app -d flask
```

(他のポート番号と被っている場合は`$ ~ 5000:80 ~`部分を`5001`などよしなに変更)

<br>

## コンテナの起動を確認する
```
$ docker container ls
```
停止しているものも出力させる場合は
```
$ docker container ls -a
```

<br>

## 設定したポートにアクセスする
↑の設定なら http://localhost:5000   

<br><hr>

## コンテナを停止する
```
$ docker stop [コンテナID]
```
(コンテナIDは `$ docker container ls` で出力できる)

<br><hr><br>

## コンテナを再起動する
```
$ docker start [コンテナID]
```
(コンテナIDは `$ docker container ls` で出力できる（２回目）)

<br><hr>

## 構成
```
.
├── Dockerfile
├── README.md
└── app
    ├── main.py
    ├── static
    │   └── style.css
    └── templates
        ├── data.html
        └── index.html
```

<hr><br>

# ↓自分用

## 参考資料

・[DockerでPython実行環境を作ってみる](https://qiita.com/jhorikawa_err/items/fb9c03c0982c29c5b6d5)  
・[DockerでFlaskを実行するコンテナを作る](https://gray-code.com/blog/flask-on-docker/)  

<br><hr><br>

## 資料からのメモ

<br>

### FROM
> 1行目の「FROM python:alpine」は「FROM」を使ってベースになるコンテナイメージを指定しています。  
> 今回は先述の通りDocker HubのPython公式イメージをベースにするため、「python」とタグ「alpine」を「:」で区切って指定します。

<br>

### WORKDIR
> 3行目の「WORKDIR」はコンテナの作業ディレクトリを指定します。  
> 今回はコンテナ内の「/app」を作業ディレクトリとして指定します。

<br>

### COPY
> 5行目の「COPY」は「docker image build」コマンドでイメージを構築するときに、ローカル環境からコンテナ内にファイルをコピーして設置するための指定です。   
> 「COPY」の1つ目にはコピー元のホスト（ローカル環境）ディレクトリ、2つ目にはコピー先のコンテナのディレクトリを指定します。  
> 今回の例であれば、buildコマンドを実行したときに1つ目に指定したホスト（ローカル環境）ディレクトリ「./app」にあるファイルが、コンテナ内のディレクトリ「/app」に複製して設置されます。

<br>

### RUN
> 7行目の「RUN」は「docker image build」コマンドでイメージを構築するときにコンテナ内でシェルコマンドを実行します。  
> 今回はFlaskをインストールするためのpip（Pytyhonのパッケージ管理システム）のコマンド「pip install Flask」を実行する指定になっています。  
> pip自体はベースのイメージ「python:alpine」に含まれているため、こちらはインストール不要で使用することができます。

### CMD
> 最後の行の「CMD」もコンテナ内でシェルコマンドを実行するためのものですが、こちらは「dockeru run」でコンテナが起動したタイミングに実行するコマンドを指定することができます。  
> 上記の設定はLinuxのCUIに置き換えると「python index.py」を実行する指定です。  
> CMDではCUIのコマンド入力で使う半角スペースを「,（コンマ）」に置き換え、それぞれのコマンドやパラメータを「“(ダブルクォート）」で囲みます。

