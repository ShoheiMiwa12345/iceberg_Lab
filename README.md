# Apache Iceberg（Pyiceberg）イネーブルメントガイド

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

URL: http://localhost:19120/ui/

説明: Nessieカタログのブラウザインターフェース、リポジトリやブランチの管理ができます

これらのURLはdocker-compose.ymlで指定されたポート設定に基づいています。コンテナが正常に起動していれば、これらのURLでアクセスできます。

# Iceberg テーブルの基本操作

docker-compose が終了したら notebook をブラウザ上で開きます。記述済みの **Pyiceberg1.ipynb**　を上から Shift + Enter を押下し、実行してきます。 

notebook で複数の処理が含まれています。

## 1. 必要なライブラリのインストール
## 2. Nessie カタログへの接続
## 3. 名前空間の作成
## 4. テーブルスキーマの定義
## 5. Iceberg テーブルの作成
## 6. データの追加
## 7. データの確認
## 8. 異なる日付のデータを追加（パーティション確認用）
## 9. テーブルメタデータの確認
## 10. テーブルプロパティ
## 11. パーティション情報の取得
## 12. スナップショット情報
## 13. マニフェスト情報


# Nessie ブランチ機能の活用

ここからNotebook を離れ、ローカルのbash 上から処理を実行します。

## 1. docker attach nessie-cliを実行します。
以下を実行します。

```bash
docker attach nessie-cli
main> SHOW LOG
```

## 2. 新しいブランチの作成します。
以下を実行します。

```bash
main> CREATE BRANCH feature_branch
例) Created BRANCH feature_branch at 20e572c04f366464ab65245cd1794a35484ec2e77127434dbd1e14f12f5f468e
```

## Notebook での再操作

先ほどの ipnyb ファイルの続きを実行します。

```python
# Nessie REST Catalog に接続（feature_branch）
catalog = load_catalog(
    "nessie",
    **{
        "uri": "http://nessie:19120/iceberg/feature_branch/",
    }
)
```

## 新しいブランチへのデータ追加
## 結果の確認
## ブランチのマージ

```bash
# Nessie CLI上で実行
main> merge feature_branch demo.user_table_with_date_partition 

例）Target branch main is now at commit 9fdbb88f58ca8bb73e4dd7293ab5159e1340811755976cdc096bbd3199c5f00c```


## マージ後のメインブランチ確認
## データの確認
table = catalog.load_table("demo.user_table_with_date_partition")
result = table.scan().to_arrow()
df = result.to_pandas()
df = df.sort_values(by="id", ascending=True)
print("データの参照結果")
print(df)
```

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
