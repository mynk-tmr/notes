

**data model** - a set of tools for describing data, semantics, and constraints. It helps in defining the relationship between data entities and their attributes. Some common data models include *hierarchical, network, ER models, and relational models.* 

*storage engine* - part of a database used to manage how data is stored on disk.

4 basic NoSQL types:
1. Key value store 
2. Document store
3. Column store
4. Graph base

*Why file based storage is bad ?*
- lack indexing, leading to time-consuming content retrieval. 
- Redundancy and inconsistency (when updates)
- Disorganized data
- No concurrency management, which allows simultaneous operations

*Why use DBMS / Pros of DBMS*
- **Data sharing:** Multiple users can access data from the same database simultaneously.
- **Integrity restrictions:** maintain data integrity and ensure that only valid and consistent data is stored
- **redundancy control:** storing data in a centralized database
- **Data independence:** data structure can be changed without impacting running applications.
- automatic **Backup and recovery** features
- **Data security:** through authentication and encryption techniques.

*Languages used by DBMS*
- **Data Definition Language (DDL):** commands for defining databases, such as CREATE, ALTER, DROP, TRUNCATE, and RENAME.
- **Data Manipulation Language (DML):** manipulation in a database using commands like SELECT, UPDATE, INSERT, DELETE, etc.
- **Data Control Language (DCL):** commands manage user permissions and controls within the database system, including GRANT and REVOKE.
- **Transaction Control Language (TCL):** commands to manage database transactions, such as COMMIT, ROLLBACK, and SAVEPOINT.

**ACID properties** in DBMS are fundamental principles that ensure data integrity. They are 
- *Atomicity* - transaction is treated as a single unit of execution, either completing entirely or not at all.
- *Consistency* - database integrity remains before and after each transaction.
- *Isolation* - multiple transactions occur concurrently without interfering
- *Durability* - Once a transaction is committed, its changes are permanently saved in non-volatile memory.

**Q7: Are NULL values in a database equivalent to zero or blank space?**

A NULL value represents an unknown, missing, or inapplicable value, whereas zero represents a numeric value and blank space represents a character.

**Q10:  Explain RDBMS with examples.**

RDBMS was developed to facilitate easier data access and storage. In RDBMS, data is stored in tables consisting of rows and columns. It enables efficient retrieval of specific values from the database. Examples - MySQL, Oracle DB.

**Q11: How would you define a checkpoint in DBMS?**

A checkpoint in DBMS is a technique that permanently stores all previous logs onto the storage drive. It ensures the recovery and maintenance of the ACID properties by preserving transaction logs and maintaining shadow pages. Checkpoints serve as minimal recovery points from which the database engine can recover after a crash, using the transaction log records to restore all committed data up to the crash moment.


**Q14: When does a checkpoint occur in DBMS?**

A checkpoint occurs as a snapshot of the current state of DBMS. It limits the amount of work needed during system restart after a subsequent crash. Checkpoints are used for database recovery following a system crash. They are particularly employed in log-based recovery solutions to restart the system without having to redo all transactions from the beginning.

**Q16: What types of interactions can be handled by a DBMS?**

A DBMS can handle various interactions, including data definition (creating and modifying the structure of databases and tables), update (adding, modifying, or deleting data), retrieval (retrieving specific data from databases), and administration (managing user permissions, security, backups, etc.).

**Q17: Explain query optimization in DBMS.**

Query optimization is the process of identifying the most efficient plan for evaluating a query, minimizing the estimated cost. It involves selecting the best algorithms and approaches. Query optimization aims to deliver query results faster, handle a larger number of queries in less time, and reduce the complexity of time and space requirements.

**Q19: Define aggregation

Aggregation is a feature in the Entity-Relationship (E-R) model that allows one relationship set to interact with another relationship set.

**Q20: What are the different levels of abstraction in DBMS?**

- Physical -> defines how data is stored in database system.
- Logical -> what data is stored and how data pieces relate to each other.
- View -> describes a specific portion of database, focusing on a user's perspective or requirements.

**Q21: How would you define an entity-relationship model?**

An entity-relationship model is a graphical approach to database design where real-world objects are represented as entities, and relationships between them are depicted. It provides a visual representation that allows the database administrators (DBAs) to quickly understand the schema.

**Q22: Explain the terms Entity, Entity Type, and Entity Set in DBMS.**

- Entity: entity refers to a real-world object with attributes that represent its specific characteristics. 
- Entity Type: a collection of entities that share same attributes. It represents one or more related tables in a database. 
- Entity Set: a group of related entity types, such as a set of employees, companies, or persons.

