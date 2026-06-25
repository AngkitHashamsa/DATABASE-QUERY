[Home](../README.md)

![tables](../assets/E%20STORE_2026-06-24T11_53_26.540Z.png)
CREATED IN DRAW DB
[draw db](https://www.drawdb.app/editor/diagrams/6e2a7fe6-e7d0-4924-91da-edee58ab5a55)
```sql
CREATE TABLE IF NOT EXISTS "Product" (
	"product_id" BIGSERIAL NOT NULL,
	"Product" TEXT NOT NULL,
	"Category" TEXT NOT NULL,
	"Price" TEXT NOT NULL,
	PRIMARY KEY("product_id")
);




CREATE TABLE IF NOT EXISTS "Customers" (
	"customer_id" BIGSERIAL NOT NULL,
	"first_name" TEXT NOT NULL,
	"last_name" TEXT,
	"country" TEXT,
	"score" INTEGER,
	PRIMARY KEY("customer_id")
);




CREATE TABLE IF NOT EXISTS "Employees" (
	"employee_id" BIGSERIAL NOT NULL,
	"first_name" TEXT,
	"last_name" TEXT,
	"department" TEXT,
	"brithdate" DATE,
	"gender" TEXT,
	"salary" INTEGER,
	"manager_id" INTEGER,
	PRIMARY KEY("employee_id")
);




CREATE TABLE IF NOT EXISTS "Orders" (
	"order_id" BIGSERIAL NOT NULL,
	"customer_id" INTEGER NOT NULL,
	"product_id" INTEGER NOT NULL,
	"salesperson_id" INTEGER NOT NULL,
	"order_date" DATE NOT NULL,
	"shipt_date" DATE NOT NULL,
	"order_status" TEXT NOT NULL,
	"shipAdress" TEXT NOT NULL,
	"bill_address" TEXT NOT NULL,
	"quantity" INTEGER NOT NULL,
	"sales" INTEGER,
	"created_at" ,
	PRIMARY KEY("order_id")
);



ALTER TABLE "Customers"
ADD FOREIGN KEY("customer_id") REFERENCES "Orders"("customer_id")
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE "Product"
ADD FOREIGN KEY("product_id") REFERENCES "Orders"("product_id")
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE "Employees"
ADD FOREIGN KEY("employee_id") REFERENCES "Orders"("salesperson_id")
ON UPDATE NO ACTION ON DELETE NO ACTION;
```