# Database for E-commerce

This project consists of creating a database for an e-commerce system using MySQL. The database is designed to store information about customers, suppliers, products, orders and payments, as well as managing the relationship between these entities.

## Model Database ER
<img src="banco de dados/e-commerce.png" alt="relational database schema">

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

"""

    select concat(cd.first_name," ",cd.middle_name," ",cd.last_name) as Client, p.name_product as Produto_Comprado
    from clients_data as cd, products as p, order_client as oc
    where cd.id_client = oc.client_id_order and oc.product_id_order = p.id_product; 

"""

2. Where does each client live.

"""

    select concat(cd.first_name," ",cd.middle_name," ",cd.last_name) as Client, ca.city as City
    from clients_data as cd, clients_address as ca
    where cd.id_client = ca.id_client;

"""

3. How many orders were placed by each customer.


"""

    select concat(cd.first_name," ",cd.middle_name," ",cd.last_name) as Client, count(oc.id_order) as Pedidos_Por_Cliente
    from clients_data as cd, order_client as oc
    where cd.id_client = oc.client_id_order
    group by oc.id_order;
    
"""

4. List of product suppliers and stocks.

"""

    select s.name_supplier as Supplier, p.name_product as Produto_Enviado_Pelo_Fornecedor, se.quantity_product_storage as Quantidade_do_Produto_em_Estoque
    from supplier_provides_products as spp, storage_ecommerce as se, supplier as s, products as p
    where s.id_supplier = spp.supplier and p.id_product = spp.products_by_supplier and se.products_storage = p.id_product;
    
    SELECT p.name_product, s.name_supplier, se.quantity_product_storage
    FROM products p
    INNER JOIN supplier_provides_products spp ON p.id_product = spp.products_by_supplier
    INNER JOIN supplier s ON spp.supplier = s.id_supplier
    INNER JOIN storage_ecommerce se ON p.id_product = se.products_storage;
"""

These queries are just examples of how the database can be consulted to obtain useful information for managing e-commerce.



