#Database Tools Documentation

##Introduction
Package Names:

* [com.jgcomptech.tools.databasetools.jdbc](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/databasetools/jdbc/package-summary.html)
* [com.jgcomptech.tools.databasetools.jdbc.builders](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/databasetools/jdbc/builders/package-summary.html)

Contains tools to communicate with a database using JDBC.
The goal behind this class is to allow connection with the
database and creation of auto-generated SQL statements so manually
creating native SQL code is not needed. This includes initial table creation
which is not normally included in most database frameworks.

##Supported Databases
- [H2](http://www.h2database.com/)
- [HyperSQL](http://hsqldb.org/)
- [SQLite](https://www.sqlite.org/)

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
    
!!! info
    This page is a WIP and more documentation is coming soon.
    
*[JDBC]: Java Database Connectivity
*[SQL]: Structured Query Language
*[sql]: Structured Query Language
*[WIP]: Work In Progress