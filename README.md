# flyway-practice

## コマンドの説明

### flyway info
マイグレーションとそのステータスを確認

ステータス (頻出のもののみ抜粋)
- Pending: クラスパスに含まれているが、未実行 (“flyway_schema_history`に未登録) のSQL
- Success: クラスパスに含まれており、実行済み　 (“flyway_schema_history`に登録済み) のSQL
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

### flyway migrate

### flyway clean
