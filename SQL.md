In SQL, a clause is a component of a query that allows for filtering or customizing the data being queried. It helps specify conditions or actions to be applied to the data.

**HAVING clause** is used to filter rows in a SELECT statement based on conditions applied to groups defined by the GROUP BY clause. It is similar to the WHERE clause but operates on grouped data rather than individual rows. WHERE clause cannot contain aggregate functions, while the HAVING clause is specifically designed for such purposes.

DELETE command is used to remove specific rows from a table based on conditions specified in the WHERE clause. 

TRUNCATE command is used to delete all data from a table without conditions.

DROP -> delete an existing table, database, index, or view from a database. DROP removes the entire table structure, while TRUNCATE deletes all rows from a table but keeps the structure intact.

DELETE allows the operation to be rolled back (undo), while DROP and TRUNCATE cannot be undone.