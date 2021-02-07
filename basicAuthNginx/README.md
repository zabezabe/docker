# basic Authentication + Nginx
Basic認証を行っている仮サーバのコンテナ


### 前提:
- dockerコマンドが実行できる
- Dockerfileの存在するディレクトリに移動済み

### イメージビルド
```bash
docker build -t BasicAuthNginx .
```

### イメージビルド確認
```bash
docker images
```
- [ ] ビルドしたタグのイメージが作成されていることを確認する  
上記では`BasicAuthNginx`が該当する

### コンテナを起動する
```bash
docker container run -d -p 8080:80 BasicAuthNginx
```

### コンテナが起動中であることを確認
```bash
docker ps
```
- [ ] `IMAGE`の欄に起動したコンテナのイメージタグが存在すること

### 疎通確認（正常系）  
正しいユーザ情報を指定してリクエスト
```bash
curl -v -u admin:password localhost:8080
```
<details><summary>レスポンスサンプル</summary><div>

```
$ curl -v -u admin:password localhost:8080
*   Trying 127.0.0.1:8080...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8080 (#0)
* Server auth using Basic with user 'admin'
> GET / HTTP/1.1
> Host: localhost:8080
> Authorization: Basic YWRtaW46cGFzc3dvcmQ=
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Server: nginx/1.18.0
< Date: Sun, 07 Feb 2021 16:51:33 GMT
< Content-Type: text/html
< Content-Length: 612
< Last-Modified: Thu, 29 Oct 2020 15:25:17 GMT
< Connection: keep-alive
< ETag: "5f9adedd-264"
< Accept-Ranges: bytes
<
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
* Connection #0 to host localhost left intact
```
</div></details><br>

### 疎通確認（異常系）  
ユーザ情報を指定せずリクエスト
```bash
curl -v localhost:8080
```
<details><summary>レスポンスサンプル</summary><div>

```bash
curl -v localhost:8080
*   Trying 127.0.0.1:8080...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8080 (#0)
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 401 Unauthorized
< Server: nginx/1.18.0
< Date: Sun, 07 Feb 2021 17:08:20 GMT
< Content-Type: text/html
< Content-Length: 179
< Connection: keep-alive
< WWW-Authenticate: Basic realm="Please enter ID and password."
<
<html>
<head><title>401 Authorization Required</title></head>
<body>
<center><h1>401 Authorization Required</h1></center>
<hr><center>nginx/1.18.0</center>
</body>
</html>
* Connection #0 to host localhost left intact
```
</div></details>

***

## 後片付け
コンテナの停止
```bash
## 動作中のコンテナIDを取得する
docker ps
## コマンドで取得したコンテナIDを指定し停止する
docker stop ${CONTAINER ID}
```
コンテナの再起動
```bash
## 停止させたコンテナIDを取得する
docker ps -a
## コマンドで取得したコンテナIDを指定し起動する
docker start ${CONTAINER ID}
```
コンテナの削除
```bash
## 動作中であればコンテナを停止する
docker stop ${CONTAINER ID}
## 事前に停止してあればコンテナIDを取得する
docker ps -a
## コンテナを削除する
docker rm ${CONTAINER ID}
```
コンテナイメージの削除
```bash
## 
docker images
## 
dicker rmi -f ${IMAGE ID}
```
