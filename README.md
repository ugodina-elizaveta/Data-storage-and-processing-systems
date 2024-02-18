# Data-storage-and-processing-systems
## Hw2
1.
 ![1](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/1f359071-438d-4377-b25f-9040486c6e36)

2.
 ![2](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/e04e02f5-5236-48ad-ae06-1ca244a0fd9b)

3.
 ![3](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/38736624-7f21-43c5-95fb-59e3296cd09d)

4.
 ![4](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/d6b544b6-4794-4803-b60a-add3f6149e8d)

5.
 ![5](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/7c07d7f6-c746-4833-a651-d069ed3b56c7)

6.
 ![6](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/f3410298-7a57-4dca-8f05-de98bd402723)

7.
 ![7](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/495111a4-6100-46c3-bdc9-6e3d6637ead3)

8.
 ![8](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/8ff0fbac-b5f9-47c1-9079-38a6b3aa2d24)



## Sql-скрипт:
-- 1. Вывести все уникальные бренды, у которых стандартная стоимость выше 1500 долларов
SELECT DISTINCT brand
FROM transaction
WHERE standard_cost > 1500;

-- 2. Вывести все подтвержденные транзакции за период '2017-04-01' по '2017-04-09' включительно
SELECT *
FROM transaction
WHERE order_status = 'Approved'
AND transaction_date BETWEEN '2017-04-01' AND '2017-04-09';

-- 3. Вывести все профессии у клиентов из сферы IT или Financial Services, которые начинаются с фразы 'Senior'
SELECT job_title
FROM customer
WHERE (job_industry_category = 'IT' OR job_industry_category = 'Financial Services')
AND job_title LIKE 'Senior%';

-- 4. Вывести все бренды, которые закупают клиенты, работающие в сфере Financial Services
SELECT DISTINCT t.brand
FROM transaction t
JOIN customer c ON t.customer_id = c.customer_id
WHERE c.job_industry_category = 'Financial Services';

-- 5. Вывести 10 клиентов, которые оформили онлайн-заказ продукции из брендов 'Giant Bicycles', 'Norco Bicycles', 'Trek Bicycles'
SELECT c.*
FROM customer c
JOIN transaction t ON c.customer_id = t.customer_id
WHERE t.brand IN ('Giant Bicycles', 'Norco Bicycles', 'Trek Bicycles')
ORDER BY t.transaction_date DESC
LIMIT 10;

-- 6. Вывести всех клиентов, у которых нет транзакций
SELECT *
FROM customer
WHERE customer_id NOT IN (SELECT DISTINCT customer_id FROM transaction);

-- 7. Вывести всех клиентов из IT, у которых транзакции с максимальной стандартной стоимостью
WITH max_cost AS (
    SELECT customer_id, MAX(standard_cost) AS max_cost
    FROM transaction
    GROUP BY customer_id
)
SELECT c.*
FROM customer c
JOIN transaction t ON c.customer_id = t.customer_id
JOIN max_cost mc ON t.customer_id = mc.customer_id AND t.standard_cost = mc.max_cost
WHERE c.job_industry_category = 'IT';

-- 8. Вывести всех клиентов из сферы IT и Health, у которых есть подтвержденные транзакции за период '2017-07-07' по '2017-07-17'
SELECT c.*
FROM customer c
JOIN transaction t ON c.customer_id = t.customer_id
WHERE c.job_industry_category IN ('IT', 'Health')
AND order_status = 'Approved'
AND t.transaction_date BETWEEN '2017-07-07' AND '2017-07-17';
