**CREATE TABLES**

CREATE TABLE PET_OWNER(
OwnerID				INT NOT NULL,
OwnerLastName		NVARCHAR(100) NOT NULL,
OwnerFirstName		NVARCHAR(100) NOT NULL,
OwnerPhone			NVARCHAR(100) NULL,
OwnerEmail			NVARCHAR(100) NOT NULL UNIQUE,
CONSTRAINT PET_OWNER_PK PRIMARY KEY (OwnerID)
);

CREATE TABLE PET(
PetID				INT NOT NULL,
PetName				NVARCHAR(100) NOT NULL,
PetType				NVARCHAR(100) NOT NULL,
PetBreed			NVARCHAR(100) NOT NULL,
PetDOB				DATE NULL,
PetWeight			FLOAT NOT NULL,
OwnerID				int NOT NULL,
CONSTRAINT PET_OWNER_FK FOREIGN KEY (OwnerID)
	REFERENCES PET_OWNER (OwnerID)
		ON DELETE NO ACTION,
CONSTRAINT PET_PK PRIMARY KEY (PetID)
);


USE A_DB17_2018
GO
INSERT INTO PET_OWNER VALUES (1, 'Downs', 'Marsha', '555 537 8765', 'Marsha.Downs@somewhere.com')
INSERT INTO PET_OWNER VALUES (2, 'James', 'Richard', '555 537 7654', 'Richard.James@somewhere.com')
INSERT INTO PET_OWNER VALUES (3, 'Frier', 'Liz', '555 537 6543', 'Liz.Frier@somewhere.com')
INSERT INTO PET_OWNER(OwnerID, OwnerLastName, OwnerFirstName, OwnerEmail) VALUES (4, 'Trent', 'Miles', 'Miles.Trent@somewhere.com')

INSERT INTO PET VALUES (1, 'King', 'Dog', 'Std. Poodle', '21.02.2011', 25.5, 1)
INSERT INTO PET VALUES (2, 'Teddy', 'Cat', 'Cashmere', '01.02.2012', 10.5, 2)
INSERT INTO PET VALUES (3, 'Fido', 'Dog', 'Std. Poodle', '17.07.2010', 28.5, 1)
INSERT INTO PET VALUES (4, 'AJ', 'Dog', 'Collie Mix', '05.05.2011', 20.0, 3)
INSERT INTO PET VALUES (5, 'Cedro', 'Cat', 'Unknown', '06.06.2009', 9.5, 2)
INSERT INTO PET(PetID, PetName, PetType, PetBreed, PetWeight, OwnerID) VALUES (6, 'Wooley', 'Cat', 'Unknown', 9.5, 2)
INSERT INTO PET VALUES (7, 'Buster', 'Dog', 'Border Collie', '11.12.2008', 25.0, 4)

**STORED PROCEDURE**

CREATE PROC InsertPet
(
	@PetID			INT				=NULL,
	@PetName		NVARCHAR(16)	=NULL,
	@PetType		NVARCHAR(16)	=NULL,
	@PetBreed		NVARCHAR(100)	=NULL,
	@PetDOB			DATETIME2		=NULL,
	@PetWeight		FLOAT(1)		=NULL,
	@OwnerID		INT				=NULL
)
AS
	BEGIN
		INSERT INTO PET (PetID, PetName, PetType, PetBreed, PetDOB, PetWeight, OwnerID)
		VALUES (@PetID, @PetName, @PetType, @PetBreed, @PetDOB, @PetWeight, @OwnerID)
	END
	
	
EXEC InsertPet @PetID = 21, @PetType = 'CAT', @PetBreed = 'Unknown', @PetWeight = 20.0, @OwnerID = 1, @PetName = 'kat'

CREATE PROC InsertOwner
(
@OwnerID			INT				=NULL,
@OwnerLastName		VARCHAR(100)	=NULL,
@OwnerFirstName		VARCHAR(100)	=NULL,
@OwnerPhone			VARCHAR(100)	=NULL,
@OwnerEmail			VARCHAR(100)	=NULL
)

AS
	BEGIN
		INSERT INTO PET_OWNER (OwnerID,OwnerLastName,OwnerFirstName,OwnerPhone,OwnerEmail)
		VALUES (@OwnerID,@OwnerLastName,@OwnerFirstName,@OwnerPhone,@OwnerEmail)
	END


EXEC InsertOwner @OwnerID = 20, @OwnerLastName = 'Downs', @OwnerFirstName = 'Marsha', @OwnerEmail = 'Marsha.Downs@some.com'


CREATE PROC	InsertBreed
(
@BreedName					NVARCHAR(100)	=NULL,
@MinWeight					FLOAT			=NULL,
@MaxWeight					FLOAT			=NULL,
@AverageLifeExpendency		INT				=NULL
)

AS
	BEGIN
		INSERT INTO BREED (BreedName,MinWeight,MaxWeight,AverageLifeExpendency)
		VALUES (@BreedName,@MinWeight,@MaxWeight,@AverageLifeExpendency)
	END

EXEC InsertBreed @BreedName = 'kat'

**Ex31-Implementer tabeller**

CREATE TABLE Ex31Person(
Id			INT IDENTITY (1, 1),
Navn		NVARCHAR(100)	NOT NULL
CONSTRAINT PK_Ex31Person PRIMARY KEY (Id)
);