Entity type extension involves combining similar entity types into a single type and grouping them together as an entity set.

**Q23: What is meant by a transparent DBMS?**

A transparent DBMS hides its physical structure from users. This abstraction allows users to focus on the logical representation of data only

**Q24: What are the unary operations in Relational Algebra?**

unary operations are operations that work on a single operand. e.g PROJECTION and SELECTION. These operations manipulate and retrieve data from a single relation. RENAME is used to rename attributes or relations 

**Q27: Explain Relation Schema and Relation.**

A Relation/table Schema defines the properties and structure of a relation or table. It specifies the name of the table and provides a blueprint for organizing data into tables. The relation schema does not contain any actual data.

A Relation represents a table in a relational database. It is a collection of tuples (rows) where each tuple consists of an ordered list of values. The attributes in the relation are the connected columns, and key attributes uniquely identify the tuples. 

**Q28: What is the Degree of relation?**

 Number of attributes (columns) in the relation. The degree, also known as Cardinality, describes how many times one entity occurs in relation to another entity. There are three degrees of relation: one-to-one (1:1), one-to-many (1:M), and many-to-one (M:1).

**Q29: Define Relationship in a database context.**

A relationship in a database refers to an association between two or more entities. There are three types of relationships
- One-to-One: A single record of one entity can be linked to a single record of another entity.
- One-to-Many (Many-to-One): A single record of one entity can be linked to multiple records of another entity, and vice versa.
- Many-to-Many: Multiple records of one entity can be linked to multiple records of another entity.

**Q84: Define specialization and generalization in the context of database design.**

Specialization: process of creating subclasses for a specific entity type. Each subclass inherits all attributes and relationships of parent entity and may have extra unique attributes and relationships.

Generalization: process of identifying common attributes and relationships among a group of entities and defining a common superclass for them.


**Q31: Define Data Abstraction in DBMS.**

Data abstraction in DBMS refers to the process of hiding unnecessary details from users, allowing for simplified user interaction with the complex data structures of a database system.

**Q34: Define a transaction

A transaction in a database refers to a collection of database operations that must be treated as a single unit

**Q35: What is a Join?**

Ans. In SQL, a join is a technique used to combine data from two or more tables based on a common field. It allows for the retrieval of related data from multiple tables into a single result set.

**Q36: What does Identity represent in a database context?**

Identity (or AutoNumber) is a column in a database that automatically generates numeric values. Identity and GUID columns are often used as primary keys. Indexing these columns is typically unnecessary.

**Q37: Define a view in SQL.**

In SQL, a view is a virtual table created from the result set of a SQL statement. It allows users to query the view as if it were a regular table

Views have several uses in a database:
1. represent a subset of data from one or more tables
2. combine and simplify multiple tables 
3. aggregated tables, where the database engine performs aggregate functions (e.g., sum, average) and displays the results alongside the data.
4. *abstraction* of underlying tables
5. occupy minimal storage space since they only store the definition of the view
6. provide additional security by restricting access to specific columns or rows of the underlying tables.

**Q94: What is the difference between a view and a table?**

A table is a storage structure that holds data in a relational database, while a view is a virtual table derived from one or more tables or views. Unlike tables, views do not store data physically but provide a way to query and manipulate data from multiple tables as a single entity.

**Q39: What is a Trigger?**

A trigger is a piece of code associated with write operations on a table. It is automatically executed when the associated query is run. Triggers are used to maintain database integrity by enforcing specific actions or constraints whenever data changes occur.

**Q40: What is a stored procedure?**

A stored procedure consists of a set of pre-defined procedures that are commonly used in applications to perform database-related activities. Stored procedures can be executed with specific parameters and provide a way to encapsulate and reuse code logic within the database.

**Q41: How does a Trigger differ from a Stored Procedure?**

Triggers cannot be directly called, unlike stored procedures. They are only associated with specific queries or actions.

**Q42: Explain the concept of database normalization.**

Database normalization is a process that evaluates relation schemas based on their functional dependencies and primary keys to achieve desirable properties such as minimizing redundancy and inconsistencies in write operations. If a relation schema does not meet these properties, it is decomposed into smaller relation schemas that do meet the requirements.

**Q43: What are indexes in a database?**

Indexes in a database are data structures designed to improve speed of data retrieval operations on tables. They trade off increased writes and storage space for faster access to data. Indexes allow for quicker searches by organizing and storing data in a specific order.

**Q44: Differentiate between clustered and non-clustered indexes.**

Clustered indexes determine the physical order in which data is stored on a disk. Each database table can have only one clustered index. Non-clustered indexes establish logical ordering of data rather than physical ordering. They often involve the creation of tree structures, such as B-trees or B+ trees, where leaves point to disk records.

