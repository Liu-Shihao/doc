在Oracle数据库中，要向表中添加新字段，你可以使用`ALTER TABLE`语句。以下是添加新字段的一般语法：

```sql
ALTER TABLE table_name
ADD column_name datatype;
```

其中：
- `table_name`是要修改的表的名称。
- `column_name`是新字段的名称。
- `datatype`是新字段的数据类型。

例如，如果你想在名为`employees`的表中添加一个名为`email`的新字段，其数据类型为`VARCHAR2(100)`，可以使用以下语句：

```sql
ALTER TABLE employees
ADD email VARCHAR2(100);
```

请注意，这只是一个简单的示例。在实际使用中，你可能还需要考虑其他因素，例如默认值、约束等。如果要为新字段添加默认值，可以使用`DEFAULT`关键字：

```sql
ALTER TABLE employees
ADD salary NUMBER DEFAULT 0;
```

此外，如果需要添加多个字段，可以在单个`ALTER TABLE`语句中使用逗号分隔它们，如下所示：

```sql
ALTER TABLE table_name
ADD column1 datatype1,
    column2 datatype2,
    ...
    columnN datatypeN;
```

请确保在执行此类操作之前进行备份，以防不时之需。
当向Oracle表中添加`published_time`、`modify_time`、`version`三个字段时，你可以使用以下SQL语句：

```sql
ALTER TABLE your_table_name
ADD published_time TIMESTAMP,
    modify_time TIMESTAMP,
    version NUMBER;
```

请将`your_table_name`替换为实际表的名称。上述语句假定`published_time`和`modify_time`使用`TIMESTAMP`数据类型，而`version`使用`NUMBER`数据类型。如果需要根据实际需求选择不同的数据类型，可以进行调整。

如果你希望为这些字段添加默认值，可以通过使用`DEFAULT`关键字来实现。例如，为`published_time`和`modify_time`添加当前时间的默认值：

```sql
ALTER TABLE your_table_name
ADD published_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modify_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    version NUMBER;
```

上述语句中使用了`CURRENT_TIMESTAMP`函数来设置默认值为当前时间。

确保在执行这样的ALTER TABLE语句之前，先备份数据库或表，以便在需要时进行恢复。