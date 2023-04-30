## 执行计划

[参考官方文档](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html)

关键字 `explain`
```sql
explain select * from order_info where customer_name = 'test'
```

执行计划输出的内容

| 字段                                                         | JSON 名称       | 解释                                                         |
| :----------------------------------------------------------- | :-------------- | :----------------------------------------------------------- |
| [`id`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_id) | `select_id`     | 查询ID                                                       |
| [`select_type`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_select_type) | None            | 查询类型                                                     |
| [`table`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_table) | `table_name`    | 查询的是哪个表，实表会显示表名，子查询会显示依赖的是哪个查询语句（查询ID） |
| [`partitions`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_partitions) | `partitions`    | 分区信息，命中的是哪个分区                                   |
| [`type`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_type) | `access_type`   | The join type                                                |
| [`possible_keys`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_possible_keys) | `possible_keys` | 可能使用到的索引                                             |
| [`key`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_key) | `key`           | The index actually chosen                                    |
| [`key_len`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_key_len) | `key_length`    | The length of the chosen key                                 |
| [`ref`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_ref) | `ref`           | The columns compared to the index                            |
| [`rows`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_rows) | `rows`          | Estimate of rows to be examined                              |
| [`filtered`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_filtered) | `filtered`      | Percentage of rows filtered by table condition               |
| [`Extra`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain_extra) | None            | Additional information                                       |

- ID

  查询ID。ID 相同的时候，执行顺序由上而下；ID 不同的时候，执行顺序为ID越大越先执行。

- select_type

  可以是下表显示的任何类型。JSON

| `select_type` 值                                             | JSON 名称                    | 解释                                                         |
| :----------------------------------------------------------- | :--------------------------- | :----------------------------------------------------------- |
| `SIMPLE`                                                     | None                         | Simple [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html) (not using [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html) or subqueries) |
| `PRIMARY`                                                    | None                         | Outermost [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html) |
| [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html) | None                         | Second or later [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html) statement in a [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html) |
| `DEPENDENT UNION`                                            | `dependent` (`true`)         | Second or later [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html) statement in a [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html), dependent on outer query |
| `UNION RESULT`                                               | `union_result`               | Result of a [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html). |
| [`SUBQUERY`](https://dev.mysql.com/doc/refman/5.7/en/optimizer-hints.html#optimizer-hints-subquery) | None                         | First [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html) in subquery |
| `DEPENDENT SUBQUERY`                                         | `dependent` (`true`)         | First [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html) in subquery, dependent on outer query |
| `DERIVED`                                                    | None                         | Derived table                                                |
| `MATERIALIZED`                                               | `materialized_from_subquery` | Materialized subquery                                        |
| `UNCACHEABLE SUBQUERY`                                       | `cacheable` (`false`)        | A subquery for which the result cannot be cached and must be re-evaluated for each row of the outer query |
| `UNCACHEABLE UNION`                                          | `cacheable` (`false`)        | The second or later select in a [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html) that belongs to an uncacheable subquery (see `UNCACHEABLE SUBQUERY`) |
