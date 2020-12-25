#Soru 3) Bu çalışmada çıkarmak istediğimiz bilgi, günün her bir dakikası için aktif kullanıcı sayısının hesaplanması.
```SQL

--1.adım: 23:10:00 start_time olarak belirlendi.
--2.adım: Yeni oluşturulan active_users tablosuna her dakikada (set ile 1 dakika ekleyerek), 5 dakikada bir aktif kullanıcı sayısı eklendi

declare start_time timestamp default "2020-03-03 23:10:00";

while start_time < "2020-03-03 23:59:00" do
  insert into `ceren_erdogan.active_users` (view_period, active_user_count)
    select timestamp_add(start_time,interval 5 minute) view_period,approx_count_distinct(deviceid) active_user_count 
    from ceren_erdogan.pageview
    where view_ts between start_time and timestamp_add(start_time,interval 5 minute);
    set start_time = timestamp_add(start_time, interval 1 minute);
end while;

select * from `ceren_erdogan.active_users`
order by view_period
limit 10

```
