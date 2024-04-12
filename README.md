# Database for E-commerce

This project consists of creating a database for an e-commerce system using MySQL. The database is designed to store information about customers, suppliers, products, orders and payments, as well as managing the relationship between these entities.

## Model Database ER
![Database relational schema](banco de dados/e-commerce.png](https://github.com/MarcoABsouza/E-commerce-Logical-SGBD/blob/main/banco%20de%20dados/e-commerce.png)

## Database Model
The database model consists of seven main tables:

1. **clients_data**: Stores customers' personal information, such as name, email address, telephone number and social security number.
2. **clients_address**: Stores customer addresses, associating them with the corresponding customers.
3. **supplier**: Stores supplier information, such as CNPJ and company name;.
4. **products**: Stores information on the products offered in e-commerce, such as name, description, value and category.
5. **storage_ecommerce**: Stores information on product storage locations.
6. **supplier_provides_products**: Establishes the relationship between suppliers and the products they supply, including the quantity supplied.
7. **order_client**: Stores information about orders placed by customers, including status, payment, shipping and description.
8. **payment**: Stores information on payments made by customers for each order.

## Data entry

Fictitious data was inserted into all the tables to simulate a working e-commerce environment. The data includes information on customers, suppliers, products, addresses, orders and payments.

## Consultations held

Several queries were carried out to demonstrate the database's functionalities. Some queries include:

1. Which product did each customer buy.
2. Where does each client live.
3. How many orders were placed by each customer.
4. List of product suppliers and stocks.

These queries are just examples of how the database can be consulted to obtain useful information for managing e-commerce.



