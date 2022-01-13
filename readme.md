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

For tables that are (almost) always called as part of another record store them in a sorted way

```mysql
CREATE TABLE post_comment (
    post_id BIGINT,
    comment_id BIGINT AUTO_INCREMENT UNIQUE KEY,
    comment TEXT,
    PRIMARY KEY (post_id, comment_id)
);
```

Lateral joins for complex queries that can't use limit

```mysql
SELECT users.*, recent_logins.*
FROM users
         LEFT JOIN LATERAL (
    SELECT *
    FROM logins
    WHERE logins.user_id = users.user_id
    ORDER BY created_at DESC
    LIMIT 5
    ) AS recent_logins USING (user_id)
```

Design multi-column indexes for their order purpose

```mysql
CREATE INDEX stock_high_performer ON stocks (
    stock_value DESC, created_at ASC                                        
);
```

Which allows for optimized query of
```mysql
SELECT *
FROM stocks
ORDER BY stock_value DESC, created_at ASC
LIMIT 30;
```
