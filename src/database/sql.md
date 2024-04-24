# SQL

> 💬 Chưa chuyển sang tiếng Việt

## CREATE Database

```sql
CREATE DATABASE <database_name>;
```

## CREATE Table

```sql
-- here is the comment
CREATE TABLE <table_name>;

CREATE TABLE Users(
	id INT PRIMARY KEY AUTO_INCREMENT,
	email VARCHAR(255) NOT NULL UNIQUE,
	bio TEXT,
	country VARCHAR(2)
);
```

## INSERT New Data

```sql
-- insert single values
INSERT INTO Users (email, bio, country)
VALUES(
	'hello@world.com',
	'i love you!',
	'US'
);

-- insert multiple values
INSERT INTO Users (email, bio, country)
VALUES
	('hello@world.com','i love you!','US'),
	('hola@munda.com','foo','MX'),
	('bonjour@monde.com','bar','FR'),
);
```

## Read or SELECT Data

```sql
-- select all columns
SELECT * FROM Users;

-- select specific columns
SELECT email, id FROM Users;

-- limit amount of data will be fetched
SELECT email, id FROM Users LIMIT 2;

-- sort
SELECT email, id FROM Users
ORDER BY id ASC
LIMIT 2;

-- filter
SELECT email, id FROM Users
WHERE country = 'US'
ORDER BY id ASC
LIMIT 2;

-- logical AND OR
SELECT email, id FROM Users
WHERE country = 'US' AND id > 1
ORDER BY id ASC
LIMIT 2;

-- LIKE matching pattern | kinda slow
SELECT email, id FROM Users
WHERE country = 'US' AND email LIKE 'h%'
ORDER BY id ASC
LIMIT 2;
```

## INDEX for read speed

```sql
CREATE INDEX email_index ON Users(email);
```

## JOIN a relationship

![[Pasted image 20230613104608.png]]
Create mock table for example:

```sql
-- create rooms table
CREATE TABLE Rooms(
	id INT AUTO_INCREMENT,
	street VARCHAR(255),
	owner_id INT NOT NULL,
	PRIMARY KEY (id),
	FOREIGN KEY (owner_id) REFERENCES Users(id)
)

-- insert data
INSERT INTO Rooms (owner_id, street)
VALUES
	(1, 'san diego sailboat'),
	(1, 'nantucket cottage'),
	(1, 'vail cabin'),
	(1, 'sf cardboard box'),
);
```

![](/assets/sql.png)

### INNER JOIN

Returns only the rows where there is a match between the columns in both tables.

```sql
SELECT * FROM Users
INNER JOIN Rooms
ON Rooms.owner_id = Users.id
```

### OUTER JOIN

Returns all the rows from both tables, matching them where possible. If there is no match, NULL values are returned for the columns of the table that doesn't have a match.

```sql
SELECT * FROM Users
OUTER JOIN Rooms
ON Rooms.owner_id = Users.id
```

### LEFT JOIN

Returns all the rows from the right table and the matching rows from the left table. If there is no match, NULL values are returned for the columns of the left table.

> In our case, we want to show all the users who have room and don't have room. Just simply using Left Join query.

```sql
SELECT * FROM Users
LEFT JOIN Rooms
ON Rooms.owner_id = Users.id
```

### RIGHT JOIN

Returns all the rows from the left table and the matching rows from the right table. If there is no match, NULL values are returned for the columns of the right table.

> Right Join is just an opposite of Left Join.

```sql
SELECT * FROM Users
RIGHT JOIN Rooms
ON Rooms.owner_id = Users.id
```

## SELECT Alias

```sql
SELECT
	Users.id AS user_id,
	Rooms.id AS room_id,
	email,
	street
FROM Users
INNER JOIN Rooms
ON Rooms.owner_id = Users.id
```

## JOIN through a middle man

> User has booked many **rooms**
> and room has been booked by **users**.

```sql
CREATE TABLE Bookings(
	id INT AUTO_INCREMENT,
	guest_id INT NOT NULL,
	room_id INT NOT NULL,
	check_in DATETIME,
	PRIMARY KEY (id),
	FOREIGN KEY (guest_id) REFERENCES Users(id),
	FOREIGN KEY (room_id) REFERENCES Rooms(id),
)
```

For example, we can get all the rooms that user has been booked with this query:

```sql
SELECT
	guest_id,
	street,
	check_in
FROM Bookings
INNER JOIN Rooms ON Rooms.owner_id = guest_id
WHERE guest_id = 1;
```

Or get a history of all the guests who stayed in a room:

```sql
SELECT
	room_id,
	guess_id,
	email,
	bio
FROM Bookings
INNER JOIN Users ON Users.owner_id = guest_id
WHERE room_id = 2;
```

## DROP everything

```sql
DROP TABLE Users;
DROP TABLE Rooms;
DROP TABLE Bookings;

DROP DATABASE <database_name>;
```
