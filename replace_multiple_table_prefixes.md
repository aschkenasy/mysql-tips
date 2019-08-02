To replace the prefix of multiple tables use

```mysql
SET group_concat_max_len = 5000;
SELECT CONCAT('RENAME TABLE ', GROUP_CONCAT(table_name, ' TO ', REPLACE(table_name, 'pattern_', 'new_prefix')), ';')
       INTO @renametables
FROM information_schema.tables
WHERE table_name LIKE 'pattern_%';

PREPARE stmt FROM @renametables;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```