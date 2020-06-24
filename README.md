# Drone.io

## 準備

### Host マシンの hosts を編集

docker を動かしている Host マシンの hosts に以下を追加する

```hosts
# 192.168.X.XXX is host machine ip
192.168.X.XXX ci-cd.internal
```

### Host マシンの環境変数を設定する

docker を動かしている Host マシンの環境変数に以下を追加する

| 変数         | 値            | 説明             |
| ------------ | ------------- | ---------------- |
| CICD_HOST_IP | 192.168.X.XXX | ホストマシンのIP |

## Gitea との連携

* [マニュアル](https://docs.drone.io/server/provider/gitea/)

### docker の Host に以下の環境変数を設定する

* DRONE_GITEA_CLIENT_ID
  * Gitea に Drone.io がアクセスするためのクライアントのID
  1. Gitea で drone 用のユーザーを作成する
![image](readme_img/2020-06-23-22-45-04.png)

  2. 作成したユーザーでログインする
![image](readme_img/2020-06-23-22-51-36.png)

  3. 設定をクリックする
![image](readme_img/2020-06-23-22-52-39.png)

  4. OAuth Application を作成する
![image](readme_img/2020-06-24-00-44-29.png)

  5. OAuth Application のクライアントIDを DRONE_GITEA_CLIENT_ID に設定する
![image](readme_img/2020-06-24-00-45-07.png)

* DRONE_GITEA_CLIENT_SECRET
  * Gitea に Drone.io がアクセスするためのクライアントシークレット
  * 同様に作成された OAuth Application のクライアントシークレットを DRONE_GITEA_CLIENT_SECRET に設定する

* DRONE_RPC_SECRET
  * Runner と連携するためのシークレット
  * 以下のコマンドを実行した結果を DRONE_RPC_SECRET に設定する

  ```bash
  openssl rand -hex 16
  ```

  * 実行するときは以下のようなオンラインで Unix コマンドを実行できるサイトを使用すると便利
    * [Compile Bash Online - Run Bash Online](https://www.tutorialspoint.com/execute_bash_online.php)

### Gitea と接続する

Gitea に作成したアカウントと連携をする処理を行う

1. `http://ci-cd.internal:21080` にアクセスする
2. `http://ci-cd.internal:20080/user/login` にリダイレクトされるので作成した Drone.io との連携ユーザーでログインする
![image](readme_img/2020-06-23-23-23-57.png)
  作成した OAuth Application を作成したユーザーでログインをしているとこの画面はスキップされる

3. アクセス権の許可ダイアログが出るので許可を行う
![image](readme_img/2020-06-24-00-57-26.png)

4. 
