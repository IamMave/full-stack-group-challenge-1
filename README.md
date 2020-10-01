# Web-site using Angular, Express, Node and SQL
The warehousing company you worked with on the JOINs challenge wants you back. They are hiring new people who don't know how to use SQL, so they want a website that makes it simple to view some information in their database using Angular, Express, Node, and SQL.

In case you lost the tables from before, they have sent you the Entity Relationship Diagram (ERD) and the queries to create the tables (also available in `database-creation.sql`):
[Entity Relationship Diagram](https://docs.google.com/drawings/d/1eA7JJtCVDL0K45aVzbxIUrgWXHoKY5vv1jAhssC2c1A/edit)
```SQL
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    first_name character varying(60),
    last_name character varying(80)
);

CREATE TABLE addresses (
    id SERIAL PRIMARY KEY,
    street character varying(255),
    city character varying(60),
    state character varying(2),
    zip character varying(12),
    address_type character varying(8),
    customer_id integer REFERENCES customers
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    order_date date,
    total numeric(4,2),
    address_id integer REFERENCES addresses
);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    description character varying(255),
    unit_price numeric(3,2)
);

CREATE TABLE line_items (
    id SERIAL PRIMARY KEY,
    unit_price numeric(3,2),
    quantity integer,
    order_id integer REFERENCES orders,
    product_id integer REFERENCES products
);

CREATE TABLE warehouse (
    id SERIAL PRIMARY KEY,
    warehouse character varying(55),
    fulfillment_days integer
);

CREATE TABLE warehouse_product (
    product_id integer NOT NULL REFERENCES products,
    warehouse_id integer NOT NULL REFERENCES warehouse,
    on_hand integer,
    PRIMARY KEY (product_id, warehouse_id)
);

INSERT INTO customers
VALUES (1, 'Lisa', 'Bonet'),
(2, 'Charles', 'Darwin'),
(3, 'George', 'Foreman'),
(4, 'Lucy', 'Liu');

INSERT INTO addresses
VALUES (1, '1 Main St', 'Detroit', 'MI', '31111', 'home', 1),
(2, '555 Some Pl', 'Chicago', 'IL', '60611', 'business', 1),
(3, '8900 Linova Ave', 'Minneapolis', 'MN', '55444', 'home', 2),
(4, 'PO Box 999', 'Minneapolis', 'MN', '55334', 'business', 3),
(5, '3 Charles Dr', 'Los Angeles', 'CA', '00000', 'home', 4),
(6, '934 Superstar Ave', 'Portland', 'OR', '99999', 'business', 4);

INSERT INTO orders
VALUES (1, '2010-03-05', 88.00, 1),
(2, '2012-02-08', 23.50, 2),
(3, '2016-02-07', 4.09, 2),
(4, '2011-03-04', 4.00, 3),
(5, '2012-09-22', 12.99, 5),
(6, '2012-09-23', 23.00, 6),
(7, '2013-05-25', 39.12, 5);

INSERT INTO products
VALUES (1, 'toothbrush', 3.00),
(2, 'nail polish - blue', 4.25),
(3, 'generic beer can', 2.50),
(4, 'lysol', 6.00),
(5, 'cheetos', 0.99),
(6, 'diet pepsi', 1.20),
(7, 'wet ones baby wipes', 8.99);

INSERT INTO line_items
VALUES (1, 5.00, 16, 1, 1),
(2, 3.12, 4, 1, 2),
(3, 4.00, 2, 2, 3),
(4, 7.25, 3, 4, 4),
(5, 6.00, 1, 5, 7),
(6, 2.34, 6, 6, 5),
(7, 1.05, 9, 7, 5);

INSERT INTO warehouse VALUES (1, 'alpha', 2),
(2, 'beta', 3),
(3, 'delta', 4),
(4, 'gamma', 4),
(5, 'epsilon', 5);

INSERT INTO warehouse_product
VALUES (1, 3, 0),
(1, 1, 5),
(2, 4, 20),
(3, 5, 3),
(4, 2, 9),
(4, 3, 12),
(5, 3, 7),
(6, 1, 1),
(7, 2, 4),
(6, 3, 88),
(6, 4, 3);
```

To start, the customer would like a web application with three views: warehouse, customer, orders. Each view should have it's own angular controller, template, and a clickable angular route available in the navigation bar. The client has included a wireframe of what they would like:
![Site Wireframe](/images/wireframe.png)

They have also included an image of the SQL data they would like returned.

1. The warehouse view should have a table that includes this information:
![warehouse table](/images/warehouses.png)
2. The customer view should have a table that includes this information:
![customers table](/images/customers.png)
3. The orders view should have a table that includes this information which will require a few `JOIN`s:
![orders table](/images/orders.png)

They want you to make their website look nice and pretty by putting these into a table and adding bootstrap to their site (the rows should be part of an ng-repeat).

## Hard Mode

1. The warehouse view should have a search bar that allows the list to be filtered by what is in stock. For example:
  * When the user searches for `diet pepsi` or `Diet Pepsi`, the table should display the following information: ![Diet Pepsi Filter](/images/diet-pepsi.png)
  * When the user searches for `cheetos` or `Cheetos` the table should display the following information: ![Cheetos](/images/cheetos.png)
2. The customers table should include the number of orders that customer has: ![Customer With Number of Orders](/images/customer-with-number-of-orders.png)

## Pro Mode

1. Create a new view for products that includes a table with the total number of that product in stock: ![Products On Hand](/images/on-hand.png)
2. Allow a user to create (post) a new order on the orders view. Keep in mind that this will require updating multiple tables.
