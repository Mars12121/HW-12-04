# Домашнее задание к занятию "SQL. Часть 2" - Морозов Александр

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
SELECT s.store_id Магазин, CONCAT(s2.last_name, " ",s2.first_name) Продавец, c.city Город, cl Клиенты
from store s 
join staff s2 on s2.staff_id = s.manager_staff_id
join address a on a.address_id = s.address_id 
join city c on c.city_id = a.city_id
JOIN (select store_id, COUNT(store_id) as cl
	from customer
	GROUP BY store_id) cc on cc.store_id = s.store_id
WHERE cl > 300
```

![alt text](https://github.com/Mars12121/hw-12-04/blob/main/img/1.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
select count(1)  
from film f
where length > (
	select avg(length) from film
	)
```

![alt text](https://github.com/Mars12121/hw-12-04/blob/main/img/2.png)

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```
SELECT DATE_FORMAT(payment_date, '%m-%Y') AS Месяц, SUM(amount) AS Сумма, count(rental_id) AS Аренды
FROM payment AS pp
GROUP BY Месяц
HAVING Сумма = (SELECT max(ss) FROM 
(SELECT DATE_FORMAT(payment_date, '%m-%Y') AS dd, SUM(amount) AS ss 
FROM payment
GROUP BY dd) AS qss)
```

![alt text](https://github.com/Mars12121/hw-12-04/blob/main/img/3.png)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.