#Soru 4) Merge ile update, delete, insert işlemleri
```SQL

merge `ceren_erdogan.content_category` t
using `ceren_erdogan.content_category_20201222_00_59` s
on t.id = s.id
--update edilen kayıtların gerekli field'larının güncellenmesi
when matched then
  update set cdc_date = s.cdc_date, category = s.category 
--yeni kayıtların target tabloya eklenmesi
when not matched by target then 
  insert (cdc_date, is_deleted, id, category)
  values (s.cdc_date,s.is_deleted,s.id,s.category)
--silinmiş kayıtların is_deleted field'larının true yapılması
when not matched by source then 
  update set is_deleted = true
;
--Karşılaştırma için--
--1.adım: Her 2 tabloya hash değeri atandı
--2.adım: Join yapılan 2 tablonun, hash değerleri bazında birbirine eşit olmayan satırlarının count'u ekrana yazdırıldı.
select count(*) as not_equal_hash from
(select farm_fingerprint(to_json_string(t1)) as hash1, farm_fingerprint(to_json_string(t2)) as hash2
from `dsmbootcamp.ceren_erdogan.content_category_target` as t1
join 
`dsmbootcamp.ceren_erdogan.content_category` as t2
on farm_fingerprint(to_json_string(t1)) = farm_fingerprint(to_json_string(t2)) 
)
where hash1 != hash2
;

```
