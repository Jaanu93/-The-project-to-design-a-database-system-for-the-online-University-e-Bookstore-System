# The-project-to-design-a-database-system-for-the-online-University-e-Bookstore-System
Create database Bookstore
use Bookstore
CREATE TABLE Publisher
(
  PublisherID VARCHAR(50) NOT NULL,
  PublisherName VARCHAR(50) NOT NULL,
  PublisherAddress VARCHAR(50) NOT NULL,
  PRIMARY KEY (PublisherID)
);
CREATE TABLE OrderTbl
(
  OrderID VARCHAR(50) NOT NULL,
  OrderDate DATE NOT NULL,
  OrderTotal INT NOT NULL,
  BookID VARCHAR(50) NOT NULL,
  PRIMARY KEY (OrderID)

CREATE TABLE Book
(
  BookID VARCHAR(50) NOT NULL,
  BookTitle VARCHAR(50) NOT NULL,
  BookSerialNUmber VARCHAR(50) NOT NULL,
  BookQuantity INT NOT NULL,
  BookPrice INT NOT NULL,
  PublisherID VARCHAR(50) NOT NULL,
  PRIMARY KEY (BookID),
  FOREIGN KEY (PublisherID) REFERENCES Publisher(PublisherID)
);
CREATE TABLE Cart
(
  CartID VARCHAR(50) NOT NULL,
  CartDate DATE NOT NULL,
  CartStatus VARCHAR(50) NOT NULL,
  CartTotal INT NOT NULL,
  PRIMARY KEY (CartID)
);
CREATE TABLE holds
(
  CartQuantity INT NOT NULL,
  SubTotalCart INT NOT NULL,
  BookID VARCHAR(50) NOT NULL,
  CartID VARCHAR(50) NOT NULL,
  PRIMARY KEY (BookID, CartID),
  FOREIGN KEY (BookID) REFERENCES Book(BookID),
  FOREIGN KEY (CartID) REFERENCES Cart(CartID)
);
CREATE TABLE has
(
  OrderQuantity INT NOT NULL,
  SubTotalOrder INT NOT NULL,
  BookID VARCHAR(50) NOT NULL,
  OrderID VARCHAR(50) NOT NULL,
  OrderDetailID VARCHAR(50) NOT NULL,
  PRIMARY KEY (BookID, OrderID),
  FOREIGN KEY (BookID) REFERENCES Book(BookID),
  FOREIGN KEY (OrderID) REFERENCES OrderTbl(OrderID)
);
CREATE TABLE Publisher_PublisherContact
(
  PublisherContact VARCHAR(12) NOT NULL,
  PublisherID VARCHAR(50) NOT NULL,
  PRIMARY KEY (PublisherContact, PublisherID),
  FOREIGN KEY (PublisherID) REFERENCES Publisher(PublisherID)
);
CREATE TABLE Invoice
(
  InvoiceID VARCHAR(50) NOT NULL,
  InvoiceDate DATE NOT NULL,
  OrderID VARCHAR(50) NOT NULL,
  TotalInvoice INT NOT NULL,
  PublisherID VARCHAR(50) NOT NULL,
  PRIMARY KEY (InvoiceID),
  FOREIGN KEY (PublisherID) REFERENCES Publisher(PublisherID)
);
INSERT INTO Publisher VALUES
('P001','Reinger Group','Mccormick Street'),
('P002','Daugherty Group','Burrows Street'),
('P003','Wehner Group','Elmside Street'),
('P004','Feest Group','Schlimgen Street'),
('P005','Hane Group','Delladonna Street');

INSERT INTO OrderTbl VALUES
('O001','2020-12-02','2800','B001'),
('O002','2020-12-24','6300','B002' ),
('O003','2020-01-29','1100','B003'),
('O004','2021-01-12','7000','B004'),
('O005','2021-01-21','6800','005'),
('O006','2021-01-25','1200','B006');

INSERT INTO Book VALUES
('B001','The Smurfs','69-531-7584','8','100','P001'),
('B002','The Italian Job','56-203-6485','18','200','P002'),
('B003','O Horten','84-737-1902','14','300','P003'),
('B004','Stand by Me Doraemon','21-941-9475','20','400','P004'),
('B005','Atomic Twister','74-412-0200','13','500','P005'),
('B006','High School High','64-571-1632','15','600','P002');

INSERT INTO Cart VALUES
('C001','2021-01-09','Delivered','1000'),
('C002','2021-01-17','Delivered','1500'),
('C003','2021-01-19','Delivered','100'),
('C004','2021-01-25','Delivered','400'),
('C005','2021-01-27','Delivered','300'),
('C006','2021-02-01','Not Delivered','400');

INSERT INTO has VALUES
('OD01','O001','B002','14','2800'),
('OD02','O002','B003','20','6000'),
('OD03','O002','B001','3','300'),
('OD04','O003','B001','11','1100'),
('OD05','O004','B005','14','7000'),
('OD06','O005','B004','17','6800'),
('OD07','O006','B003','4','1200');

INSERT INTO holds VALUES
('CD01','C001','B003','2','600'),
('CD02','C001','B004','1','400'),
('CD03','C002','B005','3','1500'),
('CD04','C003','B001','1','100'),
('CD05','C004','B002','2','400'),
('CD06','C005','B003','1','300'),
('CD07','C006','B004','1','400');

INSERT INTO Invoice VALUES
('I001','2020-12-03','P001','O001','2800'),
('I002','2020-12-25','P001','O002','6300'),
('I003','2020-12-30','P002','O003','1100'),
('I004','2021-01-13','P003','O004','7000'),
('I005','2021-01-22','P004','O005','6800'),
('I006','2021-01-26','P005','O006','1200');

INSERT INTO Publisher_PublisherContact VALUES
('P001','2926330264','8979870217'),
('P002','7991230545','8869542597'),
('P003','8088143740','7542549821'),
('P004','4625826402','7895418874'),
('P005','2064784677','7895487515');


/*To select All records from Book table*/
SELECT *
FROM Book
/*6 records are found*/

/*Total book each publisher publishes*/
SELECT b.PublisherID, p.PublisherName, COUNT(b.BookID) AS TotalBookPublished
FROM Publisher p, Book b
WHERE p.PublisherID=b.PublisherID GROUP BY b.PublisherID,p.PublisherName;
/*5 records are found with various publishers name and total books published*/ 

/*Invoice from various publishers*/
SELECT i.InvoiceID, i.InvoiceDate, i.PublisherID, p.PublisherName, p.PublisherAddress
FROM Invoice i, Publisher p
WHERE i.PublisherID=p.PublisherID
/*6 records are found with PublisherName,PublisherAddress and with Invoice ID and Invoice Date*/

/*Avg Book quantity*/
SELECT Avg(BookQuantity) From Book;
/* 14.66 is the  average bookquantity*/

/* List all the bookquantity which is greather than average bookquantity*/ 
SELECT BookID, BookTitle, BookSerialNumber, BookQuantity, BookPrice
FROM Book
WHERE BookQuantity > (SELECT AVG(BookQuantity) AS AVGQuantity From Book);
/*3 records are found with bookquantity greater than average quantity*/

/*List of books and total price as added in shopping cart*/
SELECT b.BookTitle, b.BookPrice, cd.CartQuantity,c.CartTotal
FROM Book b, Cart c, holds as cd
WHERE b.BookID=cd.BookID AND cd.CartID=c.CartID
/*7 records are found with books total price which was added in the cart along with cart total*/
