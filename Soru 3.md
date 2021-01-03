#Soru 3) Bu çalışmada çıkarmak istediğimiz bilgi, günün her bir dakikası için aktif kullanıcı sayısının hesaplanması.
```SQL

with hyper_count as(
SELECT * FROM(
SELECT timestamp_trunc(view_ts, minute) minute, HLL_COUNT.init(deviceid) users
FROM ceren_erdogan.pageview
GROUP BY minute)
ORDER BY minute), 

five_minute_window AS (
  SELECT
    minute,
    ARRAY_AGG(users) OVER (ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) five_minute_users
  FROM hyper_count)
  
SELECT
  minute,(
  SELECT
    HLL_COUNT.merge(users)
  FROM
    UNNEST(five_minute_users) users) user_count
FROM
  five_minute_window
ORDER BY
  minute;

```
