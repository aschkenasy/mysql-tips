Use hash indexes for equality columns

```mysql
CREATE UNIQUE INDEX orders_uuid USING HASH ON orders (uuid);
```
