
````
DROP DATABASE IF exists cargo;
create database cargo;

use cargo;

CREATE TABLE user(
user_id varchar (255),
username varchar (50),
email varchar(50),
password  varchar(50),
image  text(255),
date  timestamp,
primary key(user_id)

);

CREATE TABLE customer(
customer_id int (11) auto_increment,
fristname varchar (50),
lastname varchar (50),
phone  varchar(50),
address  varchar(50),
city  varchar(50),
state  varchar(50),
primary key(customer_id)

);


CREATE TABLE office(
office_id int (11) auto_increment,
address  varchar(50),
city  varchar(50),
state  varchar(50),
primary key(office_id)
);

CREATE TABLE employee(
employee_id int (11) auto_increment,
fristname varchar (50),
lastname varchar (50),
phone  varchar(50),
city  varchar(50),
state  varchar(50),
office_id int ,
primary key(employee_id),
foreign key (office_id) references office (office_id) on update cascade

);

CREATE TABLE Shipping_type(
Shipping_type_id int (11) auto_increment not null,
name  varchar(50),
primary key(Shipping_type_id)
);


create table orders (
order_id int (11) auto_increment ,
customer_id int,
item_name varchar(50),
size int,
Shipping_type_id int, 
shipment varchar(50),
shipping_from varchar(50),
shipping_to varchar(50),
shipped_date timestamp,
planed_date timestamp,
status varchar(50) default 'process',
primary key(order_id),
foreign key (customer_id) references customer (customer_id) on update cascade,
foreign key (Shipping_type_id) references Shipping_type (Shipping_type_id) on update cascade
);

create table invoice (
invoice_id int (11) auto_increment,
customer_id int,
invoice_total varchar(50),
invoice_date timestamp,
status varchar(50) default 'padding',
primary key(invoice_id),
foreign key (customer_id) references customer (customer_id) on update cascade
);

CREATE TABLE account (
    account_id INT(11) AUTO_INCREMENT,
    number INT,
    name VARCHAR(50),
    bank_name varchar(50),
    balance decimal(9,2),
    primary key(account_id)
);

CREATE TABLE payment_method (
    payment_method_id INT(11) AUTO_INCREMENT,
    Name VARCHAR(50),
    primary key(payment_method_id)
);

CREATE TABLE payment (
    payment_id INT(11) AUTO_INCREMENT,
    customer_id int,
    account_id int,
    amount decimal(9,2),
    payment_method_id int,
    status varchar(50) default 'paid',
    primary key(payment_id),
    foreign key (customer_id) references customer (customer_id) on update cascade,
    foreign key (payment_method_id) references payment_method (payment_method_id) on update cascade,
    foreign key (account_id) references account(account_id) on update cascade
);

CREATE TABLE bill (
    bill_id INT(11) AUTO_INCREMENT,
    employee_id int,
    account_id int,
    amount float(9,2),
    primary key(bill_id),
    foreign key (employee_id) references employee (employee_id) on update cascade
  
);

##tigger account 

DELIMITER $$
CREATE TRIGGER update_status_ap
AFTER INSERT ON payment 

FOR EACH ROW

BEGIN 
Update account set balance = balance + NEW.amount
where account_id = NEW.account_id;
Update invoice set status = 'paid'
where  invoice_id = NEW.account_id;

END $$

DELIMITER ;

##tigger bill

DELIMITER $$
CREATE TRIGGER update_account
AFTER INSERT ON bill 

FOR EACH ROW

BEGIN 
Update account set balance = balance - NEW.amount
where account_id = NEW.account_id;

END $$

DELIMITER ;


##inserting

insert into customer values(1,'anwar','isak',9687687,'hodan','mogadisho','banaadir') ;
insert into Shipping_type values(1,'airplane') ;
insert into Shipping_type values(2,'container') ;
insert into payment_method values(1,'bank') ;
insert into account (number,name,bank_name,balance)values (100100,'anwar isak','my bank',100);

insert into orders (customer_id,item_name,size,Shipping_type_id,shipment,shipping_from,shipping_to,shipped_date,planed_date)values (1,'boorso',5,1,'Qatar airways','china','somalia','2022-11-24','2022-11-25');
insert into invoice (customer_id,invoice_total,invoice_date)values (1,30,'2022-11-24');
-- insert into invoice (customer_id,invoice_total,invoice_date)values (1,30,'2022-11-24');

insert into payment (customer_id,account_id,amount,payment_method_id)values (1,1,30,1);

insert into office (address,city,state)values ('bakaaro','mogadishu','banaadir');
insert into employee (fristname,lastname,phone,city,state,office_id)values ('abdirahmaan','maxamed',252616771717,'mogadishu', 'banaadir',1);
insert into bill (employee_id,account_id,amount)values (1,1,100);

select * from customer;
select * from orders;
-- select i.invoice_id , c.fristname, c.lastname, i.invoice_total, i.invoice_date  from invoice i join customer c on i.customer_id=c.customer_id;

select * from payment;
select * from invoice;
select * from account;
select * from employee;
select * from bill;
select * from user;

-- select order_id,customer_id, size,size*3.5 as price,Shipping_type_id,shipment,shipping_from,shipping_to,shipped_date ,planed_date from orders;
select * from payment;
-- select * from customer;


```