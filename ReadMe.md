# はじめに

```
sudo apt install npm
```

```
bash script/docker_cleanup_all.sh

bash script/cleanup_repository.sh

docker network create --driver bridge --subnet=162.18.1.0/24 --gateway=162.18.1.1 oss_network

docker compose -f Haru-OSS/docker-compose.yaml up -d

```

# GitHubへコミットする方法

## 1. サーバでリポジトリを初期化する

サーバ内でリポジトリにしたいディレクトリに移動する。

```
$ cd /path/to/your/files
```
次にGitリポジトリを初期化する。
```
$ git init
```
## 2. GitHubリポジトリをリモートに設定する

GitHubのリポジトリURLをコピーする。（例: https://github.com/username/my-repo.git）。
そのURLを使ってリモートリポジトリを設定する。

```
$ git remote add origin https://github.com/username/my-repo.git
```

## 3. ファイルをステージングしてコミットする

リポジトリ内のファイルをすべてステージングする。

```
$ git add .
```
ファイルをコミットする。

```
$ git commit -m "Initial commit"
```

## 4. GitHubにファイルをプッシュする

リポジトリの内容をGitHubにプッシュする。
```
$ git branch -M main
```
表示
```
$ git branch
```
変更

```
$ git checkout test-branch
```
-M オプションは、ブランチの名前を強制的に変更するためのもの

```
$ git push -u origin test-branch
```
## 5. （必要に応じて）GitHubへの認証

GitHubのパーソナルアクセストークンを入力する必要がある。
トークンの取得方法

 GitHubの「Settings」 > 「Developer settings」 > 「Personal access tokens」 > 「Tokens (classic)」 > 「Generate new token」で新しいトークンを作成。
トークンを保存し、プッシュ時に使用する。


# 認証

GitHubでは、2021年8月13日以降、HTTPSでのリポジトリアクセスにパスワードを使用する認証が廃止され、代わりにPersonal Access Token (PAT) を使用する必要があります。
以下の手順で認証エラーを解決できます。

解決方法: Personal Access Token を使う
## 1. Personal Access Token を作成

GitHubアカウントにログインします。
右上のプロフィールアイコンをクリックし、Settings を選択します。
左側のメニューで Developer settings → Personal access tokens → Tokens (classic) を選択します。
Generate new token ボタンをクリックします。

必要なスコープを選択:
repo（プライベートリポジトリにアクセスする場合）
read:org（組織リポジトリが必要な場合）

Generate token をクリックします。
生成されたトークンをコピーします（この画面でしか表示されないので忘れずに保存してください）。

## 2. Git リポジトリでトークンを使用

git push コマンドを実行したときに、パスワードの代わりにトークンを入力します。

例:
Username for 'https://github.com': halchil
Password for 'https://halchil@github.com': <Your-Token>

## 3. Gitの認証情報をキャッシュ

毎回トークンを入力するのを防ぐため、認証情報を保存できます。
方法1: Git Credential Managerを使用

Git for WindowsやGitHub CLIを使用している場合は、Git Credential Managerがインストールされている可能性があります。次のコマンドでトークンを保存できます:

```
git credential-manager-core configure
```

方法2: HTTPS URLに直接トークンを埋め込む（推奨されない）

リモートURLを以下の形式に変更します。ただし、セキュリティ上推奨されません。

```
git remote set-url origin https://<Your-Token>@github.com/halchil/Haru-OSS.git
```

方法3: SSH を使った認証

SSHキーを生成します（まだ作成していない場合）。

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

公開鍵をGitHubに登録します。

GitHubのSettings → SSH and GPG keys → New SSH key から登録。

リモートURLをSSH形式に変更します。

```
git remote set-url origin git@github.com:halchil/Haru-OSS.git
```

再度プッシュ

トークンまたはSSHを設定した後、もう一度以下を試します

```
git push -u origin test-branch
```


