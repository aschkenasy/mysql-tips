For tables that are (almost) always called as part of another record store them in a sorted way

```mysql
CREATE TABLE post_comment (
    post_id BIGINT,
    comment_id BIGINT AUTO_INCREMENT UNIQUE KEY,
    comment TEXT,
    PRIMARY KEY (post_id, comment_id)
);
```