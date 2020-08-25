```sql
CREATE TABLE Customer 
(
	CustomerID CHAR(7) CONSTRAINT customer_pk PRIMARY KEY,
	FirstName VARCHAR(50) CONSTRAINT custm_first_nn NOT NULL,
	LastName VARCHAR(50) CONSTRAINT custm_last_nn NOT NULL,
	Phone CHAR(10)
		CONSTRAINT custm_phone_nn NOT NULL
		CONSTRAINT custm_phone_digits CHECK (REGEXP_LIKE (Phone, '\d{10}')),
	Address VARCHAR(50) CONSTRAINT custm_addr_nn NOT NULL,
	Address2 VARCHAR(50),
	City VARCHAR(50) CONSTRAINT custm_city_nn NOT NULL,
	State CHAR(2) CONSTRAINT custm_state_nn NOT NULL,
	ZipCode VARCHAR(50)
		CONSTRAINT custm_zip_nn NOT NULL
		CONSTRAINT custm_zip_digits CHECK (REGEXP_LIKE (ZipCode, '\d{5}'))
);

CREATE TABLE PaymentCard
(
	CardNumber VARCHAR(16), 
	CVV VARCHAR(4), 
	CustomerID CHAR(7) CONSTRAINT pay_custm_nn  NOT NULL,
	Expiration DATE CONSTRAINT pay_exp_nn NOT NULL,
	Name VARCHAR(50),
	CONSTRAINT pay_num_digits CHECK (REGEXP_LIKE (CardNumber, '^\d{15,16}$')),
	CONSTRAINT pay_cvv_digits CHECK (REGEXP_LIKE (CVV, '^\d{3,4}$')),
    CONSTRAINT payment_pk PRIMARY KEY(CardNumber, CVV),    
	CONSTRAINT payment_fk FOREIGN KEY(CustomerID) REFERENCES Customer(CustomerID)
		ON DELETE CASCADE
);

CREATE TABLE Employee
(
	EmployeeID CHAR(5) CONSTRAINT employee_pk PRIMARY KEY,
	FirstName VARCHAR(50) CONSTRAINT empl_first_nn NOT NULL,
	LastName VARCHAR(50) CONSTRAINT empl_last_nn NOT NULL,
	Phone CHAR(10) 
		CONSTRAINT empl_phone_nn NOT NULL,
		CONSTRAINT empl_phone_digits CHECK (REGEXP_LIKE (Phone, '\d{10}')),
	Title VARCHAR(10)
		CONSTRAINT empl_title_nn NOT NULL,
		CONSTRAINT empl_title_valid CHECK (Title IN ('MANAGER','STAFF')),
	EmploymentDate DATE CONSTRAINT empl_date_nn NOT NULL
);

CREATE TABLE Film
(
	FilmID CHAR(9) CONSTRAINT film_pk PRIMARY KEY,
	Title VARCHAR(50) CONSTRAINT film_title_nn NOT NULL,
	Director VARCHAR(50),
	Year NUMBER(4) CONSTRAINT film_year CHECK (Year > 1880),
	MPAA VARCHAR(10) CONSTRAINT film_mpaa_ratings 
		CHECK (MPAA IN ('Unrated','G','PG','PG-13','R','NC-17','X')),
	Runtime NUMBER(4) CONSTRAINT positive_runtime CHECK (Runtime > 0)
);

CREATE TABLE FilmCopy
(
	FilmID CHAR(9),
	CopyNumber NUMBER(3),
	Format VARCHAR(10) 
		CONSTRAINT copy_format_nn NOT NULL
		CONSTRAINT copy_format_valid CHECK (Format IN ('VHS','DVD','BLURAY')),
	CheckedOut CHAR(1)
		CONSTRAINT copy_chkdout_nn NOT NULL
		CONSTRAINT copy_chkdout_valid CHECK (CheckedOut IN ('Y','N')),
	CONSTRAINT filmcopy_pk PRIMARY KEY (FilmID, CopyNumber),
	CONSTRAINT filmcopy_id_fk FOREIGN KEY (FilmID) REFERENCES Film(FilmID)
);

CREATE TABLE Rental
(
	RentalID CHAR(9) CONSTRAINT rental_pk PRIMARY KEY,
	CustomerID CHAR(7) CONSTRAINT rental_custm_nn NOT NULL,
	EmployeeID CHAR(5) CONSTRAINT rental_empl_nn NOT NULL,
	FilmID CHAR(9) CONSTRAINT rental_film_nn NOT NULL,
	CopyNumber NUMBER(3) CONSTRAINT rental_copy_nn NOT NULL,
	RentalDate DATE DEFAULT SYSDATE CONSTRAINT rental_date_nn NOT NULL,
	RentalPeriod NUMBER(2) 
		CONSTRAINT rental_period_nn NOT NULL
		CONSTRAINT valid_rental_period CHECK (RentalPeriod >= 1),
	Cost NUMBER(3,2),
	ReturnDate DATE,
	CONSTRAINT valid_return_date CHECK (ReturnDate > RentalDate),
	CONSTRAINT rental_custmr_fk FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
	CONSTRAINT rental_empl_fk FOREIGN KEY (EmployeeID) REFERENCES Employee(EmployeeID),
	CONSTRAINT rental_filmid_fk FOREIGN KEY (FilmID, CopyNumber) REFERENCES FilmCopy(FilmID, CopyNumber)
);

CREATE TABLE LateFee
(
	RentalID CHAR(9) CONSTRAINT fee_rental_fk REFERENCES Rental(RentalID),
	DateIncurred DATE DEFAULT SYSDATE,
	CONSTRAINT fee_pk PRIMARY KEY (RentalID, DateIncurred),
	Amount NUMBER(3,2)
		CONSTRAINT fee_amount_nn NOT NULL
		CONSTRAINT fee_amount_valid CHECK (Amount > 0),
	AmountUnpaid NUMBER(3,2) -- DEFAULT Amount
);

CREATE TABLE SaleItem
(
	ItemID CHAR(7) CONSTRAINT saleitem_pk PRIMARY KEY,
	Title VARCHAR(50),
	Price NUMBER(4,2) 
		CONSTRAINT saleitem_price_nn NOT NULL
		CONSTRAINT saleitem_price_valid CHECK (Price >= 0),
	Stock NUMBER(4) CONSTRAINT saleitem_stock_valid CHECK (Stock >= 0)
);

CREATE TABLE Sale
(
	ItemID CHAR(7) CONSTRAINT sale_item_fk REFERENCES SaleItem(ItemID),
	EmployeeID CHAR(5) CONSTRAINT sale_empl_fk REFERENCES Employee(EmployeeID),
	PurchaseDate DATE DEFAULT SYSDATE,
	Quantity Number(3) CONSTRAINT sale_quantity_nn NOT NULL,
	CONSTRAINT sale_pk PRIMARY KEY (ItemID, EmployeeID, PurchaseDate)
);

INSERT INTO Customer (CustomerID, FirstName, LastName, Phone, Address, Address2, City, State, ZipCode)
VALUES ('C379265','Glenn','Springfield','2153876458','201 29th St','Apt 14','Philadelphia','PA','19111');
INSERT INTO Customer (CustomerID, FirstName, LastName, Phone, Address, City, State, ZipCode)
VALUES ('C947523','Erin','Brown','2021894657','3309 Spruce St','Philadelphia','PA','19103');
INSERT INTO Customer (CustomerID, FirstName, LastName, Phone, Address, Address2, City, State, ZipCode)
VALUES ('C998547','Jerry','Randall','9172298745','1112 3rd St','Apt 5A','New York','NY','10036');


INSERT INTO PaymentCard (CardNumber, CVV, CustomerID, Expiration, Name)
VALUES ('1111111111111111','123','C379265','1-MAY-2024','Glenn Springfield');
INSERT INTO PaymentCard (CardNumber, CVV, CustomerID, Expiration, Name)
VALUES ('222222222222222','0456','C947523','13-SEP-2023','John Brown');
INSERT INTO PaymentCard (CardNumber, CVV, CustomerID, Expiration, Name)
VALUES ('3333333333333333','789','C998547','23-JAN-2022','Gerald Randall');
INSERT INTO PaymentCard (CardNumber, CVV, CustomerID, Expiration, Name)
VALUES ('4444444444444444','341','C947523','16-JUN-2020','John Brown');


INSERT INTO Employee (EmployeeID, FirstName, LastName, Phone, Title, EmploymentDate)
VALUES ('E6871','Don','Hickory','2159876561','STAFF','4-JAN-2019');
INSERT INTO Employee (EmployeeID, FirstName, LastName, Phone, Title, EmploymentDate)
VALUES ('E1421','Sarah','Sandwiches','2227632895','STAFF','13-JUN-2020');
INSERT INTO Employee (EmployeeID, FirstName, LastName, Phone, Title, EmploymentDate)
VALUES ('E9587','Charles','Benoit','2159687450','MANAGER','22-AUG-2015');

INSERT INTO Film (FilmID, Title, Director, Year, MPAA, Runtime)
VALUES ('M95874026','Lethal Weapon','Richard Donner',1987,'R',109);
INSERT INTO Film (FilmID, Title, Director, Year, MPAA, Runtime)
VALUES ('M97946526','Planes, Trains And Automobiles','John Hughes',1987,'R',93);
INSERT INTO Film (FilmID, Title, Director, Year, MPAA, Runtime)
VALUES ('M56455567','Minority Report','Steven Spielberg',2002,'PG-13',145);
INSERT INTO Film (FilmID, Title, Director, Year, MPAA, Runtime)
VALUES ('M99461576','Good Will Hunting','Gus Van Sant',1997,'R',126);
INSERT INTO Film (FilmID, Title, Director, Year, MPAA, Runtime)
VALUES ('M56784348','The Exorcist','William Friedkin',1973,'R',122);
INSERT INTO Film (FilmID, Title, Director, Year, MPAA, Runtime)
VALUES ('M11019573','Jackie Brown','Quentin Tarantino',1997,'R',154);

INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M95874026',1,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M95874026',2,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M95874026',3,'BLURAY','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M97946526',1,'DVD','Y');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M56455567',1,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M56455567',2,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M99461576',1,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M99461576',2,'BLURAY','Y');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M56784348',1,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M56784348',2,'DVD','N');
INSERT INTO FilmCopy (FilmID, CopyNumber, Format, CheckedOut)
VALUES ('M11019573',1,'DVD','N');

INSERT INTO Rental (RentalID, CustomerID, EmployeeID, FilmID, CopyNumber, RentalDate, RentalPeriod, Cost)
VALUES ('R76503774','C379265','E6871','M99461576',2,'2-AUGUST-2020',7,6.99);
INSERT INTO Rental (RentalID, CustomerID, EmployeeID, FilmID, CopyNumber, RentalPeriod, Cost)
VALUES ('R33024533','C947523','E9587','M97946526',1,7,4.99);
INSERT INTO Rental (RentalID, CustomerID, EmployeeID, FilmID, CopyNumber, RentalPeriod, Cost)
VALUES ('R33049856','C947523','E9587','M99461576',1,7,4.99);
INSERT INTO Rental (RentalID, CustomerID, EmployeeID, FilmID, CopyNumber, RentalDate, RentalPeriod, Cost, ReturnDate)
VALUES ('R88123231','C947523','E1421','M95874026',1,'1-JULY-2020',14,9.99,'11-JUL-2020');

INSERT INTO LateFee (RentalID, Amount, AmountUnpaid)
VALUES ('R76503774','9.99','9.99');
INSERT INTO LateFee (RentalID, DateIncurred, Amount, AmountUnpaid)
VALUES ('R76503774','10-AUGUST-2020','9.99','9.99');
INSERT INTO LateFee (RentalID, DateIncurred, Amount, AmountUnpaid)
VALUES ('R76503774','17-AUGUST-2020','9.99','9.99');

INSERT INTO SaleItem (ItemID, Title, Price, Stock)
VALUES ('S368745','raisinets-small',1.99,45);
INSERT INTO SaleItem (ItemID, Title, Price, Stock)
VALUES ('S847564','pulp-fiction-poster',19.99,6);
INSERT INTO SaleItem (ItemID, Title, Price, Stock)
VALUES ('S120586','store-shirt-size-L',24.95,38);

INSERT INTO Sale (ItemID, EmployeeID, Quantity)
VALUES ('S368745','E6871',2);
INSERT INTO Sale (ItemID, EmployeeID, Quantity)
VALUES ('S120586','E6871',1);
INSERT INTO Sale (ItemID, EmployeeID, Quantity)
VALUES ('S847564','E1421',1);
```
