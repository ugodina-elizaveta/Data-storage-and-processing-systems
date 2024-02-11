# Data-storage-and-processing-systems
## Hw1
### Скрипт для отрисовки структуры базы данных в редакторе:
Table store {
  transaction_id integer [primary key]
  transaction_date timestamp
  online_order bool
  order_status varchar 
  list_price float
  product_id integer
  customer_id integer
}

Table product {
  product_id integer [primary key]
  brand varchar
  product_line varchar
  product_class varchar
  product_size varchar
  standart_cost float
}

Table customer {
  customer_id integer [primary key]
  first_name varchar
  last_name varchar
  gender varchar
  DOB timestamp
  job_title varchar
  job_industry_category varchar
  wealth_segment varchar
  deceased_indicator varchar
  owns_car varchar
  address varchar
  postcode integer
  state varchar
  country varchar
  property_valuation integer

}

Ref: store.product_id > product.product_id
Ref: store.customer_id > customer.customer_id

![1](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/4505d591-1cf1-4e16-a0af-9c4bfdf0b708)
 
### Норамализация базы данных:
Таблица store:
*	1НФ: Первичный ключ transaction_id и нет повторяющихся столбцов или групп столбцов.
*	2НФ: Нет неполностей в зависимостях от первичного ключа, так как все атрибуты зависят только от первичного ключа.
*	3НФ: Нет транзитивных зависимостей, так как все атрибуты зависят только от первичного ключа.
  
Таблица product:
*	1НФ: Первичный ключ product_id и нет повторяющихся столбцов или групп столбцов.
*	2НФ: Нет неполностей в зависимостях от первичного ключа, так как все атрибуты зависят только от первичного ключа.
*	3НФ: Нет транзитивных зависимостей, так как все атрибуты зависят только от первичного ключа
  
Таблица customer:
*	1НФ: Первичный ключ customer_id и нет повторяющихся столбцов или групп столбцов.
*	2НФ: Нет неполностей в зависимостях от первичного ключа, так как все атрибуты зависят только от первичного ключа.
*	3НФ: Нет транзитивных зависимостей, так как все атрибуты зависят только от первичного ключа.


### Скрипт из DBeaver:
CREATE TABLE store (
  transaction_id INTEGER PRIMARY KEY,
  transaction_date TIMESTAMP,
  online_order BOOLEAN,
  order_status VARCHAR,
  list_price FLOAT,
  product_id INTEGER,
  customer_id INTEGER
);

CREATE TABLE product (
  product_id INTEGER PRIMARY KEY,
  brand VARCHAR,
  product_line VARCHAR,
  product_class VARCHAR,
  product_size VARCHAR,
  standard_cost FLOAT
);

CREATE TABLE customer (
  customer_id INTEGER PRIMARY KEY,
  first_name VARCHAR,
  last_name VARCHAR,
  gender VARCHAR,
  DOB TIMESTAMP,
  job_title VARCHAR,
  job_industry_category VARCHAR,
  wealth_segment VARCHAR,
  deceased_indicator VARCHAR,
  owns_car VARCHAR,
  address VARCHAR,
  postcode INTEGER,
  state VARCHAR,
  country VARCHAR,
  property_valuation INTEGER
);

INSERT INTO store (transaction_id, transaction_date, online_order, order_status, list_price, product_id, customer_id) VALUES
(1, '25.02.2017', false, 'Approved', 71.49, 2, 2950),
(2, '21.05.2017', true, 'Approved', 2091.47, 3, 3120),
(3, '16.10.2017', false, 'Approved', 1793.43, 37, 402);


INSERT INTO product (product_id, brand, product_line, product_class, product_size, standard_cost) VALUES
(2, 'Solex', 'Standard', 'medium', 'medium', 53.62),
(3, 'Trek Bicycles', 'Standard', 'medium', 'large', 388.92),
(37, 'OHM Cycles', 'Standard', 'low', 'medium', 248.82);

INSERT INTO customer (customer_id, first_name, last_name, gender, DOB, job_title, job_industry_category, wealth_segment, deceased_indicator, owns_car, address, postcode, state, country, property_valuation) VALUES
(2950, 'Laraine', 'Medendorp', 'F', '1953-10-12', 'Executive Secretary', 'Health', 'Mass Customer', 'N', 'Yes', '060 Morning Avenue', 2016, 'New South Wales', 'Australia', 10),
(3120, 'Eli', 'Bockman', 'Male', '1980-12-16', 'Administrative Officer', 'Financial Services', 'Mass Customer', 'N', 'Yes', '6 Meadow Vale Court', 2153, 'New South Wales', 'Australia', 10),
(402, 'Arlin', 'Dearle', 'Male', '1954-01-20', 'Recruiting Manager', 'Property', 'Mass Customer', 'N', 'Yes', '0 Holy Cross Court', 4211, 'QLD', 'Australia', 9);

* Таблица customer:
![customer](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/cf7acb6b-9e6a-4008-9c3d-4c18d747a2bd)

 
* Таблица product:
![product](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/10fa4e25-5036-45dd-ab33-a786afcb1abf)
 
* Таблица store:
![store](https://github.com/ugodina-elizaveta/Data-storage-and-processing-systems/assets/108820578/79e54ae3-6e2f-403f-bfd1-527e8e547769)
