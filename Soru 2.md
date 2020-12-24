# Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
--1.adım: atletlerin hangi yıllarda hangi spor dalında madalya aldıklarını buldum.
--2.adım: çıkan tablo üzerinde önce atlete sonra spor dalına göre gruplayarak madalya alınan ilk yılı getirdim.
--3.adım: current row’dan bir sonraki yıla second_year, 2 sonraki yıla da third_year dedim.
--4.adım: aralarındaki fark 4 olmalı ki peş peşe alınan 3 yıl veya daha fazlasını bulabileyim.
select * from (
with medals as(
select year,athlete,sport
from `dsmbootcamp.ceren_erdogan.summer_medals`
where athlete is not null and year >= 1980
order by athlete)
select athlete,sport,
first_value(year) over(partition by athlete,sport order by athlete,year) as first_year,
last_value (year) over(partition by athlete,sport order by athlete,year rows between current row and 1 following) as second_year,
last_value (year) over(partition by athlete,sport order by athlete,year rows between current row and 2 following) as third_year,
from medals
order by athlete,sport
)
where second_year - first_year = 4 and third_year - second_year = 4

```
