-- ДЗ2, етап 5. Порожня схема після 3НФ.
-- Дані з етапів 1–3 сюди не вставляються: у завданні лише колонки і зв'язки.

CREATE DATABASE IF NOT EXISTS goit_rdb_hw_02
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

USE goit_rdb_hw_02;

DROP TABLE IF EXISTS order_items;
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS customers;

CREATE TABLE customers (
  customer_id INT NOT NULL AUTO_INCREMENT,
  customer_name VARCHAR(100) NOT NULL,
  address VARCHAR(255) NOT NULL,
  PRIMARY KEY (customer_id)
) ENGINE=InnoDB;

CREATE TABLE products (
  product_id INT NOT NULL AUTO_INCREMENT,
  product_name VARCHAR(100) NOT NULL,
  PRIMARY KEY (product_id),
  UNIQUE KEY uq_products_name (product_name)
) ENGINE=InnoDB;

CREATE TABLE orders (
  order_id INT NOT NULL,
  customer_id INT NOT NULL,
  order_date DATE NOT NULL,
  PRIMARY KEY (order_id),
  KEY idx_orders_customer (customer_id),
  CONSTRAINT fk_orders_customer
    FOREIGN KEY (customer_id) REFERENCES customers (customer_id)
) ENGINE=InnoDB;

CREATE TABLE order_items (
  order_id INT NOT NULL,
  product_id INT NOT NULL,
  quantity INT NOT NULL,
  PRIMARY KEY (order_id, product_id),
  KEY idx_order_items_product (product_id),
  CONSTRAINT chk_order_items_quantity CHECK (quantity > 0),
  CONSTRAINT fk_order_items_order
    FOREIGN KEY (order_id) REFERENCES orders (order_id),
  CONSTRAINT fk_order_items_product
    FOREIGN KEY (product_id) REFERENCES products (product_id)
) ENGINE=InnoDB;
