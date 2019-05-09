To drop multiple tables with a specific pattern prefix use

```mysql
SET group_concat_max_len = 5000;
SELECT CONCAT('DROP TABLE ', GROUP_CONCAT(table_name), ';')
       INTO @droplike
FROM information_schema.tables
WHERE table_name LIKE 'pattern_%';

PREPARE stmt FROM @droplike;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```