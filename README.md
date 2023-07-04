# 12.3 «SQL. Часть 1» - Дрибноход Давид

### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
```sql
SELECT DISTINCT district
FROM sakila.address
WHERE district like 'K%a'
      and POSITION(' ' IN district) = 0;
```

### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.
```sql
SELECT *
FROM sakila.payment
WHERE Date(payment_date) between '2005-06-15' and '2005-06-18'
		and amount > 10.0;
```
### Задание 3

Получите последние пять аренд фильмов.
```sql
--Если речь о последних записях в таблице по ключу то нужен такой запрос.
SELECT *
FROM sakila.rental
ORDER BY rental_id DESC
LIMIT 5;

--Ечли же реь о последних арендах по дате то подойдет такой запрос
SELECT *
FROM sakila.rental
ORDER BY rental_date DESC
LIMIT 5;
```

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.

```sql
SELECT REPLACE(LOWER(first_name), 'll', 'pp'), LOWER(last_name), active
FROM sakila.customer
WHERE first_name IN ('Kelly', 'Willie') and active = 1;
```

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.
```sql
SELECT LEFT(email, POSITION('@' IN email)-1) as 'left_part_of_email',
		RIGHT(email, CHAR_LENGTH(email)-POSITION('@' IN email))  as 'right_part_of_email'
FROM sakila.customer;
```

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.
```sql
SELECT	CONCAT(UPPER(LEFT(LEFT(email, POSITION('@' IN email)-1), 1)), LOWER(SUBSTRING(LEFT(email, POSITION('@' IN email)-1), 2, LENGTH(LEFT(email, POSITION('@' IN email)-1))-1)))  as 'left_part_of_email',
		CONCAT(UPPER(LEFT(RIGHT(email, CHAR_LENGTH(email)-POSITION('@' IN email)), 1)), LOWER(SUBSTRING(RIGHT(email, CHAR_LENGTH(email)-POSITION('@' IN email)), 2, LENGTH(RIGHT(email, CHAR_LENGTH(email)-POSITION('@' IN email)))-1)))  as 'right_part_of_email'
FROM sakila.customer;
```
