# DATABASE-QUERY

TOPICS

1) [TABLE GENERATION](#table-generation) 
2) [DATA MANIPULATIOn](#data-manipulation-method) 
3) [FILTERS](#filters)

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
    ```
- Range Operators
    ```sql
    BETWEEN
    ```
- Membership Operators
   ```sql
    IN
    NOT IN
   ```
- Search Operators
   ```sql
    LIKE
   ```