# Database

### 1. Three Normal Forms of Databases

- Each column of a table is an indivisible atomic data item. This ensures that each column is easy to maintain, query efficiently, and easy to calculate.
- Every non-key attribute field is dependent on the primary key. This ensures that each row of data is independent and divided by the primary key.
- Any non-primary attribute field is not dependent on any other non-primary attribute field. This reduces the number of table fields and data storage. Dependent non-primary key fields can be placed in a separate table as a single relationship table.

The three normal forms are basic principles for designing databases. They help us design table structures that have minimal redundancy, high storage efficiency, and query performance. However, we should not blindly pursue database design norms. Database design should focus on requirements and performance, in order of priority: requirements -> performance -> table structure. For example, sometimes adding a redundant field can significantly improve query performance.

### 2. Five Constraints

- Primary key constraint: unique and non-null
- Unique constraint: unique and nullable, but only one is allowed
- Default constraint: when there are no other specified values, the default value will be written to the field
- Foreign key constraint
- Not null constraint

### 3. Transaction Isolation Levels

- Read uncommitted: other transactions can see the data that has not been committed by this transaction. There are issues with dirty reads, phantom reads, and non-repeatable reads.
- Read committed: data can only be read by another transaction after it has been committed. This avoids dirty reads. This is the most common database isolation level.
- Repeatable read: in addition to ensuring that a transaction cannot read uncommitted data from another transaction, it also avoids non-repeatable reads. However, it cannot prevent phantom reads, such as reading newly added rows for the second time. This is the default isolation level for MySQL, and phantom reads are resolved using mvvc version concurrency control.
- Serializable: transactions are executed in sequence. This is the highest isolation level.

The problems that occur under each isolation level are compared as follows:

| Isolation Level | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
| --- | --- | --- | --- |
| Read Uncommitted | √ | √ | √ |
| Read Committed | × | √ | √ |
| Repeatable Read | × | × | √ |
| Serializable | × | × | × |

### 4. ACID Principles and Four Characteristics of Transactions

- Atomicity: A transaction must either commit all changes successfully or roll back all changes. It cannot execute only a part of the operation (it only cares about success or failure, not whether it is correct or not).
- Consistency: The consistency of a transaction means that the database must be in a consistent state before and after the transaction is executed. This ensures that any transaction will cause the database to transition from one legal state to another (database constraints are enforced, and the code itself ensures that it is logically correct, such as not exceeding the balance in a transfer).
- Isolation: In a concurrent environment, each transaction has its own complete data space, and modifications are isolated. Data must be either in its original state or its modified state; it cannot be in an intermediate state.
- Durability: After a transaction is successful, the operation on the database must be permanently saved and must take effect. Even if the system crashes, it should be able to recover to the state after the transaction was successful.

### 5. Pessimistic and Optimistic Locking in Databases

Pessimistic Locking: Assume that every time data is retrieved, it is assumed that it will be modified. For example, lock rows or tables in a database, as shown in the following for update example. Also, note that for update should be used on an index, otherwise the entire table will be locked.

```
START TRANSACTION; # Start a transaction
SELECT * FROM table_name WHERE id=1 FOR UPDATE;
UPDATE table_name SET name = 'nathan' WHERE id = 1;
COMMIT; # Commit the transaction

```

Optimistic Locking: Assume that every time data is retrieved, it is optimistic that no one else will modify it. When updating, if the version changes, the update will fail.

```
update status set name = 'nathan', version = (version + 1) where id = 1 and version = 1;

```

### 6. Difference between B+ Tree Indexes and Hash Indexes

- B+ Tree is a balanced multi-way tree that searches from the root node to the leaf node.
- Hash Index uses a hash algorithm to convert the key value into a new hash value, and then immediately locates the corresponding position with a single hash algorithm.
- If it is an equal value query, the hash index has an absolute advantage.
- If it is a range query search, the hash index is useless. The hash index cannot complete sorting through indexes, nor can it support the most left matching rule for multi-column combined indexes. In the case of a large number of duplicate key values, the efficiency of the hash index is also very low. Therefore, B+ Tree index is used in most scenarios.

### 7. Difference between B Tree and B+ Tree

This video explains it well: [https://www.bilibili.com/video/BV1Aa4y1j7a4](https://www.bilibili.com/video/BV1Aa4y1j7a4)

### 7.1. B Tree

B-tree is also known as B-Tree.

- All key values are distributed throughout the tree (index values and specific data are all in each node).
- Any one keyword appears only in one node.
- The search may end at a non-leaf node (in the best case, O(1) can find data and end without searching to the leaf node).
- Search within the entire key set is close to binary search.

### 7.2. B+ Tree

- All key values are stored in leaf nodes, and the internal nodes do not store real data.
- Add a chain pointer to all leaf nodes.

### 8. Clustered Index and Non-clustered Index

Reference: [[https://www.cnblogs.com/jiawen010/p/11805241.html](https://www.cnblogs.com/jiawen010/p/11805241.html)] ([https://web.archive.org/web/20220319154240/https://www.cnblogs.com/jiawen010/p/11805241.html](https://web.archive.org/web/20220319154240/https://www.cnblogs.com/jiawen010/p/11805241.html))

### 8.1. Clustered Index

> The data storage and index are combined into one. The data is actually stored in the leaf node of the index, and a table can only have one clustered index, usually the primary key.
> 

The clustered index is a B+ Tree constructed according to the primary key of each table. The data records of the entire table are stored in the leaf nodes of the clustered index, and the leaf nodes of the clustered index are also called data pages. This feature determines that the data organized in the index is also part of the index. Each table can only have one clustered index.

InnoDB primary key indexes and row records are stored together. If there is no primary key index, a unique index will be used. If there is no unique index, the id of a row in the database will be used as the primary key index.

Advantages:

- Faster data access. Because the clustered index stores data and indexes in the same B+ Tree, it is faster to get data from the clustered index than from a non-clustered index.
- Clustered indexes are very fast for sorting and querying primary keys.

Disadvantages:

- Insertion speed depends heavily on the insertion order. Inserting data in the order of the primary key is the fastest way to do it. Otherwise, page splitting will occur, which seriously affects performance. Therefore, for InnoDB tables, we generally define an auto-incremented ID column as the primary key.
- Updating the primary key is very expensive because it causes the updated row to move. Therefore, for InnoDB tables, we generally define primary keys that cannot be updated.
- Querying a secondary index requires two index lookups. First, find the primary key value, and then find the row data based on the primary key value.

### 8.2. Non-clustered Index

> The data storage and index are separated, and the leaf node of the index structure points to the corresponding row of data.
> 

In addition to the clustered index, non-clustered indexes of Innodb tables are called secondary indexes. Accessing data through a secondary index always requires a two-stage search. The leaf node of the secondary index does not store the physical location of the row, but stores the primary key value.

Advantages:

- Multiple non-clustered indexes can be created for a table.
- Updating non-clustered indexes is faster than updating clustered indexes because updating non-clustered indexes does not cause data to move.
- Querying a secondary index does not require scanning the entire table.

Disadvantages:

- Secondary index access requires two index searches, which is slower than clustered index access.
- The leaf nodes of the secondary index do not store the row data, but instead store the primary key value.