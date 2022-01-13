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
