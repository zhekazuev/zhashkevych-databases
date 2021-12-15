Learning SQL / PostgreSQL with zhashkevych

## Links
[SQL with PostgreSQL Video](https://www.youtube.com/watch?v=i5-1HNf3W_Y)

## Steps
Create Docker cCntainer with Postgres
```bash
docker run --name postgres_test -e POSTGRES_PASSWORD=qwerty --rm -d postgres
```

Go to Postgres Container
```bash
docker exec -it <hash> /bin/bash
```


Go to Postgres
```bash
psql -U postgres
```

### DDL - Data Definition Language

Show all dbs
```bash
\l
```


Show tables in current db - postgres
```bash
\d
```


Create test_db
```bash
CREATE DATABASE test_db;
```


Switch to test_db
```bash
\c test_db
```


Create members table
```bash
CREATE TABLE members (
    id serial not null unique,
    first_name varchar(255) not null,
    last_name varchar(255) not null,
    middle_name varchar(255),
    phone varchar(255) not null unique,
    date_of_birth date not null,
    expires_at timestamp not null
);

/d
```


Crate memberships table
```bash
CREATE TABLE memberships (
    id serial not null unique,
    title varchar(255) not null,
    price int not null,
    duration int not null
);

/d
```


Add membership_id reference for relation members and memberships
```bash
ALTER TABLE members ADD COLUMN membership_id int not null references memberships (id);

/d
```


Create visits table
```bash
CREATE TABLE visits (
    id serial not null unique,
    member_id int not null references (id),
    came_at timestamp not null default now(),
    left_at timestamp not null
);

/d
```


### DML - Data Manipulation Language

Insert into members table with error
```bash
INSERT into members (
    first_name, 
    last_name,
    phone,
    date_of_birth,
    expires_at,
    membership_id
) values (
    "Jack",
    "Pupkin",
    "+123456789",
    "04-03-1998",
    "04-06-2021",
    1
);
Error

```


Insert into memberships
```bash
INSERT into memberships (
    title,
    price,
    duration
) values (
    "Basic 3 mounth",
    2999,
    3
);

```


Select all from memberships
```bash
SELECT * from memberships;
```


Select title and price from memberships
```bash
SELECT title, price from memberships;
```


Insert 2 rows into memberships
```bash
INSERT into memberships (
    title,
    price,
    duration
) values (
    "Basic 6 mounth",
    5999,
    6
);

INSERT into memberships (
    title,
    price,
    duration
) values (
    "Basic 12 mounth",
    8999,
    12
);
```


Select with filter from memberships
```bash
SELECT * from memberships
WHERE duration >= 6;

SELECT * from memberships
WHERE duration >= 6 and duration <= 9;
```


Insert member
```bash
INSERT into members (
    first_name, 
    last_name,
    phone,
    date_of_birth,
    expires_at,
    membership_id
) values (
    "Jack",
    "Pupkin",
    "+123456789",
    "04-03-1998",
    "04-06-2021",
    1
);

SELECT * from members;
```


Insert member with non-unique phone
```bash
INSERT into members (
    first_name, 
    last_name,
    phone,
    date_of_birth,
    expires_at,
    membership_id
) values (
    "John",
    "Pupkin",
    "+123456789",
    "04-03-1998",
    "04-06-2021",
    1
);
Error with non-unique phone

```


Insert member with unique phone
```bash
INSERT into members (
    first_name, 
    last_name,
    phone,
    date_of_birth,
    expires_at,
    membership_id
) values (
    "John",
    "Pupkin",
    "+123456788",
    "04-03-1998",
    "04-06-2021",
    1
);

SELECT * from members;
```


Insert member with middle_name
```bash
INSERT into members (
    first_name, 
    last_name,
    middle_name,
    phone,
    date_of_birth,
    expires_at,
    membership_id
) values (
    "Kate",
    "Hats",
    "Web",
    "+123456787",
    "04-03-2000",
    "04-09-2021",
    6
);

SELECT * from members;
```


Updating middle_name for member
```bash
UPDATE members 
SET middle_name="Zuaz"
WHERE id=2;

SELECT * from members;
```


Updating membership_id for member
```bash
UPDATE members 
SET membership_id=4
WHERE id=2;
Error - membership_id=4 don't exist
```


Updating middle_name for member
```bash
UPDATE members 
SET middle_name="Zuaz"
WHERE id=2;

SELECT * from members;
```


Change column left_at for visits
```bash
ALTER TABLE visits 
ALTER COLUMN left_at
DROP not null;

/d
```


Insert visit for client with id=4
```bash
INSERT into visits (
    member_id
) values (
    4
);

SELECT * from visits;
```


### JOINS and DELETE

SELECT info about member with visits
```bash
SELECT v.id, m.first_name, m.phone, v.came_at 
FROM visits as v
JOIN members as m
ON v.member_id = m.id;
```

DELETE membership
```bash
DELETE FROM memberships 
WHERE id = 3;

SELECT * FROM memberships;
```
