# Concourse Docker

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Concourse CIをDocker Composeで実行します。

## Concouese CI

Pivotal社が開発したOSSのCIツール。UIとCI環境のポータビリティ性に優れる。

https://concourse-ci.org/

※CIツールを比較検討した結果は[「CIツール選定」](https://nikkeibp.backlog.jp/view/NMO-3)を参照。

### Requirements & Prerequisites

- Linux
  - kernel v3.19 or later [（※参照）](https://concourse-ci.org/install.html)

## Docker Compose

Docker Composeをインストールします。

```
$ curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose

# 確認
$ docker-compose -v
docker-compose version 1.21.0, build ...
```

**補足：**  
※以降の手順はDockerがインストールされていることを前提に記載します。各ディストリビューション毎のパッケージマネージャを使ってインストールしてください。

## StartUp of Concourse CI

本プロジェクトを`git clone`した後に以下を実施します。

### Generating Keys

起動に必要な鍵を生成します。

```
$ ./generate-keys.sh
```

### Run

```
$ docker-compose -f ./docker-compose.yml up -d
```

#### Access

```
http://[ホスト名]:8081
```

## Oparation of Concourse CI

### `fly` Setup

Concourse CI の操作は`fly`コマンドにて実施します。その為、各プラットフォーム（OS）に合わせて実行ファイルをパスに配置します。

#### Download

`http://[ホスト名]:8081`で表示された画面で、プラットフォームのアイコンをクリックするとダウンロードされます。

#### Set to "Path"

`fly`コマンドの実行ファイルを環境変数のPathに通します。（各プラットフォームに応じて実施してください。）`fly`コマンドが実行できるか確認します。

```
$ fly -v
3.10.0
```

### Initial settings

* ログイン

```
fly -t lite login -c http://[ホスト名]:8081
```

* 認証情報追加

```
$ fly -t lite set-team -n main --basic-auth-username [auth_name] --basic-auth-password [auth_password]
```

## Set Pipeline

パイプライン設定は、各Javaプロジェクトの`./ci`以下を参照して、設定をしてください。
