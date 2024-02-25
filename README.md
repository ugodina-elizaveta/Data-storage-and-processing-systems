# Data-storage-and-processing-systems
## Hw3
1.
![1](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/f2dd0f8a-14d2-4f18-bf7e-b477b6b1c6c7)

2.
![2](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/033db3fa-8e64-4305-8fa1-659f8f7bde1e)

3.
![3](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/c7309ce4-c50d-4197-9644-20f2756c2b85)

4.
![4_1](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/0f6b7a34-3849-43e7-a67b-087dcac0a9c8)
![4_2](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/47a23a18-822a-4460-a5d5-1efd7356397c)

5.
![5_1](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/bbc19942-16f1-4829-878d-4adc015eefc7)
![5_2](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/0344b953-cf69-4b6d-a3f9-7bd77d4700f3)


6.
![6](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/3c8abd12-b92a-4bb9-a895-a16a25fb4dba)

7.
![7](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/2f681ed6-5321-4bdf-a519-40f37b4919a6)


-- 1.Вывести распределение (количество) клиентов по сферам деятельности, отсортировав результат по убыванию количества:
SELECT job_industry_category, COUNT(customer_id) AS customer_count
FROM customer
GROUP BY job_industry_category
ORDER BY customer_count DESC;

-- 2.Найти сумму транзакций за каждый месяц по сферам деятельности, отсортировав по месяцам и по сфере деятельности:
SELECT EXTRACT(MONTH FROM t.transaction_date) AS month,
       c.job_industry_category,
       SUM(t.list_price) AS total_transactions
FROM transaction t
JOIN customer c ON t.customer_id = c.customer_id
GROUP BY month, c.job_industry_category
ORDER BY month, total_transactions DESC;

-- 3.Вывести количество онлайн-заказов для всех брендов в рамках подтвержденных заказов клиентов из сферы IT:
SELECT brand, COUNT(online_order) AS online_orders_count
FROM transaction t
JOIN customer c ON t.customer_id = c.customer_id
WHERE c.job_industry_category = 'IT' AND t.order_status = 'Approved'
GROUP BY brand;

-- 4.Найти по всем клиентам сумму всех транзакций (list_price), максимум, минимум и количество транзакций, отсортировав результат по убыванию суммы транзакций и количества клиентов.

--Используя только GROUP BY:
SELECT customer_id,
       SUM(list_price) AS total_transactions,
       MAX(list_price) AS max_transaction,
       MIN(list_price) AS min_transaction,
       COUNT(transaction_id) AS total_transactions_count
FROM transaction
GROUP BY customer_id
ORDER BY total_transactions DESC, total_transactions_count DESC;

-- Используя только оконные функции:
SELECT customer_id,
       SUM(list_price) OVER (PARTITION BY customer_id) AS total_transactions,
       MAX(list_price) OVER (PARTITION BY customer_id) AS max_transaction,
       MIN(list_price) OVER (PARTITION BY customer_id) AS min_transaction,
       COUNT(transaction_id) OVER (PARTITION BY customer_id) AS total_transactions_count
FROM transaction
ORDER BY total_transactions DESC, total_transactions_count DESC;

-- 5.Найти имена и фамилии клиентов с минимальной/максимальной суммой транзакций за весь период:
-- Для минимальной суммы транзакций:
SELECT c.first_name, c.last_name
FROM customer c
JOIN transaction t ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
HAVING SUM(t.list_price) = (SELECT MIN(total_transactions) FROM 
    (SELECT customer_id, SUM(list_price) as total_transactions
    FROM transaction
    GROUP BY customer_id) as total
);

-- Для максимальной суммы транзакций: 
SELECT c.first_name, c.last_name
FROM customer c
JOIN transaction t ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
HAVING SUM(t.list_price) = (SELECT MAX(total_transactions) FROM 
    (SELECT customer_id, SUM(list_price) as total_transactions
    FROM transaction
    GROUP BY customer_id) as total
);

-- 6.Вывести только самые первые транзакции клиентов с помощью оконных функций:
WITH ranked_transactions AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY transaction_date) AS rn
    FROM transaction
)
SELECT *
FROM ranked_transactions
WHERE rn = 1;

-- 7.Вывести имена, фамилии и профессии клиентов, между транзакциями которых был максимальный интервал (интервал вычисляется в днях):
WITH t2 AS (
  SELECT customer_id, MAX(EXTRACT(DAY FROM transaction_date - CAST(LaggedTransactionDate AS TIMESTAMP))) AS MaxInterval
  FROM (
    SELECT customer_id, transaction_date, LAG(transaction_date) OVER (PARTITION BY customer_id ORDER BY transaction_date) AS LaggedTransactionDate
    FROM transaction
  ) t
  GROUP BY customer_id
)
SELECT c.first_name, c.last_name, c.job_title
FROM customer c
JOIN t2 ON c.customer_id = t2.customer_id
WHERE t2.MaxInterval = (SELECT MAX(MaxInterval) FROM t2)
