# Database for E-commerce

This project consists of creating a database for an e-commerce system using MySQL. The database is designed to store information about customers, suppliers, products, orders and payments, as well as managing the relationship between these entities.This project consists of creating a database for an e-commerce system using MySQL. The database is designed to store information about customers, suppliers, products, orders and payments, as well as managing the relationship between these entities. In this project I entered random data, made queries for some questions, created views, created indexes for some things and finally created procedures in my database for an e-commerce.

## Model Database ER
<img src="banco de dados/e-commerce.png" alt="relational database schema">

## Database Model
The database model consists of eight main tables:

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

## View db
1. View clients details

"""

    create view Clients as 
    select concat(cd.first_name," ",cd.middle_name," ",cd.last_name) as complete_name, cd.email as Email_Client,
    cd.phone as Phone_CLiente, concat(ca.street," ",ca.number_address," ",ca.district," ",ca.city," ",ca.state," ",ca.cep) as Address
    from clients_data as cd
    Join clients_address ca on cd.id_client = ca.client_id;
    
    
    select * from Clients;

"""

## Index use in our db
1. Create index on tables

"""

    create index idx_cpf on clients_data (cpf);
    create index idx_cnpj on supplier (cnpj);
    
"""

Now using some index create we can do some queries

"""

        select * 
        from order_client as oc
        join clients_data as ca on oc.client_id = ca.id_client
        where ca.cpf = "45678901234";
        
        
        select *
        from supplier_products as sp
        join supplier as s on sp.supplier_id = s.id_supplier
        where s.cnpj = "34567890123456";
        

"""
## Procedure
1. Create procedure in our db

"""

    delimiter \\
    create procedure revenue()
    begin
    	declare current_revenue decimal(10,2);
        
        SELECT SUM(p.value_payment)
        INTO current_revenue
        FROM order_client oc
        JOIN payment p ON oc.id_order = p.order_client_id
        WHERE oc.status_order = 'Aprovado';
    
        -- Exibir a receita total
        SELECT current_revenue;
        
    end \\
    delimiter ;
    
    
    delimiter ||
    create procedure info_clients()
    begin
        select concat(first_name," ",middle_name," ",last_name) as complete_name,concat(street,",",number_address,",",district,",",city,",",state) from clients_data inner join clients_address on id_client = client_id;
    end ||
    delimiter ;
    
    
    DELIMITER $$
    CREATE PROCEDURE cancel_order(
        IN id SMALLINT
    )
    BEGIN
        -- Atualizar o status do pedido para "Cancelado"
        UPDATE order_client
        SET status_order = 'Cancelado', 
            description_order = 
                CASE 
                    WHEN payment = TRUE THEN 'Realizar retorno ao cliente' 
                    ELSE description_order 
                END
        WHERE id_order = id;
    
        -- Selecionar e retornar a mensagem de sucesso junto com o número do pedido cancelado
        SELECT 'Pedido cancelado com sucesso' AS message, id AS numero_pedido_cancelado;
    END $$
    DELIMITER ;
    
    
    delimiter ##
    create procedure orders (
    	in order_ varchar(50),
        in id_order smallint
    )
    begin
    	case order_
    		when "check revenue" then
    			call revenue;
    		when "client information" then
    			call info_clients;
    		when "cancel order" then
    			call cancel_order(id_order);
            else
    			select "ação invalida" as message;
    	end case;
    end ##
    delimiter ;

"""

### Triggers
1. Trigger used before insertion, to check if the customer's cpf already exists in the database

"""

    delimiter \\
    create trigger orders before insert on clients_data
    for each row
    	begin
    		declare cpf_count int;
            
            
            select count(*) into cpf_count
            from clients_data use index (unique_cpf_client)
            where cpf = new.cpf;
            
            
            if cpf_count > 0 then
    			signal sqlstate '45000'
    			set message_text = "cpf ja existe no banco de dados";
    		end if;
    	end \\
    
    delimiter ;

"""
