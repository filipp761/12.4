# Домашнее задание к занятию 12.4 "Реляционные базы данных: SQL. Часть 2"
Решение нужно прислать одним SQL файлом, содержащим запросы по всем заданиям.

Любые вопросы по решению задач задавайте в чате учебной группы.

Задание можно выполнить как в любом IDE, так и в командной строке.

## Задание 1.
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина, город нахождения магазина, количество пользователей, закрепленных в этом магазине.
____

Ответ:
```SQL
SELECT * s.first_name, s.last_name, address,
COUNT (c.store_id) as 'Покупатели'
FROM customer c
JOIN staff s ON c.store=s.store_id
JOIN address a on s.address_id=a.address_id
GROUP by c.store_id, s.first_name, s.last_name, address
HAVING Покупатели > 300
```
![alt text](https://github.com/filipp761/12.4/blob/main/img/1.jpg)

## Задание 2.
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
____

Ответ:
```SQL
SELECT COUNT(*) as COUNT_FILM
FROM film f
WHERE lenght > (SELECT AVG('lenght') FROM film)
```
![alt text](https://github.com/filipp761/12.4/blob/main/img/2.jpg)
## Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
____

Ответ:
```SQL
SELECT
MONTH(payment_date) as 'Месяц'
,YEAR(payment_date) as 'Год'
,SUM(amount) as 'Сумма_заказов'
,COUNT(rental_id) as 'Количество_заказов'
FROM payment p
GROUP BY MONTH(payment_date), YEAR(payment_date)
ORDER BY SUM(amount) DESC
limit 1
```
![alt text](https://github.com/filipp761/12.4/blob/main/img/3.jpg)
# Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

## Задание 4*
Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».
___

Ответ:
```SQL
SELECT staff_id AS 'Продавец'
,COUNT (staff_id) AS 'Продажи'
,CASE
    WHEN COUNT (staff_id) > 8000 THEN 'ДА'
    WHEN COUNT (staff_id) < 8000 THEN 'НЕТ'
END as 'Премия'
FROM payment p
GROUP by staff_id
```
![alt text](https://github.com/filipp761/12.4/blob/main/img/4.jpg)
## Задание 5*
Найдите фильмы, которые ни разу не брали в аренду.
____

Ответ:
```SQL
SELECT title FROM inventory i
RIGHT JOIN film f ON i.film_id=f.film_id
WHERE inventory_id IS NULL
```
![alt text](https://github.com/filipp761/12.4/blob/main/img/5.jpg)
