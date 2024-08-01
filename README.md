# Домашнее задание к занятию «Индексы» - Мельник Юрий Александрович

## База данных Sakila содержит 16 основных таблиц, описывающих различные аспекты компании по прокату DVD-дисков. Ниже приведен список этих таблиц:

   - actor – информация об актерах, которые участвовали в фильмах.
   - address – информация об адресах, в которых зарегистрированы клиенты компании.
   - category – список категорий, к которым могут относиться фильмы.
   - city – информация о городах, в которых расположены адреса клиентов.
   - country – информация о странах, в которых расположены города.
   - customer – информация о клиентах, которые берут в аренду фильмы.
   - film – информация о фильмах, доступных для проката.
   - film_actor – связь между фильмами и актерами, которые участвуют в этих фильмах.
   - film_category – связь между фильмами и категориями, к которым они относятся.
   - film_text – полный текст сюжетов фильмов, доступных для проката.
   - inventory – информация о DVD-дисках, доступных для проката.
   - language – список языков, на которых доступны фильмы.
   - payment – информация о платежах, сделанных клиентами за аренду фильмов.
   - rental – информация о факте аренды фильма клиентом.
   - staff – информация о сотрудниках компании, работающих в магазине.
   - store – информация о магазинах компании по прокату DVD-дисков.
## Задание 1
Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.


## Решение 1 
(размера всех индексов /  размеру всех таблиц) * 100 
выберем только таблицы, без представлений, тип TABLE_TYPE = 'BASE TABLE'
 
размер всех индексов именно таблиц 
```
 select index_length from information_schema.tables WHERE table_schema = 'sakila' and TABLE_TYPE = 'BASE TABLE'
```
![рис 1](https://github.com/ysatii/DB-HW4/blob/main/img/image1.jpg)

Сумарно по всем таблицам 
```
 select sum(index_length) from information_schema.tables WHERE table_schema = 'sakila' and TABLE_TYPE = 'BASE TABLE'
```
![рис 1_1](https://github.com/ysatii/DB-HW4/blob/main/img/image1_1.jpg)

Место занимаемое таблицами
```
 SELECT data_length from information_schema.tables WHERE table_schema = 'sakila' and TABLE_TYPE = 'BASE TABLE'
```
![рис 1_2](https://github.com/ysatii/DB-HW5/blob/main/img/image1_2.jpg)

СУммарно по всем таблицам
```
 SELECT sum(data_length) from information_schema.tables WHERE table_schema = 'sakila' and TABLE_TYPE = 'BASE TABLE'
```
![рис 1_3](https://github.com/ysatii/DB-HW5/blob/main/img/image1_3.jpg)

Итоговый запрос
```
SELECT  ((select sum(index_length) from information_schema.tables WHERE table_schema = 'sakila' and TABLE_TYPE = 'BASE TABLE')
/(SELECT sum(data_length) from information_schema.tables WHERE table_schema = 'sakila' and TABLE_TYPE = 'BASE TABLE'))*100 as index_size_in_data_size  
 ```
![рис 1_4](https://github.com/ysatii/DB-HW5/blob/main/img/image1_4.jpg)

Процентное отношение всех индексов ко всем данным

```
SELECT  ((select sum(index_length) from information_schema.tables WHERE table_schema = 'sakila')
/(SELECT sum(data_length) from information_schema.tables WHERE table_schema = 'sakila'))*100 as index_size_in_data_size  
```
![рис 1_5](https://github.com/ysatii/DB-HW5/blob/main/img/image1_5.jpg)


## Задание 2
- Выполните explain analyze следующего запроса:
```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

## Решение 2

