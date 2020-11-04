# Useful queries
1. `Select column_name from INFORMATION_SCHEMA.COLUMNS where TABLE_NAME = N'table_name';` will let you know all the column names in the `table_name`.
1. `show DATABASES` will list all the databases you can `use`, which is handy if you've forgotten things.
1. `show tables` is the same for tables.
1. To see a list of column names, use this: `SELECT column_name FROM information_schema.columns WHERE table_schema = 'YOUR_DATABASE_NAME' AND table_name = 'YOUR_TABLE_NAME';`
1. See a list of data types by selecting the `data_type` column from the above select.
1. When constructing foreign keys, the types need to match. This includes signed vs. unsigned.
1. You can use `SHOW ENGINE INNODB STATUS;` to see causes for specific error messages, seeing what went wrong in the DB.
