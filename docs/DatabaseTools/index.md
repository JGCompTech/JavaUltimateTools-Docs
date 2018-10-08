#Database Tools Documentation

##Introduction
Package Name: com.jgcomptech.tools.databasetools.jdbc
Package Name: com.jgcomptech.tools.databasetools.jdbc.builders

Contains tools to communicate with a database using JDBC.
The goal behind this class is to allow connection with the
database and creation of auto-generated SQL statements so manually
creating native SQL code is not needed. This includes initial table creation
which is not normaly included in most database frameworks.

##Supported Databases
- H2
- HyperSQL
- SQLite

##SQL Statement Builders
- ColumnBuilder 	
    * Used for creating a database table column.
- DeleteBuilder 	
    * Used for creating a DELETE sql statement to delete a row from a table.
- IndexBuilder 	
    * Used for creating a CREATE sql statement to create a new table index.
- InsertBuilder 	
    * Used for creating an INSERT sql statement to add a row to a table.
- QueryBuilder 	
    * Used for creating a SELECT sql statement to query the database.
- TableBuilder 	
    * Used for creating an CREATE sql statement to create a new table.
- UpdateBuilder 	
    * Used for creating an UPDATE sql statement to update a row in a table.
- WhereBuilder 	
    * Used for creating a WHERE sql statement to set a constraint on another statement.