**Q45: What is denormalization in a database?**

Denormalization is a technique used for database optimization, where duplicate data is intentionally introduced into one or more tables. This approach aims to improve performance by reducing the need for complex joins and increasing data retrieval speed.

**Q46: What is a clause in SQL?**

In SQL, a clause is a component of a query that allows for filtering or customizing the data being queried. It helps specify conditions or actions to be applied to the data.

**Q47: Define Livelock.**

Livelock occurs when two or more processes repeatedly engage in a futile interaction in response to changes in other processes. In this situation, the processes are continuously executing without making any progress. Unlike a deadlock, where processes are waiting, livelock involves active execution without achieving a desirable outcome.

**Q48: What is QBE in the context of databases?**

Query-by-example (QBE) is a visual/graphical technique used to retrieve information from a database by using skeleton tables as query templates. QBE allows users to express what they want to achieve by entering example values into the template, without the need for programming languages. It simplifies the process of accessing desired information by employing a two-dimensional syntax that resembles tables.

**Q49: Why are cursors necessary in embedded SQL?**

Cursors are used in embedded SQL to store the result of a query, allowing application programs to process the data row by row. SQL statements operate on sets of data and return another set of data, while host language programs typically work with data one row at a time. Cursors enable traversal through a set of rows generated by a [](https://www.almabetter.com/bytes/tutorials/sql/select-statement-in-sql)SQL SELECT statement within the code, acting as a pointer for efficient row-by-row processing.

**Q50: What is the purpose of normalization in DBMS?**

- structuring the attributes of a database to minimize or eliminate data redundancy
- helps break down large database tables into smaller ones and establish relationships between them

**Q51: How does a database schema differ from a database state?**

A database state refers to actual collection of data stored in a database at a specific moment, while a database schema refers to overall design and structure of database.

**Q52: What is the purpose of SQL?**

Ans. SQL serves as a language used to interact with relational databases. Its primary purpose is to perform operations such as querying, updating, and modifying data in the database.

**Q55: What is the concept of a subquery in SQL?**

Ans. A subquery/ inner query is nested within another query. It is used to retrieve data based on result of another query.

**Q58: Define Correlated Subquery in DBMS.**

Ans. A correlated subquery is executed for each row of outer query. It is a nested query where the inner query depends on the values from the outer query.

**Q57: What is the main difference between UNION and UNION ALL?**

Ans. UNION and UNION ALL are used to combine data from multiple tables, but they differ in terms of duplicate row handling. UNION removes duplicate rows and selects only distinct rows after merging the data, while UNION ALL does not

**Q59: What integrity rules exist in a DBMS?**

Ans. In a DBMS, two major integrity rules are
- Entity Integrity: value of a primary key cannot be NULL.
- Referential Integrity: foreign key value is either NULL or matches the primary key of another relation.

**Q60: What is the E-R model in DBMS?**

E-R (Entity-Relationship) model is a conceptual model used in relational databases. It represents entities, their attributes, and the relationships between them, providing a visual representation of the database structure and data dependencies.

**Q61: What is the significance of a functional dependency in a DBMS?**

A functional dependency in a DBMS is a constraint that describes the relationship between different attributes within a relation. 

**Q64: Explain the distinction between intension and extension in a database.**

In a database, intension refers to the database schema or design, which remains relatively unchanged. Extension represents the actual data stored in the database at a specific point in time, fluctuating.

**Q66: Explain lock**

A lock in a database prevents multiple users or sessions from updating the same data simultaneously. A shared lock allows multiple transactions to read data concurrently, while an exclusive lock ensures only one transaction can perform write operations to maintain data consistency.

**Q67: Describe the different normalization forms in a DBMS.**

1. 1NF: Ensures atomicity of data and eliminates duplicate columns.
2. 2NF: Builds upon 1NF by ensuring non-key attributes are fully functionally dependent on primary key.
3. 3NF: Extends 2NF by eliminating transitive dependencies between non-key attributes.
4. BCNF: Further refines 3NF by ensuring that no non-prime attribute is functionally dependent on another non-prime attribute.

**Q68: Explain the various types of keys in a database.**

- Candidate Key: minimal super key
- Super Key: set of attributes that uniquely identifies each tuple
- Primary Key: chosen candidate key that uniquely identifies each tuple. Only 1 for a table
- Unique Key: Similar to a primary key but allows NULL values. Can be many in a table
- Alternate Key: Candidate keys that are not selected as the primary key.
- Foreign Key: Connects attributes between tables, referencing a primary key from another table.
- Composite Key: A primary key composed of multiple attributes


**Q69: Differentiate between a 2-tier and 3-tier architecture in a DBMS.**

A 2-tier architecture involves direct interaction between client applications and the database server. In contrast, a 3-tier architecture adds an intermediary layer, providing a GUI and enhanced security

**Q70: Explain the distinction between logical database design and physical database design and how this separation leads to data independence.**

Logical database design involves transforming the conceptual schema into a relational database structure, while physical database design focuses on storage, indexing, and optimization. This separation leads to data independence because the logical design is independent of the physical design

**Q71: What are temporary tables and when are they beneficial?**

Temporary tables are tables that are used for a single session or for the duration of a transaction. They are commonly used to support unique rollups or specific application processing needs. Unlike permanent tables, temporary tables do not have pre-allocated space and dynamically allocate space as rows are added.

**Q73: What is conceptual design in DBMS?**

Conceptual design is the initial stage of the database design process. It aims to create a database independent of database software and physical implementation details. During conceptual design, a conceptual data model is developed, describing the primary data items, properties, relationships, and constraints within a specific domain.

**Q76: Define database partitioning and its significance.**

Database partitioning is the process of dividing a logical database into independent components for horizontal scaling and faster access to specific portions of databases.

**Q77: Explain the functionality of a DML compiler.**

A DML compiler is responsible for converting DML statements into a query language that can be understood by the query evaluation engine. 

**Q78: What is Relational Algebra?**

*Relational Algebra* is a procedural query language that takes 1 or 2 relations as input to produce a new relation as output. It represents basic set of operations in the relational model. eg union, intersection, etc

*Relational Calculus* is a non-procedural query language that utilizes mathematical predicate calculus instead of algebraic operations. 

**Q82: How do you interact with an RDBMS?**

SQL queries are used to provide input to the database, and the database processes these queries to generate the desired output.

**Q83. Explain the concepts of Proactive, Retroactive, and Simultaneous Update.**

Proactive: Changes made to the database before it is operational in the real world.
Retroactive: Updates applied to a database after it has been in use.
Simultaneous: Updates applied to the database at the same time they become effective in the real world.

**Q85: What is the concept of Fill Factor in relation to indexes?**

Fill Factor refers to the percentage of space left on each leaf-level page of an index that is densely packed with data. The default value is typically 100, indicating full packing of index data.

**Q86: What is Index Tuning and how does it enhance query performance?**

Index Tuning involves optimizing a set of indexes to improve query performance and query processing time. It helps in query performance enhancement through:
- Suggesting optimal queries using query optimizers.
- Measuring the impact with indices, query distribution, and performance metrics.
- Optimizing databases for a small number of frequently executed queries.

**Q87: Explain what a deadlock is and how it can be resolved.**

A deadlock occurs when two transactions are waiting for resources that are mutually unavailable or when one transaction is waiting for another operation to complete. Deadlocks can be resolved by ensuring that all transactions acquire all required locks at the same time. If a deadlock is detected, one of the transactions needs to be aborted to break the deadlock and remove the incomplete work.


**HAVING clause** is used to filter rows in a SELECT statement based on conditions applied to groups defined by the GROUP BY clause. It is similar to the WHERE clause but operates on grouped data rather than individual rows. WHERE clause cannot contain aggregate functions, while the HAVING clause is specifically designed for such purposes.

DELETE command is used to remove specific rows from a table based on conditions specified in the WHERE clause. 
TRUNCATE command is used to delete all data from a table without conditions.

**Q62: How is pattern matching performed in SQL?**

Pattern matching in SQL is achieved using the LIKE operator. The 'percent' symbol (%) is used to match zero or more characters, while the underscore symbol is used to match a single character.

**Q56: How is the DROP command used, and what are the differences between DROP, TRUNCATE, and DELETE commands?**

DROP -> delete an existing table, database, index, or view from a database. DROP removes the entire table structure, while TRUNCATE deletes all rows from a table but keeps the structure intact.

DELETE allows the operation to be rolled back (undo), while DROP and TRUNCATE cannot be undone.

**Referential integrity** is a rule that ensures the consistency and integrity of data between related tables. It requires that foreign key values in one table match primary key values in another table, preventing the creation of orphaned or invalid records. A foreign key constraint is a rule that ensures the referential integrity between two tables. 

*OLTP (Online Transaction Processing) databases*
- optimized for transactional processing, supporting day-to-day operations 
- focus on fast and concurrent data modifications.

*OLAP (Online Analytical Processing) databases*
- designed for analytical processing and reporting
- providing complex querying capabilities and aggregations

![[Pasted image 20240816204511.png]]