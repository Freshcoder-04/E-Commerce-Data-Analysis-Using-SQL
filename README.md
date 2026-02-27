# E-Commerce-Data-Analysis-Using-SQL

**Short description —** A small, self-contained SQL project that demonstrates how to build a relational schema for an e-commerce dataset and run analytic queries (sales, best-selling products, buyer profiling, etc.). The repository contains table DDL, example queries, an ER diagram, and the dataset used for exploration. ([GitHub][1])

---

## What’s in this repository

* `databases_query.sql` — create database and switch to it. ([GitHub][2])
* `users_table_query.sql`, `products_table_query.sql`, `orders_table_query.sql`, `order_details_table_query.sql` — DDL for the four main tables (`users`, `products`, `orders`, `order_details`). These files show column names, keys and foreign key constraints. ([GitHub][3])
* `main.sql` — collection of exploratory queries and analytics (data checks, monthly transaction summaries, top products, buyer profiling, dropshipper/reseller detection, etc.). ([GitHub][4])
* `ER diagram.png` — visual schema (tables and relationships). 
* `dataset.zip` / `file_CSV_updated.rar` — the dataset used to populate the tables (included in repo).

---

## ER diagram

![ER diagram](https://raw.githubusercontent.com/Freshcoder-04/E-Commerce-Data-Analysis-Using-SQL/main/ER%20diagram.png)
(Shows `users`, `products`, `orders`, `order_details` and their relationships.) 

---

## Database schema (summary)

**users**

* `userID (int, PK)`
* `nama_user (varchar)`
* `kodepos (int)`
* `email (varchar)`

**products**

* `productID (int, PK)`
* `desc_product (varchar)`
* `category (varchar)`
* `base_price (int)`

**orders**

* `orderID (int, PK)`
* `seller_id (int, FK → users.userID)`
* `buyer_id (int, FK → users.userID)`
* `kodepos, subtotal, discount, total`
* `created_at, paid_at, delivery_at` (dates)

**order_details**

* `order_detailID (int, PK)`
* `order_id (int, FK → orders.orderID)`
* `product_id (int, FK → products.productID)`
* `price, quantity`
  (DDLs are provided in the `*_table_query.sql` files.) ([GitHub][3])

---

## Quick start — reproduce locally

1. Clone the repo:

```bash
git clone https://github.com/Freshcoder-04/E-Commerce-Data-Analysis-Using-SQL.git
cd E-Commerce-Data-Analysis-Using-SQL
```

2. Requirements: MySQL (or MariaDB) installed and running. A MySQL client (CLI, Workbench, DBeaver, etc.).

3. Create the database and tables, then load your data:

* If the dataset is CSVs, import them into the tables using `LOAD DATA INFILE` (or via your client).
* If you prefer to run step-by-step SQL:

```sql
-- from mysql client
SOURCE path/to/databases_query.sql;
SOURCE path/to/users_table_query.sql;
SOURCE path/to/products_table_query.sql;
SOURCE path/to/orders_table_query.sql;
SOURCE path/to/order_details_table_query.sql;
```

(Adjust `SOURCE` paths to the files in the cloned repo.)

4. Populate tables using the CSVs or the provided `dataset.zip` (unzip and import each CSV into the corresponding table). If the repo provides SQL inserts, run them with `SOURCE`.

5. Run the exploratory queries:

```sql
SOURCE path/to/main.sql;
```

`main.sql` contains many example queries and checks to get you started. ([GitHub][4])

---

## Key queries & intent (selected from `main.sql`)

Below are some of the important queries in `main.sql` and what they do — useful to mention on the README so other users understand the analysis.

* **Sanity checks**

  * Row counts and NULL checks for each table (ensures data quality). ([GitHub][4])

* **Order status classification**

  * Creates a derived `status` column using `created_at`, `paid_at`, `delivery_at` to classify orders as `Completed`, `Unpaid`, `Paid & Undelivered`, etc. (useful for lifecycle analysis). ([GitHub][4])

* **Top buyers / high-value transactions**

  * `GROUP BY buyer` with `SUM(total)` to find highest spending users. ([GitHub][4])

* **Best-selling products / categories**

  * Joins `order_details` → `products` and filters by date ranges (e.g., December 2019) to find top selling products and categories. Good for seasonal analysis. ([GitHub][4])

* **Monthly transaction volume & value**

  * Aggregation by `YEAR(created_at), MONTH(created_at)` giving number of transactions and total transaction value per month. Useful for time series analysis. ([GitHub][4])

* **Buyer behavior / profiling**

  * Queries to find likely dropshippers (many transactions, many distinct postal codes), resellers (large average quantities), and users who are both buyers & sellers. These leverage simple heuristics based on `orders` and `order_details`. ([GitHub][4])

---

## Suggested improvements / next steps

If you want to extend or harden this project, here are suggestions:

* Add a `README` summary of the dataset columns (source, meaning, ranges, null handling).
* Convert exploratory SQL into parameterized scripts (stored procedures or `.sql` templates) so non-SQL users can run analyses by changing date ranges or thresholds.
* Add a `data-loading` script that automates CSV → table import and applies necessary cleanup (NULL conversion, type casting, trimming).
* Add unit tests for data quality (counts, uniqueness of PKs, referential integrity checks).
* Create simple visualizations (Jupyter + pandas or a BI tool) that consume the SQL outputs (monthly revenue charts, top product dashboards).
* If privacy matters, redact or anonymize user emails/postcodes in the dataset before sharing.

---

## Reproducibility & Data

The repository includes `dataset.zip` / `file_CSV_updated.rar` — use those to load the tables locally and reproduce the `main.sql` results. If you publish or share results, note any data transformations you performed (NULL handling, `NA` → `NULL`, date normalizations). ([GitHub][1])

---

## License & contact

* No license file is present in the repo. If you want others to reuse the work, add a `LICENSE` (e.g., MIT, Apache-2.0) and mention any dataset usage restrictions. ([GitHub][1])

---

## Quick references (files)

* `main.sql` — exploratory and analytic queries. ([GitHub][4])
* `*_table_query.sql` — table DDL for `users`, `products`, `orders`, `order_details`. ([GitHub][3])
* `ER diagram.png` — visual schema. 

---