# HBase Note

HBase is a NoSQL database, a distributed, scalable, big data store running on top of HDFS, which is modeled after Google's BigTable.

Use HBase when you need random, realtime read/write access to your Big Data. 

HBase is used to host very large tables - billions of rows X millions of columns.

## Basics

- Compression
- In-memory operations:
  - MemStore
  - BlockCache
- Pursues efficiency of analysis. A great amount of data is stored redundantly.

![hbase-access-interface.png](img/hbase-access-interface.png)

![hbase-data-product-storage.png](img/hbase-data-product-storage.png)

### Namespace

- Namespace命名空间指对一组表的逻辑分组，类似RDBMS中的database，方便对表在业务上划分。
- Namespace特性是对表资源进行隔离的一种技术，隔离技术决定了HBase能否实现资源统一化管理的关键，提高了整体的安全性。

---

## Architecture

![hbase-architecture.png](img/hbase-architecture.png)

---

## Coding

- `list`: List all created tables in the current DB.
- `create '<table_name>', '<column_family_name>'`
- `describe '<table_name>'`: Check basic info of the table.
- `scan '<table_name>'`: Check all data of a table.
- `put '<table_name>', '<row_key_value>', '<column_family_name>:<column_name>', '<cell_value>'`: Insert data.
- `get '<table_name>', '<row_key_value>'`: Check data of a row.
- `get '<table_name>', '<row_key_value>', '<column_family_name>:<column_name>`: Check data of a cell.
- `truncate '<table_name>'`: Delete data in the table.
- Delete a table: `disable '<table_name>'` and then `drop '<table_name>'`

Take "student" table as an example:

| id | name     | gender | age |
| :--| :--------| :------| :---|
| 1  | Zhangsan | F      | 23  |
| 2  | Lisi     | M      | 24  |

1. `create 'student', 'info'`
2. `put 'student', '1', 'info:name', 'Zhangsan'`
3. `put 'student', '1', 'info:gender', 'F'`
4. `put 'student', '1', 'info:age', '23'`
5. `put 'student', '2', 'info:name', 'Lisi'`
6. `put 'student', '2', 'info:gender', 'M'`
7. `put 'student', '2', 'info:age', '24'`
