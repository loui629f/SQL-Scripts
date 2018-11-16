DROP TABLE PET;
DROP TABLE PET_OWNER;
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
/* *** SQL-INSERT-CH??-01 *** */
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










DROP TABLE PET_2
	CREATE TABLE PET_2(
PetID				INT NOT NULL,
PetName				NVARCHAR(100) NOT NULL,
PetType				NVARCHAR(100) NOT NULL,
PetBreed			NVARCHAR(100) NOT NULL,
PetDOB				DATE NULL,
PetWeight			FLOAT NOT NULL,
OwnerID				int NOT NULL,
CONSTRAINT PET_OWNER_FKv2 FOREIGN KEY (OwnerID)
	REFERENCES PET_OWNER (OwnerID)
		ON DELETE NO ACTION,
CONSTRAINT PET_PKv2 PRIMARY KEY (PetID)
);
CREATE TABLE BREED(
BreedName NVarChar(100) NOT NULL UNIQUE,
MinWeight	Float	NULL,
MaxWeight	Float	NULL,
AverageLifeExpendency Int NULL,
)

INSERT INTO PET_2 VALUES (1, 'King', 'Dog', 'Std. Poodle', '21.02.2011', 25.5, 1)
INSERT INTO PET_2 VALUES (2, 'Teddy', 'Cat', 'Cashmere', '01.02.2012', 10.5, 2)
INSERT INTO PET_2 VALUES (3, 'Fido', 'Dog', 'Std. Poodle', '17.07.2010', 28.5, 1)
INSERT INTO PET_2 VALUES (4, 'AJ', 'Dog', 'Collie Mix', '05.05.2011', 20.0, 3)
INSERT INTO PET_2 VALUES (5, 'Cedro', 'Cat', 'Unknown', '06.06.2009', 9.5, 2)
INSERT INTO PET_2(PetID, PetName, PetType, PetBreed, PetWeight, OwnerID) VALUES (6, 'Wooley', 'Cat', 'Unknown', 9.5, 2)
INSERT INTO PET_2 VALUES (7, 'Buster', 'Dog', 'Border Collie', '11.12.2008', 25.0, 4)


DROP TABLE BREED 
CREATE TABLE BREED(
BreedName NVarChar(100) NOT NULL UNIQUE,
MinWeight	Float	NULL,
MaxWeight	Float	NULL,
AverageLifeExpendency Int NULL,
)

ALTER TABLE PET_2 DROP PetTest 
ALTER TABLE PET_2
ADD CONSTRAINT breedname_FK FOREIGN KEY (PetBreed) 
REFERENCES BREED(BreedName)

INSERT INTO BREED VALUES('Border Collie', 15, 22.5, 20)
INSERT INTO BREED VALUES('Cashmere', 10, 15, 12)
INSERT INTO BREED VALUES('Collie Mix', 17.5, 25, 18)
INSERT INTO BREED VALUES('Std. Poodle', 25.5, 30, 18)
INSERT INTO BREED (BreedName) VALUES ('Unknown')

SELECT OwnerFirstName, OwnerLastName, OwnerEmail
FROM PET_OWNER
WHERE OwnerID in (SELECT OwnerID
				FROM PET
				WHERE PetBreed in (SELECT BreedName
									FROM BREED
									WHERE AverageLifeExpendency > 15))
