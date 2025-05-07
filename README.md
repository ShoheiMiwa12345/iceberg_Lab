# Apache Iceberg（Pyiceberg）入門ガイド

## はじめに
Apache Icebergはオープンソースのテーブルフォーマットで、大規模なデータセットを効率的に管理するために設計されています。
このガイドでは、Nessieカタログを使用したIcebergテーブルの作成、データ操作、およびブランチ機能の活用について説明します。

## 環境セットアップ

このガイドでは、以下のコンポーネントで構成される環境を使用します：

- **Nessie**: バージョン管理機能を持つカタログサービス
- **MinIO**: S3互換オブジェクトストレージ
- **Jupyter Notebook**: Python環境でのデータ処理用

## 前提条件

- Docker と Docker Compose がインストールされていること
- 基本的な Python の知識
- データベースとデータ処理の基本的な理解

## 環境構築

`docker-compose.yml` ファイルを使用して、必要なコンポーネントをセットアップします：

環境を起動するには：

```bash
# docker-compose.yml が存在するディレクトリで実行
docker-compose up -d
```

以下はIcebergの開発環境で利用できる各コンポーネントのブラウザURLです：

### Jupyter Notebook

URL: http://localhost:8888

説明: Pythonコードの実行やIcebergテーブルの操作ができるノートブックインターフェース

### MinIO (S3互換ストレージ)

管理コンソールURL: http://localhost:9001

ユーザー名: admin

パスワード: password

説明: オブジェクトストレージの管理画面、バケットやデータファイルの確認ができます

### Nessie UI

URL: http://localhost:19120/

説明: Nessieカタログのブラウザインターフェース、リポジトリやブランチの管理ができます

これらのURLはdocker-compose.ymlで指定されたポート設定に基づいています。コンテナが正常に起動していれば、これらのURLでアクセスできます。

## Iceberg テーブルの基本操作

docker-compose が終了したら notebook をブラウザ上で開きます。記述済みの **Pyiceberg1.ipynb**　を上から Shift + Enter を押下し、実行してきます。 
notebook で複数の処理が含まれています。

✅必要なライブラリのインストール
pyiceberg パッケージをインストールし、Nessie や SQLite、S3 との連携を含むオプションを有効にしています。

✅Nessie Catalog に接続
Nessie REST API を使って Iceberg のカタログに接続しています。

✅名前空間を作成 /  スキーマ、パーティション、ソート定義
Iceberg テーブルの構造を定義します

✅テーブルの作成 / PyArrow テーブルを作成して Iceberg に追加
Iceberg カタログ上に新しいテーブルを作成します

✅テーブルをスキャンして Pandas DataFrame に変換・表示
Iceberg テーブルの内容を取得して追加されたデータを表示します

✅テーブルプロパティ、パーティション仕様、スナップショット情報の取得
メタ情報の取得します。

✅Iceberg のスナップショットメタデータおよびマニフェストファイル情報の確認
履歴・ファイル構成を確認します。

✅Iceberg スキーマの進化
Iceberg テーブルのスキーマを進化（新しい列を追加）

✅追加したデータの削除

## ローカルから Nessie Cli での操作を実行します。

✅Nessie Cli を Docker へアタッチ(コンテナが起動している事をチェック)
docker attach nessie-cli
main> SHOW LOG

✅feature_branch を作成します。
main> CREATE BRANCH feature_branch
出力例) Created BRANCH feature_branch at 20e572c04f366464ab65245cd1794a35484ec2e77127434dbd1e14f12f5f468e

## Notebook での作業を再開します。

✅feature_branch への接続

✅feature_branch 上からテーブルへデータを追加します

✅feature_branch のマージ

## Nessie Cli での操作を実行します。

main> merge feature_branch demo.user_table_with_date_partition 
出力例）Target branch main is now at commit 9fdbb88f58ca8bb73e4dd7293ab5159e1340811755976cdc096bbd3199c5f00c

✅Main ブランチでのデータ確認

このガイドでは、Apache Icebergとnessieカタログを使用した:
テーブルの作成と基本操作
パーティションの活用
テーブルメタデータの分析
ブランチ機能によるバージョン管理
これらの機能により、Icebergは大規模データセットの管理に適した選択肢となります。

## Docker コンテナの削除
Docker のコンテナ削除を行う場合、以下のコマンドを発行します。

docker-compose down -v

## まとめ

このガイドでは、Apache Icebergとnessieカタログを使用した:

1. テーブルの作成と基本操作
2. パーティションの活用
3. テーブルメタデータの分析
4. ブランチ機能によるバージョン管理
これらの機能により、Icebergは大規模データセットの管理に適した選択肢となります。

## 参考資料

Pyiceberg API は以下から確認し、必要に応じて追加の操作を行ってください。

- [Apache Iceberg 公式ドキュメント](https://iceberg.apache.org/)
- [Project Nessie 公式ドキュメント](https://projectnessie.org/)
- [PyIceberg ドキュメント](https://py.iceberg.apache.org/)
