# Notes_Database_Testing
### Knowledge of SQL 
#### Database Operations
- **Create Database**: Initializes a new database.
- **Drop Database**: Permanently removes an existing database.

#### Data Definition Language (DDL)
- **Create Table**: Defines a new table with specified columns.
- **Drop Table**: Deletes a table and all its data.
- **Truncate Table**: Removes all rows from a table without deleting the table structure.
- **Alter Table**:
  - **Add**: Introduces a new column to an existing table.
  - **Drop**: Removes a column from a table.
  - **Rename Column**: Changes the name of an existing column.

#### Data Manipulation Language (DML)
- **Select**: Retrieves data from one or more tables.
- **Insert Into Table**: Adds new rows to a table.
- **Delete Row From Table**: Removes specific rows from a table based on conditions.
- **Update Row**: Modifies existing data in a table.

#### Data Query Language (DQL)
- **Select**: Fetches data from the database.
  - **Where**: Filters records that meet specified conditions.
  - **Order By ASC|DESC**: Sorts the result set in ascending or descending order.
  - **Distinct**: Ensures unique results by eliminating duplicate rows.
  - **AND, OR, NOT**: Logical operators used to refine queries.
  - **LIKE**: Searches for a specified pattern in a column.
  - **IN**: Matches any value in a list of specified values.
  - **BETWEEN**: Selects values within a given range.
  - **Aliases**: Provides a temporary name to columns or tables.
  - **Select Top**: Limits the number of rows returned.

#### Aggregate Functions
- **Min**: Returns the smallest value.
- **Max**: Returns the largest value.
- **Sum**: Calculates the total of a numeric column.
- **Avg**: Computes the average of a numeric column.
- **Count**: Counts the number of rows.

#### Grouping and Filtering
- **Group By**: Groups rows sharing a property for aggregate functions.
- **Having**: Filters groups based on aggregate criteria.
- **Exists**: Checks for the existence of rows in a subquery.

#### Joins
- **Inner Join**: Returns records with matching values in both tables.
- **Left Join**: Returns all records from the left table and matching records from the right table.
- **Right Join**: Returns all records from the right table and matching records from the left table.
- **Full Join**: Returns all records when there is a match in either table.
- **Self Join**: Joins a table with itself.
- **Union**: Combines the result sets of two or more queries, removing duplicates.
