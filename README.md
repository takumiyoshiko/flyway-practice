# flyway-practice

- flyway練習用レポジトリ
- docker + postgreSQLで環境を用意

## よく使用するコマンド

### flyway migrate

特定のディレクトリ配下のSQLファイルを再起的に探索し、順にSQLを実行する。

実行されたマイグレーションファイルは`flyway_schema_history`テーブルに登録される。

- マイグレーションファイルのフォーマット
  - `X-<Version>__<Description>.sql`
    - XはVもしくはR (Vはデフフォルト、Rはマイグレーションのチェックサムが変わるごとにマイグレーションを適用)
    

### flyway info
実行されたマイグレーションとそのステータスを確認

ステータス (頻出のもののみ抜粋)
- Pending: クラスパスに含まれているが、未実行 (`flyway_schema_history`に未登録) のSQL
- Success: クラスパスに含まれており、実行済み　 (`flyway_schema_history`に登録済み) のSQL
- Failed: 何らかの理由でSQLの適用に失敗
  - チェクサムの検証で失敗
  - SQLの実行で失敗

```
+-----------+---------+-------------+------+---------------------+---------+
| Category  | Version | Description | Type | Installed On        | State   |
+-----------+---------+-------------+------+---------------------+---------+
| Versioned | 1.0     | Initial     | SQL  | 2023-03-18 11:17:10 | Success |
| Versioned | 1.1     | Initial     | SQL  | 2023-03-18 11:52:17 | Success |
| Versioned | 1.2     | initial     | SQL  |                     | Pending |
+-----------+---------+-------------+------+---------------------+---------+
```

### flyway clean

スキーマ内のすべてのオブジェクトが削除される。

cleanできるようにするには`flyway.cleanDisabled=false`に設定する。

## 困った時の対処法

### チェックサムの検証で失敗

エラー例
```
ERROR: Validate failed: Migrations have failed validation
Migration checksum mismatch for migration version 1.1
-> Applied to database : -1158699709
-> Resolved locally    : 821638889
Either revert the changes to the migration, or run repair to update the schema history.
Need more flexibility with validation rules? Learn more: https://rd.gt/3AbJUZE
ERROR: 1
```

原因
- 実行済みのマイグレーションファイルに変更を加えてしまったため

対策
- 実行済みのマイグレーションファイルを元に戻して実行し直す
- `flyway_schema_history`テーブルを削除してからマイグレーションをやり直す
