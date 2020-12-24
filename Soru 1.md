# Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım. 

```SQL
--1.adım: sport ve country bazında gruplanıp ülkelerin madalya sayıları bulundu.
--2.adım: soruda istenilen sıradaki ülkelerin bulunması için lead fonksiyonuı spor bazında gruplandı.
--3.adım: gruplanılan sporların sadece ilkini yazdırmak için kayıtlar row_number ile numaralandırıldı.
--4.adım: row_number = 1 olanlar getirilerek sonuca ulaşıldı
--Sorun: select de row_number kullanmadan row_number = 1 yapamıyorum. O yüzden ekrana yazdırmak zorunda kaldım.
select * from(
with gold_medals as
(
select
sport,
country,
count(*) as medal_count
from `dsmbootcamp.sample.summer_medals`
where year >= 1980
group by sport,country
order by sport,medal_count desc
)
select
sport,
country as first,
-- medal_count,
lead(country,2) over (partition by sport order by medal_count desc) as third,
lead(country,4) over (partition by sport order by medal_count desc) as fifth,
row_number() over(partition by sport order by medal_count desc) as row_number
from gold_medals
order by sport, medal_count desc
)
where row_number = 1

```