CREATE TABLE Ex31Læge(
Id			INT			NOT NULL,
Lønramme	NVARCHAR(1)	NOT NULL
CONSTRAINT FK_Ex31Læge FOREIGN KEY (Id)
	REFERENCES Ex31Person (Id),
CONSTRAINT PK_Ex31Læge PRIMARY KEY (Id)
);

go
alter PROC spIndsætLæge(
@Navn			NVARCHAR(100),
@Lønramme		NVARCHAR(1)
)
AS
	BEGIN
		INSERT INTO Ex31Person VALUES (@Navn)
DECLARE @LastValue AS INT;
SET @LastValue = (SELECT SCOPE_IDENTITY());
		INSERT INTO Ex31Læge VALUES (@LastValue,@Lønramme)
		END
		
		
**CREATE VIEWS**

CREATE VIEW AllEmployees AS
SELECT E.Emp_Id, E.FName, E.LName, D.Dept_Name, Z.Zip, Z.City, M.Manager
FROM EX62_Employee E INNER JOIN EX62_Department D ON E.Dep_Id=D.Dep_Id
	INNER JOIN EX62_ZipCity Z ON E.Zip=Z.Zip
	INNER JOIN EX62_Manager M ON D.Dep_Id=M.Dep_Id


	SELECT *
	FROM AllEmployees

CREATE VIEW AllOrderLines AS
SELECT O.Order_Id, C.FName, C.LName, CONVERT(DATE, Order_Date) AS Order_date, P.Prod_Name, P.Price, OL.Amount
FROM EX62_Order O INNER JOIN EX62_Customer C ON O.Customer=C.Customer_Id
	INNER JOIN EX62_OrderLine OL ON O.Order_Id=OL.Order_Id
	INNER JOIN EX62_Product P ON OL.Product_Id=P.Product_Id

SELECT *
FROM AllOrderLines

CREATE VIEW Administration AS
SELECT E.FName, E.LName, Z.Zip, Z.City 
FROM EX62_Employee E INNER JOIN EX62_ZipCity Z ON E.Zip=Z.Zip
INNER JOIN EX62_Department D ON E.Dep_Id=D.Dep_Id
WHERE Dept_Name = 'Administration'

SELECT *
FROM Administration


**CREATE TRIGGERS**

CREATE TRIGGER trAfterDeleteCustomer ON Ex62_Customer
AFTER DELETE
AS
	declare @id int
	declare @fname nvarchar(40)
	declare @lname nvarchar(40)
	declare @zip nvarchar (4)

	select @id=d.Customer_Id FROM deleted d
	select @fname=d.FName from deleted d
	select @lname=d.LName from deleted d
	select @zip=d.Zip from deleted d

	INSERT INTO EX62_DeletedCustomer
	values (@id,@fname,@lname,@zip)

	PRINT 'hej'

	SELECT * FROM EX62_Customer
	SELECT * FROM EX62_DeletedCustomer
delete from EX62_Customer where Customer_Id=1


**EKSAMEN**

create table EVENT(
EventId  			INT IDENTITY,
EventName			NVARCHAR(100) NOT NULL,
EventDate			DATETIME NOT NULL,
EventDescription		NVARCHAR(100) NOT NULL,
EventConfirmed			BIT NOT NULL,
constraint EVENT_PK Primary key (EventId));
		

create table USERS(
UserEmail	NVARCHAR(50) NOT NULL UNIQUE,
UserName	NVARCHAR(50) NOT NULL,
UserPassword	NVARCHAR(50) NOT NULL,
constraint USER_PK primary key (UserEmail));


create table COMMENT(
CommentId		INT IDENTITY,
UserEmail		NVARCHAR(50) NOT NULL UNIQUE,
Comment			NVARCHAR(1000) NOT NULL,
constraint COMMENT_PK primary key (CommentId),
constraint COMMENT_FK foreign key (UserEmail)
		references USERS (UserEmail)
				on delete no action);

create table EVENT_SHIFT(
ShiftId			INT IDENTITY,
EventId			INT NOT NULL,
UserEmail		NVARCHAR(50) NOT NULL,
ShiftDate		DATETIME2 NOT NULL,
ShiftType		NVARCHAR(15) NOT NULL,
constraint EVENT_SHIFT_PK primary key (ShiftId),
constraint EVENT_FK1 foreign key (EventId)
		references EVENT (EventId)
			on delete no action,
constraint EVENT_FK2 foreign key (UserEmail)
		references USERS (UserEmail)
			on delete no action);
			

create procedure spSelectNotConfirmedEvents
as
select * from EVENT where EventConfirmed = 0;


create procedure spSelectAllShifts
as
select * from EVENT_SHIFT
go;

create procedure spSelectAllUsers
as
select * from USERS
go;

create procedure spSelectAllComments
as
select * from COMMENT
go;

create procedure spSelectConfirmedEvents
as
select * from EVENT where EventConfirmed = 1;


create procedure spInsertEventVolunteer @EventName nvarchar(100), @EventDate datetime, @EventDescription nvarchar(100)
as
insert into [EVENT]
([EventName],
[EventDate],
[EventDescription],
[EventConfirmed])
values
(@EventName,
@EventDate,
@EventDescription,
0)
go

create procedure spInsertEventAdmin @EventName nvarchar(100), @EventDate datetime, @EventDescription nvarchar(100)
as
insert into [EVENT]
([EventName],
[EventDate],
[EventDescription],
[EventConfirmed])
values
(@EventName,
@EventDate,
@EventDescription,
1)
go
