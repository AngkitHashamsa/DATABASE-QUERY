# DATABASE-QUERY

TOPICS

1) [TABLE GENERATION](#table-generation) 
2) [DATA MANIPULATIOn](#data-manipulation-method) 
3) [FILTERS](#filters)
4) [Combining DATa](#combine-data)
### TABLE GENERATION

***QUERY***

```sql
CREATE TABLE persons (
	id INT GENERATED ALWAYS AS IDENTITY,
	person_name VARCHAR(50) NOT NULL,
	birth_date DATE,
	phone VARCHAR(15) NOT NULL,
	CONSTRAINT pk_persons PRIMARY KEY (id)
)
```

---

***SCRIPT GENERATED***

```sql
-- Table: public.persons

-- DROP TABLE IF EXISTS public.persons;

CREATE TABLE IF NOT EXISTS public.persons
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    person_name character varying(50) COLLATE pg_catalog."default" NOT NULL,
    birth_date date,
    email character varying(254) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT pk_persons PRIMARY KEY (id),
    CONSTRAINT unique_email UNIQUE (email),
    CONSTRAINT veryf_email CHECK (email::text ~* '^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$'::text)
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.persons
    OWNER to postgres;

```

***ALTER TABLE***

```sql
ALTER TABLE persons
ADD  email VARCHAR(254)  NOT NULL,
ADD CONSTRAINT veryf_email CHECK (email ~* '^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$');

ALTER TABLE persons
ADD CONSTRAINT unique_email UNIQUE (email)

```

```DELETE TABLE```

```sql
DROP TABLE persons
```
------------------

### DATA MANIPULATION METHOD

***INSERT***

```sql
INSERT INTO users (first_name,country,score) 
VALUES ('Newgate', 'West Blue', 800),
	   ('Luffy', 'East Blue', 500);
```

***COPY FROM ONE TABLE TO ANOTHER***

```sql
INSERT INTO persons (id, person_name, birth_date, phone)
SELECT id, first_name, NULL, 'Unkown' FROM users
```

***UPDATE ROW***

```sql
UPDATE users 
	SET first_name = 'Jonshon',
		score = 330
WHERE id = 1;
```

***DELETE ROW***

```sql
DELETE FROM users
WHERE id = 9
```

***DELETE ALL DATA FROM TABLE***

```sql
 TRUNCATE TABLE persons
```

----------------------
### FILTERS

***WHERE Operators***

- Comparison operators
    ```sql
    =
    <
    >
    !=
    >=
    <=
    ```
- Logical Operators
    ```sql
    AND
    OR
    NOT

    -- AND:
    SELECT * FROM users
    WHERE country != 'Canada' AND score > 300;

    -- OR:
    SELECT * FROM users
    WHERE country != 'Canada' OR score > 300;

    -- NOT:
    SELECT * FROM users
    WHERE NOT score < 500
    -- The not operator just give us opposite result
    ```
- Range Operators
    ```sql
    BETWEEN

    -- BETWEEN:
        SELECT * FROM users
        WHERE score >= 200 AND score <= 600
        -- or
        SELECT * FROM users
        WHERE score BETWEEN 200 AND 600
    ```
- Membership Operators
   ```sql
    IN
    NOT IN

    -- IN  -> Check if value exist in list :
        SELECT * FROM users
        WHERE country = 'Canada' OR country = 'India';
        -- OR
        SELECT * FROM users
        WHERE country IN ('Canada','India');

    -- NOT IN  -> Check if value does not exist in list :
        SELECT * FROM users
        WHERE country != 'Canada' AND country != 'India';
        -- OR
        SELECT * FROM users
        WHERE country NOT IN ('Canada','India');
   ```
- Search Operators
   ```sql
    LIKE

    -- Pattern -> 
    -- 1) % -> Any result before or after or between
    -- 2) _ -> Exact result

    -- %
    -- Last two character must end with ia
    SELECT * FROM users
    WHERE first_name LIKE '%ia'

    -- or name starts with E
     SELECT * FROM users
    WHERE first_name LIKE 'E%'
     -- or name has r in middle
     SELECT * FROM users
    WHERE first_name LIKE '%r%'


    --- _ in this case the two __ expects two words before h and any word after h 
    -- ex : Rahul, John, 
     SELECT * FROM users
    WHERE first_name LIKE '__h%'
   ```
   --------------

    ### Combine Data 

    - JOINS
        - Inner JOIN
        - Full JOIN
        - LEFT JOIN
        - Right JOIN
        ![JOINS](./assets/JOINS.png)
    - SET OPERATORS
       - UNION
       - UNION ALL
       - EXCEPT(MINUS)
       - INTERSECT 
        ![SET](./assets/SET.png)

    #### Inner JOIN
    ```sql
    SELECT 
    users.id, 
    users.first_name, 
    orders.order_id, 
    orders.sales 
    FROM users 
    INNER JOIN orders ON users.id = orders.customer_id

    --- or since the table might be long we can shorter with using alias

    SELECT 
    u.id, 
    u.first_name, 
    o.order_id, 
    o.sales 
    FROM users AS u
    INNER JOIN orders AS o ON u.id = o.customer_id 
    ```
    ![inner](./assets/inner_join.png)

    #### LEFT JOIN
    -- GET all customers with orders including without orders

    ```sql
    SELECT 
    u.id,
    u.first_name,
    o.order_id,
    o.sales
    FROM users  AS u
    LEFT JOIN orders AS o ON u.id = o.customer_id
    ```
    ![Left](./assets/left_join.png)

    #### RIGHT JOIN

    ```sql
    SELECT 
    u.id,
    u.first_name,
    o.order_id,
    o.sales
    FROM users AS u
    RIGHT JOIN orders AS o 
    ON u.id = o.customer_id
    ```
    ![right_join](./assets/right_join.png)

    ### FULL JOIN

    ```sql
    SELECT 
    u.id,
    u.first_name,
    o.order_id,
    o.sales
    FROM users AS u
    FULL JOIN orders AS o 
    ON u.id = o.customer_id
    ```   
     ![full_join](./assets/full_join.png)


    ### LEFT ANTI JOIN

    -- Returns ROW from left that has No match in right
    -- ex: Get any customer who have not place any orders

    ```sql
    SELECT 
    u.id,
    u.first_name,
    o.order_id,
    o.sales
    FROM users AS u
    FULL JOIN orders AS o 
    ON u.id = o.customer_id
    WHERE o.customer_id IS NULL
    ```
    ![left_anti_join](./assets/left_anti_join.png)