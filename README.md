
## DDL - Definition Data Language
- Create
- Alter
- Truncate
- Comment
- Rename
- Drop

## DML - Definition Manipulation Language
- Insert
- Select
- Delete
- Update

## DCL - Data Control Language
- Grant - Brinda privilegios
- Revoke - Quita privilegios

## TCL - Transaction Control Language
- Commit - Guarda el trabajo hecho
- Rollback - Deshace la modificación del último commit
- Set -> Establece aislamiento de transacción
- Savepoint - Salva un punto al que se puede retroceder en la transacción

# SQL Commands

### JOIN
- Inner join
- Left join
- Right join
- Full join
- Self join

### AGGREGATION
- Sum
- Avg
- Count
- Min
- Max

### GROUP BY
- Group by
- Having

### ALIAS
- As

### ORDER BY
- Order by asc
- Order by desc

### WHERE
- Exist
- Like
- In
- Not
- Or , And
- All
- Between
- Any

# Normalization

## 1st Normal Form
- Todos los atributos son atómicos
- La tabla contiene una clave primaria unica (PK)
- La PK no contiene atributos nulos
- No debe existir variación en el número de columnas
- Los campos no clave deben identificarse por la clave (Dependencia Funcional)
- Debe existir una independencia del orden tanto de las filas como de las columnas

## 2nd Normal Form
- Una relación está en 2FN si está en 1FN
- Los atributos que no forman parte de ninguna clave dependen de forma completa de la PK (Reflexiva)
- Puedo obtener los datos de las otras columnas a través de su PK (Aumentativa)
- Podemos usar llaves foráneas (FK) para obtener datos de las otras tablas (Transitiva)
- Si tengo una tabla A -> B y B -> C, NO necesito A->C para poder obtener los datos

## 3rd Normal Form
- Una relación está en 3FN si está en 2FN

## 4th Normal Form
- No podemos repetir datos en una tabla
- Solo tenemos combinaciones únicas
- Todas las llaves van a poder ser, sí o sí, PKs

## Truncate vs Delete

1. **Efficiency:**
    - `TRUNCATE` is generally more efficient than `DELETE` for large datasets because it doesn't generate individual row deletion operations and doesn't log each deleted row.
    - `DELETE` can be less efficient because it involves logging each row deletion and may have additional overhead.
2. **Rollback:**
    - `TRUNCATE` cannot be rolled back. Once a `TRUNCATE` operation is executed, it cannot be undone.
    - `DELETE` can be rolled back within a transaction, allowing you to undo the changes made by the `DELETE` statement.
3. **Identity Columns:**
    - The behavior regarding identity columns (auto-incrementing columns) may vary between database systems.
    - Some databases automatically reset identity columns to their initial seed value after a `TRUNCATE` operation, while others may not.
    - Deleting rows using `DELETE` does not necessarily reset identity columns unless explicitly specified.
4. **DML and DDL:**
    - `TRUNCATE` is a DDL (Data Definition Language) command.
    - `DELETE` is a DML (Data Manipulation Language) command.
5. **Speed:**
    - `TRUNCATE` is much faster than `DELETE`, as it doesn't scan every record before removing it.
    - `TRUNCATE` removes the data by deallocating the data pages used to store the table's data, and only the page deallocations are recorded in the transaction log.
    - `DELETE` removes rows one at a time and records an entry in the transaction log for each deleted row.
6. **Referential Integrity:** 
    - `TRUNCATE` cannot be used if other tables reference the table containing the data to be truncated using foreign key constraints.
    - `DELETE` can be used regardless of whether or not other tables reference the table containing the data to be deleted.
7. **Conditions:**
    - `TRUNCATE` cannot be used with a `WHERE` clause.
    - `DELETE` can be used with or without a `WHERE` clause.

## Store Procedure vs Function

- Function
    - Must have at least one parameter
    - MUST return a value
    - Cannot alter the data they receive as parameters (the arguments)
    - Generally cannot contain transaction control statements, and they are meant for read-only operations.
    - Called directly as part of a SELECT statement or used in an expression.
    - Cannot call another function or stored procedure.

- Store Procedure
    - Stored procs do not have to have a parameter,
    - Can change database objects
    - Do not have to return a value
    - Can contain transaction control statements (e.g., COMMIT, ROLLBACK).
    - Allows the use of TRY...CATCH blocks for error handling.
    - Called using the EXECUTE or EXEC keyword.
    - Can call another stored procedure.

## Joins in SQL

- Inner join
- Outer join
    - Left Join  (Left outer join)
    - Right Join (Right outer join)
    - Full Join (Full outer join)
- Self join
    - A self join is a specific case of a SQL join where a table is joined with itself. In other words, you use the same table on both sides of the join operation. This is useful when you want to compare rows within the same table or when dealing with hierarchical data stored in a single table.

## Distinct

- The `DISTINCT` keyword in SQL is used in a `SELECT` statement to eliminate duplicate rows from the result set. When you use `DISTINCT`, the database engine considers only unique values for the specified columns, and it returns a result set with those unique values.

## Could you explain me indexes in SQL ?

1. **Purpose of Indexes:**
    - **Improve Query Performance:** Indexes can significantly speed up the retrieval of rows that satisfy a certain condition in the WHERE clause of a query.
    - **Enforce Uniqueness:** Unique indexes ensure that the values in a particular column (or combination of columns) are unique across all rows in the table.
    - **Primary Key:** A primary key constraint is essentially an index that enforces the uniqueness of the primary key column(s) and allows for fast retrieval of individual rows.
2. **Types of Indexes:**
    - **Single-Column Index:** Created on a single column.
    - **Composite Index:** Created on multiple columns. Useful when queries involve conditions on multiple columns.
    - **Unique Index:** Enforces uniqueness on the indexed columns.
    - **Clustered Index:** Determines the physical order of rows in a table based on the indexed columns. Each table can have only one clustered index.
    - **Non-Clustered Index:** Does not affect the physical order of the table rows and is stored separately from the actual data. A table can have multiple non-clustered indexes.
3. **Creating Indexes:**
    - Indexes can be created when a table is initially defined or added later using the `CREATE INDEX` statement.
    - Example of creating a simple index:

        ```sql
        CREATE INDEX idx_employee_name ON employees (last_name);
        ```

## Pros of SQL Databases:

- *Strong consistency and data integrity.*
- *Well-suited for complex queries and relationships between data.*
- *Mature technology with a wide range of tools and support.*
- *ACID compliance ensures reliable transactions.*

## Cons of SQL  Databases:

- *Lack of flexibility in handling unstructured or semi-structured data.*
- *Vertical scalability can be costly.*
- *Not ideal for rapidly evolving schemas or applications requiring high write throughput.*

## ACID principles to transactions

- A transaccion es una operacion en SQL que modificia, agrega, elimina o toma datos de las tablas. Tiene que seguir los siguientes principios ACID:

- A -> atomicity
    - Si un paso dentro la transaccion falla, la operacion entera es cancelada.
    - Commit and Rollback

- C -> consistency
    - En cuanto a los tipos de datos. Fuertemente tipado

- I -> Isolation
    - Aislamiento de transacciones quiere decir que cada transaccion se ejecuta independiemtente de otra de manera ordenada sin afectar la anterior o la posterior.

- D -> Durability
    - Si una transaccion ha sido finalizada y la base de datos se apaga y se vuelve a prender los datos van seguir estando alli.
    - Si incluso falla tambien puedes recuperar tu data

## Normalization 
