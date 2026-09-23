# Nexus Repository Manager で リポジトリを作成する

Nexus Repository Manager (以下NXRM) は複数のフォーマットに対応したリポジトリ管理のためのアプリケーションで、Community 版であれば無料で利用可能です。

ここでは NXRM を利用し Docker リポジトリを作成する手順に関して説明します。

## 参考

- [Nexus Repository Manager 3のDocker Registryを試す](https://qiita.com/fukasawah/items/48330564736d3368b632)
- [NexusでプライベートDockerリポジトリを作成](https://qiita.com/Imasug/items/71d2ff21c7d1d3454bff)

## System Requirements

- OS : CentOS8
- CPU : 4コア以上
- メモリ : 4G以上 ( 以下 System Requirements では 8G となってるが 4G でもなんとか動作した)
- ディスク : 20G 以上

https://help.sonatype.com/repomanager3/installation/system-requirements

メモリやディスクが不足している場合、起動しない＆エラーログも吐かれない状態になるので注意

## インストール

別途 Ansible playbook を作成したのでこれを利用してインストールする。

## NXRMへのアクセス

http://インストール先ホスト/ にアクセスすると NXRM にアクセスできる。

管理者アカウントのパスワードは以下に記載されているので、これを利用してログインする。

```
/opt/sonatype-work/nexus3/admin.password
```

## 匿名アクセスの許可

Repository Manager の画面が表示されたら右上の [`Sign in`] から上述のパスワードを利用してログインする。
初回ログイン時にパスワードのリセットを要求されるのでadmin アカウントのパスワードを適宜再設定する。
この後匿名アクセスを許可するか問い合わせてくるので、運用方針を踏まえて、許可するかどうかを決定する。

## リポジトリ

扱えるリポジトリのフォーマットは [Formats - Nexus Repository Manager 3](https://help.sonatype.com/repomanager3/formats) を参照。

リポジトリを作成する前にいくつか NXRM で考慮すべき点について述べる。

### ストレージ

### リポジトリタイプ

リポジトリには次のタイプがある

- `proxy`  
  外部リポジトリのプロキシとして動作する。Docker HubやMaven Central Repository等に社内からアクセスする場合にプロキシリポジトリを作成しておくことで、トラフィックの削減やデータ取得のためのレイテンシを少なくする事が可能になる。
- `group`  
  複数のリポジトリをまとめる。
  proxyとhostedをまとめて、社内からのリポジトリ登録はhosted,参照はgroupとすることで、外部リポジトリにあるリソースと独自リポジトリにあるリソースを透過的にアクセスする事が可能になる。
- `hosted`  
  独自のリポジトリを作成する。自社ライブラリの登録や利用のために作成する場合にはhostedを利用する事になる。

## Dockerリポジトリの作成

リポジトリ

1. 上部メニューの歯車アイコン(`Server administration and configuration`)をクリックする。
2. 管理画面に遷移するので、左メニューの [`Repositories`] をクリックする。
3. リポジトリ一覧が表示されるので [`Create Repository`] をクリックする。
4. `docker (hosted)` を選択する。
5. 設定画面に遷移するので以下を入力する。
   - `Name`: 適当な識別子
   - `Online`: チェックする
   - `Enable Docker V1 API`: チェックする  
      古い Docker クライアントやツールとの互換性が必要な場合チェックする。社内のセルフホスト環境では、互換性の観点から有効にするのが推奨かも。外部アクセス可能な環境の場合は、セキュリティ含めて要件等。  
     上記以外はデフォルトで。
6. 設定したら[`Create repository`]ボタンクリックします。

※ SSLを利用するかに関するガイドは [SSL and Repository Connector Configuration](https://help.sonatype.com/repomanager3/formats/docker-registry/ssl-and-repository-connector-configuration) を参照。  
※ SSL接続させるべきですが、NXRMで直接SSL接続を確立させるのではなく、必要に応じて前段のProxy(nginx)でHTTPS化するものとします。
