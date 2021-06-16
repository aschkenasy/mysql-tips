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
