# access psql database

    sudo -u postgres psql

# inside PSQL shell

    psql

    psql (13.2 (Ubuntu 13.2-1.pgdg20.10+1))
    Type "help" for help.

# change user password

    postgres=# ALTER USER postgres PASSWORD 'yourpassword';
    ALTER ROLE

# Postgre commands

display help

    postgres=# help

display help with SQL command

    postgres=# \h

display help with postgreSQL command

    postgres=# \?

quit the database

    postgres=# \q

display list of roles

    postgres=# \du

create a database

    postgres=# CREATE DATABASE test;
    CREATE DATABASE

display list of databases

    postgres=# \l

display list of relation databases

    postgres=# \d

connect to database

    postgres=# \c test

create a table

    CREATE TABLE person (
    id BIGSERIAL NOT NULL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    gender VARCHAR(7) NOT NULL,
    date_of_birth DATE NOT NULL,
    email VARCHAR(10) );

drop database

    test=# DROP TABLE person;
    DROP TABLE

    test=# \d person
    id | integer | | |
    first_name | character varying(50) | | |
    last_name | character varying(50) | | |
    date_of_birth | timestamp without time zone | | |
    gender | character varying(7) | | |

insert record into table

    postgres=# INSERT INTO person (
    postgres(# first_name,
    postgres(# last_name,
    postgres(# gender,
    postgres(# date_of_birth)
    postgres-#
    postgres-# VALUES ('Anne', 'Smith', 'FEMALE', DATE '1988-01-09');
    INSERT 0 1
    postgres=# \dt
            List of relations
    Schema |  Name  | Type  |  Owner
    --------+--------+-------+----------
    public | person | table | postgres
    (1 row)

# Generate 1000 rows with [Mockaroo](https://mockaroo.com/)

- Generate a table in Mockaroo
- Export the database using SQL (add create table commands):
  person.sql
- Open the file with VSCode and edit the create table command: e.g. add not null

  ```
  create table person (
  id BIGSERIAL NOT NULL PRIMARY KEY,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  email VARCHAR(150),
  gender VARCHAR(17) NOT NULL,
  date_of_birth DATE NOT NULL,
  country_of_birth VARCHAR(50)
  );

  insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values (1, 'Kalie', 'Linley', null, 'Genderqueer', '2020-10-14', 'China');
  insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values (2, 'Ethelyn', 'MacLaughlin', null, 'Polygender', '2020-08-15', 'Syria');
  ```

- load the database in psql shell:
  `\i path_to_person.sql`

- Display the database:
  `SELECT \* FROM person;`

# SELECT FROM

     test=# SELECT * FROM person;
     test=# SELECT first_name FROM person;

# ORDER BY

    test=# SELECT * FROM person
    test-# ORDER BY email;
    test=# SELECT *  FROM person ORDER BY country_of_birth DESC;
    test=# SELECT *  FROM person ORDER BY id DESC;

    test=# SELECT \* FROM person ORDER BY gender, email ASC;

# Distinct

This command remove duplicates

    test=# SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;

         country_of_birth
    ----------------------------------
     Afghanistan
     Albania
     Argentina
     Armenia
     Australia
     Azerbaijan
     Bahamas
     Bangladesh
     Belarus
     Benin
     Bhutan
     Bolivia
     Bosnia and Herzeg

Another example

    test=# SELECT DISTINCT gender FROM person ORDER BY gender;
       gender
    -------------
     Agender
     Bigender
     Female
     Genderfluid
     Genderqueer
     Male
     Non-binary
     Polygender
    (8 rows)

# WHERE clause and AND

The where clause alow to filter the data based on condition

    test=# SELECT \* FROM person WHERE gender='Male';

    id | first_name | last_name | email | gender | date_of_birth | country_of_birth
    -----+--------------+--------------+--------------------------------------+--------+---------------+------------------
    17 | Burk | Stuehmeier | bstuehmeierg@exblog.jp | Male | 2020-11-18 | Sweden
    18 | Keenan | Northill | knorthillh@vkontakte.ru | Male | 2021-04-01 | France
    29 | Eric | Lampke | elampkes@last.fm | Male | 2020-04-07 | Canada
    39 | Dukie | Bonnefin | dbonnefin12@angelfire.com | Male | 2020-04-15 | Mongolia
    53 | Doy | Lowes | | Male | 2020-10-16 | Honduras

Another example

    test=# SELECT * FROM person WHERE gender='Male' AND country_of_birth='France';
     id  | first_name | last_name |          email           | gender | date_of_birth | country_of_birth
    -----+------------+-----------+--------------------------+--------+---------------+------------------
      18 | Keenan     | Northill  | knorthillh@vkontakte.ru  | Male   | 2021-04-01    | France
     135 | Ingar      | Olanda    | iolanda3q@oracle.com     | Male   | 2021-02-03    | France
     571 | Mallissa   | Twiddell  | mtwiddellfu@freewebs.com | Male   | 2020-11-24    | France
     663 | Sergent    | Malarkey  | smalarkeyie@addtoany.com | Male   | 2020-10-07    | France
    (4 rows)

    test=# SELECT * FROM person WHERE gender='Male' AND country_of_birth='France' OR country_of_birth='China';

    	  id  | first_name |   last_name    |               email                |   gender    | date_of_birth | country_of_birth
    -----+------------+----------------+------------------------------------+-------------+---------------+------------------
       1 | Kalie      | Linley         |                                    | Genderqueer | 2020-10-14    | China
      12 | Lillian    | Southorn       | lsouthornb@nytimes.com             | Agender     | 2021-01-12    | China
      16 | Rosa       | Harkess        | rharkessf@elpais.com               | Genderqueer | 2020-07-13    | China
      18 | Keenan     | Northill       | knorthillh@vkontakte.ru            | Male        | 2021-04-01    | France
      21 | Verena     | Kasman         | vkasmank@artisteer.com             | Non-binary  | 2020-08-28    | China
      27 | Rubia      | Wellsman       | rwellsmanq@tripod.com              | Agender     | 2020-05-05    | China
      30 | Maurene    | Kinkaid        | mkinkaidt@nymag.com                | Non-binary  | 2020-06-01    | China

    test=# SELECT * FROM person WHERE gender='Male' AND (country_of_birth='France' OR country_of_birth='China') AND first_name='Mame';

     id  | first_name | last_name | email | gender | date_of_birth | country_of_birth
    -----+------------+-----------+-------+--------+---------------+------------------
     323 | Mame       | Perigo    |       | Male   | 2020-06-29    | China
    (1 row)

# Comparison Operators

Comparison between arithmitic

    test=# SELECT 1 = 1 ;
     ?column?
    ----------
     t   # True
    (1 row)

    test=# SELECT 1 < 0;
     ?column?
    ----------
     f   # False
    (1 row)

    test=# SELECT 'Amine' <> 'Enima';  # different
     ?column?
    ----------
     t
    (1 row)

# Limit, Offset & Fetch

LIMIT

    test=# SELECT * FROM person LIMIT 5;

     id | first_name |  last_name  |          email           |   gender    | date_of_birth | country_of_birth
    ----+------------+-------------+--------------------------+-------------+---------------+------------------
      1 | Kalie      | Linley      |                          | Genderqueer | 2020-10-14    | China
      2 | Ethelyn    | MacLaughlin |                          | Polygender  | 2020-08-15    | Syria
      3 | Bancroft   | Varren      | bvarren2@live.com        | Female      | 2020-08-25    | Nigeria
      4 | Jessamyn   | Eggle       | jeggle3@sohu.com         | Agender     | 2020-05-21    | Croatia
      5 | Andrei     | Dilleston   | adilleston4@symantec.com | Genderqueer | 2020-10-28    | Philippines
    (5 rows)

OFFSET

    test=# SELECT \* FROM person OFFSET 9 LIMIT 2;
    id | first_name | last_name | email | gender | date_of_birth | country_of_birth
    ----+------------+-----------+-------------------+-------------+---------------+------------------
    10 | Ronnie | Gabb | rgabb9@ebay.co.uk | Non-binary | 2020-06-21 | Indonesia
    11 | Aidan | Lebang | | Genderfluid | 2020-06-17 | Thailand
    (2 rows)

Fetch (like LIMIT)

    test=# SELECT * FROM person OFFSET 5 FETCH FIRST 2 ROW ONLY;

     id | first_name |  last_name  |        email         |   gender    | date_of_birth | country_of_birth
    ----+------------+-------------+----------------------+-------------+---------------+------------------
      6 | Maggi      | Dobrowolski |                      | Non-binary  | 2021-01-25    | Indonesia
      7 | Noni       | Scaife      | nscaife6@auda.org.au | Genderfluid | 2020-09-19    | Russia
    (2 rows)

# IN

    test=# SELECT * FROM person WHERE country_of_birth IN ('France', 'China', 'Brazil');

     id  | first_name |   last_name    |               email                |   gender    | date_of_birth | country_of_birth
    -----+------------+----------------+------------------------------------+-------------+---------------+------------------
       1 | Kalie      | Linley         |                                    | Genderqueer | 2020-10-14    | China
      12 | Lillian    | Southorn       | lsouthornb@nytimes.com             | Agender     | 2021-01-12    | China
      16 | Rosa       | Harkess        | rharkessf@elpais.com               | Genderqueer | 2020-07-13    | China
      18 | Keenan     | Northill       | knorthillh@vkontakte.ru            | Male        | 2021-04-01    | France
      21 | Verena     | Kasman         | vkasmank@artisteer.com             | Non-binary  | 2020-08-28    | China
      27 | Rubia      | Wellsman       | rwellsmanq@tripod.com              | Agender     | 2020-05-05    | China

    test=# SELECT * FROM person WHERE country_of_birth IN ('France', 'China', 'Brazil')
    ORDER BY country_of_birth;

# Between

Select data from range

    test=# SELECT * FROM person WHERE date_of_birth BETWEEN '2021-02-01' AND '2021-02-03';

     id  | first_name |   last_name    |         email          |   gender    | date_of_birth | country_of_birth
    -----+------------+----------------+------------------------+-------------+---------------+------------------
     135 | Ingar      | Olanda         | iolanda3q@oracle.com   | Male        | 2021-02-03    | France
     294 | Janeta     | Mynard         | jmynard85@ehow.com     | Female      | 2021-02-03    | United States
     325 | Willard    | Sappell        | wsappell90@usnews.com  | Bigender    | 2021-02-03    | Indonesia
     457 | Lavena     | Dungay         |                        | Genderfluid | 2021-02-02    | Indonesia
     666 | Karlene    | Hamberstone    | khamberstoneih@msn.com | Male        | 2021-02-02    | United States
     703 | Clyve      | Bevans         |                        | Male        | 2021-02-03    | Malta
     714 | Kris       | Dowzell        | kdowzelljt@dedecms.com | Female      | 2021-02-03    | China
     760 | Grata      | De'Vere - Hunt |                        | Female      | 2021-02-03    | China
     782 | Jeniffer   | Slowan         |                        | Genderqueer | 2021-02-01    | China
    (9 rows)

# Like and iLike

It is used to match a text value against a pattern, using wildcards

Find rows where emal ends with '.com'

    test=# SELECT * FROM person WHERE email LIKE '%.com';

or only 'bloomberg.com'

    test=# SELECT * FROM person WHERE email LIKE '%@bloomberg.com';

     id  | first_name | last_name |           email           |   gender   | date_of_birth | country_of_birth
    -----+------------+-----------+---------------------------+------------+---------------+------------------
     341 | Rene       | Aldhouse  | raldhouse9g@bloomberg.com | Bigender   | 2020-05-01    | Russia
     917 | Napoleon   | Scruton   | nscrutonpg@bloomberg.com  | Non-binary | 2020-11-08    | China
    (2 rows)

Another example

    test=# SELECT * FROM person WHERE email LIKE '%@google.%' LIMIT 3;

     id  | first_name | last_name |           email           |   gender    | date_of_birth | country_of_birth
    -----+------------+-----------+---------------------------+-------------+---------------+------------------
      35 | Conrade    | Meriguet  | cmeriguety@google.pl      | Genderqueer | 2020-10-09    | Japan
     150 | Janella    | Saiz      | jsaiz45@google.it         | Female      | 2020-11-15    | Russia
     160 | Leone      | Cattroll  | lcattroll4f@google.com.br | Genderfluid | 2020-04-24    | Philippines

Pattern with number of character

    test=# SELECT * FROM person WHERE email LIKE '________@%' LIMIT 3;

     id | first_name | last_name |         email          |   gender    | date_of_birth | country_of_birth
    ----+------------+-----------+------------------------+-------------+---------------+------------------
      3 | Bancroft   | Varren    | bvarren2@live.com      | Female      | 2020-08-25    | Nigeria
      7 | Noni       | Scaife    | nscaife6@auda.org.au   | Genderfluid | 2020-09-19    | Russia
     21 | Verena     | Kasman    | vkasmank@artisteer.com | Non-binary  | 2020-08-28    | China
    (3 rows)

Another example

    test=# SELECT * FROM person WHERE email LIKE '___h%' LIMIT 3;

     id  |  first_name  | last_name |         email         |   gender   | date_of_birth | country_of_birth
    -----+--------------+-----------+-----------------------+------------+---------------+------------------
      15 | Dorothea     | Johnigan  | djohnigane@com.com    | Agender    | 2020-10-02    | Poland
     268 | Damaris      | De Hoogh  | ddehoogh7f@etsy.com   | Non-binary | 2021-01-30    | China

iLike: ignore the capital in the character

    test=# SELECT * FROM person WHERE country_of_birth LIKE 'P%' LIMIT 3;
     id | first_name | last_name |          email           |   gender    | date_of_birth | country_of_birth
    ----+------------+-----------+--------------------------+-------------+---------------+------------------
      5 | Andrei     | Dilleston | adilleston4@symantec.com | Genderqueer | 2020-10-28    | Philippines
     14 | Cindie     | Themann   | cthemannd@youtube.com    | Genderqueer | 2020-08-25    | Peru
     15 | Dorothea   | Johnigan  | djohnigane@com.com       | Agender     | 2020-10-02    | Poland
    (3 rows)

    test=# SELECT * FROM person WHERE country_of_birth iLIKE 'p%' LIMIT 3;
     id | first_name | last_name |          email           |   gender    | date_of_birth | country_of_birth
    ----+------------+-----------+--------------------------+-------------+---------------+------------------
      5 | Andrei     | Dilleston | adilleston4@symantec.com | Genderqueer | 2020-10-28    | Philippines
     14 | Cindie     | Themann   | cthemannd@youtube.com    | Genderqueer | 2020-08-25    | Peru
     15 | Dorothea   | Johnigan  | djohnigane@com.com       | Agender     | 2020-10-02    | Poland

# Group By

    test=# SELECT country_of_birth, COUNT(*) AS total_appearance FROM person GROUP BY country_of_birth ORDER BY total_appearance DESC;
    	 country_of_birth         | total_appearance
    ----------------------------------+------------------
     China                            |              195
     Indonesia                        |              103
     Philippines                      |               61
     Russia                           |               55
     Brazil                           |               34
     Portugal                         |               29
     Sweden                           |               29
     United States                    |               29
     Poland                           |               28
     France                           |               26
     Ukraine                          |               21
     Peru                             |               17
     Japan                            |               17

# Group By Having

Perform extract filtering after you perfom aggragation. It must comes after Goup by and before Order By

    test=# SELECT country_of_birth, COUNT(*) AS total_appearance FROM person GROUP BY country_of_birth HAVING COUNT(*)>=20 ORDER BY total_appearance DESC;

    country_of_birth | total_appearance
    ------------------+------------------
    China | 195
    Indonesia | 103
    Philippines | 61
    Russia | 55
    Brazil | 34
    Portugal | 29
    United States | 29
    Sweden | 29
    Poland | 28
    France | 26
    Ukraine | 21
    (11 rows)

# Adding new table and using Mackaroo

# install dbeaver using snap

    sudo snap install dbeaver-ce

# References

- [PostgreSQL Documentation](https://www.postgresql.org/docs/current/)
- [Aggregates functions: Max, Min, Count, Sum ...](https://www.postgresql.org/docs/9.1/functions-aggregate.html)
- [PostgreSQL Crash Course](https://www.youtube.com/watch?v=qw--VYLpxG4&t=292s)

- [Mockaroo - Random Data Generator and API Mocking Tool](https://mockaroo.com/)
