---
layout: mysql
title: mysql
date: 2023-10-19 09:18:32
tags:
---
## 指令
```
select student_ID,student_Name
from student;
where student_Name = 'Mary Lamb';
```
```
select distinct student_Name，student_Address//查找可以有重复的，比如同一个人但是住在不同的地方或者同名的人；不能查找unique的元素。
from student;
```
```match criteria
select *
from student
where student_Mobile >111 and student_Age <18;
```
not in('  ','  ');
between 1 and 5;(1,2,3,4,5)
in(1,5);(1,5)
### %
A% = A____  例如AHMAD;
%A = ____A  例如SERA；
%A% = _A____  例如 SEAR；
A%A；
...
```
select *
from student
where student_Name like 'L%K%';
```
### group by
### join
### drop
### delete
记得删除要加from;
安全删除日志或单独的记录，可以复原，不删除表的关系等。而drop会删掉所有的并不能复原；
### concat 把两个column合在一起，只是查询临时放在一起
```
select concat(student_ID,',',student_Name)as'student info'
from student;
```
### add _ datetime null default now()
```把当前时间加入到表 _ 中
alter table student
add student_time datetime null default now();
```
### insert 
```
insert into student
(student_Name,student_Mobile,student_Email,student_ID)
values
('LK','0','0','xmus004');//加入新人lk
select * from student
order by student_ID;//显示顺序
```
```加column
alter table student
add DOB date NULL;
alter table student
add AGE int null;
```
然后设置一个DOB：
```
update student
set DOB = '2020-03-14'
where student_ID = 'xmus002';
```
根据设置的DOB来自动计算人年龄：
```
update student
set AGE =date_format(FROM_DAYS(DATEDIFF(NOW(),DOB)),'%Y') + 0
where student_ID between 'xmus001' and 'xmus004';
```
## 第6周作业
```
create database shop;
```
```
use shop;
```
```
create table customer
(
customerID varchar(10) not null primary key,
FirstName varchar(20) not null,
LastName varchar(20) not null,
DateofBirth date null,
street varchar(20) null,
city varchar(20) null,
state varchar(20) null,
MobileNo int null
);
```
```
create table item
(
ItemID varchar(10) not null primary key,
ItemName varchar(20) not null,
Price decimal(10,2) null,
Brand varchar(20) not null
);
```
```
create table salesman
(
StaffID varchar(10) not null primary key,
StaffName varchar(20) not null,
WorkingDate date null,
Salary decimal(10,2) null
);
```
```
create table transaction
(
InvoiceNo int not null primary key,
CustomeID varchar(10) not null,     //primary key 和 forigen key 都不能为null
ItemID varchar(10) not null,
StaffID varchar(10) not null,
Quantity int null,
TotalAmount decimal(10,2) null,
Foreign Key fk_transaction_CustomerID(customeID) references Customer(customerID),//不用区分大小写
Foreign Key fk_transaction_ItemID(ItemID) references item(ItemID),
Foreign Key fk_transaction_StaffID(staffID) references salesman(staffID)
);
```
```
insert into Customer
(customerID, FirstName,LastName,DateofBirth,Street,City,state,MobileNo)
values
('C001','Britney','Spears','2020-10-04','2nd Street','Sepang','Selangor','09222333'),
('C002','Britney','Jackson','2007-06-04','3rd Street','Shah Alam','Selangor','019444333'),
('C003','John','Wick','1996-04-07','2nd Level','Butterworth','Penang','019999333'),
('C004','John','Cena','1990-03-03','3rd Floor','Georgetown','Penang','012222333'),
('C005','Elizabeth','Stone','1985-04-03','4th Street','Sepang','Selangor','012567333'),
('C006','Jimmy','Stone','2002-10-03','2nd Street','Shah Alam','Selangor','019224433'),
('C007','Justin','Timerlake','2015-06-04','2nd Street','Shah Alam','Selangor','019444333');
```
```
insert into Item
(ItemID, ItemName,Price,Brand)
values
('I001','Laptop','1500','Huawei'),
('I002','Laptop','1700','Dell'),
('I003','Laptop','3500','Apple'),
('I004','Tablet','800','Apple'),
('I005','Tablet','1000','Huawei'),
('I006','Mouse','70','Huawei'),
('I007','Mouse','80','Logistech'),
('I008','Printer','500','HP'),
('I009','Printer','300','Canon'),
('I010','Speaker','50','Huawei');
select *from item;
```
```
insert into transaction
(InvoiceNo,customeID,ItemID,StaffID,Quantity)
values
('10001','C001','I003','S001','1'),
('10002','C001','I004','S001','1'),
('10003','C002','I001','S001','1'),
('10004','C002','I006','S001','1'),
('10005','C003','I002','S002','5'),
('10006','C003','I007','S002','5'),
('10007','C004','I009','S003','3'),
('10008','C004','I008','S003','2'),
('10009','C005','I001','S003','2'),
('10010','C005','I005','S003','2');
select *from transaction;
```
以上全是准备工作，现在才是重点
```1.  Write a query to display list of purchase item for customer ID (C001, C002).
select * from Transaction
where customeID IN('C001','C002');
```
```2.  Write a query to display list of customer for Staff ID (S001, S002).
select * from Transaction
where staffID IN('s001','s002');
```
```3. Write a query to display the customer ID, customer full name, and Full Address from customer Table.
select customerID as'ID NO',
concat(FirstName,' ',LastName) 'Full Name',
concat(street,',',city,',',state) 'Mailing Address'
from customer;
```
```4. Write a query to display the item id, item name, and price that price more than RM700.
select itemID,itemname,price
from item
where price >=700
order by price;
```
```5. Write a query to display the customer id, fullname (First+LastName), state that live in Selangor.
select customerID, 
concat(firstname,' ',lastname) as 'FULL Name', state
from customer
where state = 'selangor';//where state like 'sel%';
```
```6.  Write a query to list all the state in customer table.
select distinct state   //避免重复的
from customer;
```
```7. Write a query to display the price range is between 500 to 2000
select *from item
where price between 500 and 2000 //注意between和in的区别
order by price;
```
```8. Write a query where clerk salary is less than 1,500 and working before 2011.
select *from salesman
where Salary <= 2000 and WorkingDate <= '2011-01-01' //注意日期写法
order by WorkingDate;
```
```9. Write a query to display customer age.
select customerID, concat(firstname,' ',lastname) as 'full name',
concat(street,',',city,',',state)as'full address',dateofbirth,   //真的复杂，之前合并了，现在就都得写
(date_format(FROM_DAYS(DATEDIFF(NOW(),Dateofbirth)),'%Y') + 0)as age //这个不会真要背吧
from customer
order by age;
```
```10. Write a query to display working experience.
select staffid,staffname,workingdate,
(date_format(FROM_DAYS(DATEDIFF(NOW(),workingdate)),'%Y') + 0)as 'working experience' //长string用‘ ’括起来
from salesman
order by workingdate desc;
```
### 第7周作业
``` Write a query to display list of member information where name start with J.
select FirstName,
from members
where FirstName like 'J%';
```
```Write a query to display list of member that register for Diamond membership.
select *,
from members
where tpye_name = 'Diamond';
```
```Write a query to list all the state in member table.
select distinct state
from members;
```
```Write a query to display the member ID, member full name, Full Address, membership and sort by membership.
select memberID,
concat(LastName,',',city,',',FirstName)'Full Name',
concat(Street,',',City,',',State)'Full Address'),type_name,
from member
order by type_name;
```
```Write a query to display member age.
select memberID,
concat(LastName,',',city,',',FirstName)'Full Name',
concat(Street,',',City,',',State)'Full Address'),type_name,DOB,
((date_format(FROM_DAYS(DATEDIFF(NOW(),Dateofbirth)),'%Y') + 0)as age
from member
order by age;
```
```Write a query to display member that lives in Johor or Selangor.
select memberID,
concat(LastName,',',city,',',FirstName)'Full Name',
concat(Street,',',City,',',State)'Full Address'),type_name,DOB,
((date_format(FROM_DAYS(DATEDIFF(NOW(),DOB)),'%Y') + 0)as age
from member
where state = 'Johor' or state = 'selangor' -- where state in ('Johor','selangor')
order by age;
```
```Write a query to display MemberID, ClassID, TrainerID and sort by ClassID.
select memberID,classID,
from register
order by classID;
```
```Write a query to display the class id, class name, and class price between RM50 and RM150.
select classID,ClassName,Fee
from class
where fee between 50 and 150;
```
```Write a query where trainer salary is less than 3,000 and working before 2015.
select *
from trainer
where salary <3000 and WorkingDate <'2015-01-01'; --第一个是年，第二个是日，最后是月
```
```Write a query where trainer working experience is less than 5 years or specialized in Martial arts.
select trainerID,trainerName,workingDate,skill,
((date_format(FROM_DAYS(DATEDIFF(NOW(),workingdate)),'%Y') + 0)as experience
from trainer
where wokingdate >'2019-01-01' or skillID = 's001'; --大大的注意不能用experience来比较，系统不知道我们新建的这个是什么意思，所以还是只能用已有的workingdate.
```
总的来说，给列赋予别名时AS关键字是可选的，而且别名不需要用引号括起来。
### 第八周 Adding Jion Queries
#### inner join
```找到members 和 register 的交集.跟以前不一样是要加例如member.前缀
SELECT register.MemberID , members.firstName,
members.LastName, register.ClassId
from members inner join register on members.MemberID = register.memberID;
```
```升级版，但是一个table的 relationship不要超过2.
SELECT register.MemberID , concat(members.firstName,' ',members.LastName) as 'Full Nmae', register.ClassId, concat(members.street,' ',members.city,' ',members.state)'Address',
class.classname, class.price, membership.type_name
from membership inner join members on membership.type_name = members.type_name 
 inner join register on members.MemberID = register.memberID 
 inner join class on register.classid = class.classid
```
#### outer join
#### left join
#### right join
#### full join
!(){1.png}
#### aggreate
count, sum, min, max, average
```
select a,b,c,
count(register.classid) as 'Total class',
sum(class.price)as 'Total price'
from member class join register on member.memberid = register.memberid
group by register.memberid      --group 必不可少
```
#### Mathematic calculation
* / - +
```
sum(class.price)as"before discount",
sum((1-membership.discout) * class.price)as "Total Amount"
group by member.memberid;
```
### 9
```
select transaction.staffid, salesman.StaffName,
count(transaction.itemid)'order number',
sum(transaction.quantity)'total quantity',
sum(transaction.quantity*item.price) 'total sales',
(sum(transaction.quantity*item.price))*0.1+salesman.Salary as 'salery'
from salesman  inner join transaction on salesman.staffid = transaction.staffid
inner join item on item.itemid = transaction.itemid
group by transaction.staffid;
```
### 10
往已经有的table里加外键
```
alter table registration
add foreign key (staffid)
references staff(staffid);
```
Exercise
1. Write a query to display list of GuestID, Guest Full Name, Guest Address, and RoomID from registration
and guest table.
2. Write a query to display list of GuestID, Guest Full Name, Guest Address, RoomID, Roomtype from
registration, guest and room table.
3. Write a query to display list of GuestID, Guest Full Name, Guest Address, RoomID, Roomtype from
registration, guest and room table where Firstname start with ‘J’ and lives in Selangor. Compare the
registration and guest for Null values.
4. Write a query to count how many room that every guest has book.
5. Write a query for the SUM cost that the guest needs to pay for the room.
6. Write a query for the discounted rooms for every guest.
7. Write a query to count how many room that every staff has submitted.
8. Write a total sum of sales for every staff.
9. Calculate the total salary for staff if they get 15% commission from the total sales.
10. Calculate the age and working experience from guest and staff where age < 30 years old or working
experience > 10.
```
select registration.guestid,
concat(guest.firstname,'  ',guest.lastname) 'full name',
concat(guest.street,' ,',guest.city,' ,',guest.state) 'mailing address',
registration.roomid, room.roomtype,room.price
from guest inner join registration on guest.guestid = registration.guestid
inner join room on registration.roomid = room.roomid
```
在后面加where guest.firsname like '%kun'； 会发现结果为空，empty output = no sharing data on selected condition;
但是加 left outer
```
from guest left outer join registration on guest.guestid = registration.guestid
left outer join room on registration.roomid = room.roomid
where guest.firstname like 'j%' and guest.state = 'selangor';
```
有的变成null了 这个意思是no data;
再加：where registration.guestid is null; 出来的结果表示还没有订房间的客人；
```
select registration.guestid,
concat(guest.firstname,'  ',guest.lastname)as 'full name',
concat(guest.street,' ,',guest.city,' ,',guest.state) 'mailing address',
sum(room.price*datediff(registration.checkout,registration.checkin))as 'total pay',
((100-membership.discount)/100) as 'total discount'
from membership inner join guest on membership.type = guest.type
inner join registration on guest.guestid = registration.guestid
inner join room on registration.roomid = room.roomid
group by registration.guestid;
```



