#Auth System Documentation

##Introduction
Package Names:

* [com.jgcomptech.tools.authc](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authc/package-summary.html)
* [com.jgcomptech.tools.authz](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authz/package-summary.html)

The JUT Auth System is a set of classes that allow for authentication and authorization.
This system uses the Database class to connect to a database to store all account info.

##Features
- [[AuthManager | Auth Manager]]
    * Contains all methods needed for controlling the Auth System.
- [[PermissionManager | Permission Manager]]
    * Manages permissions to be used to enable or disable parts of your application.
- [[Subject]]
    * Manages all tasks related to the currently logged in user account.
- [[SessionManager | Session Manager]]
    * Manages all login sessions to allow users to login to your application.
- [[UserManager | User Manager]]
    * Manages all user accounts in the database.
- [[UserRoleManager | User Role Manager]]
    * Manages all user account roles.
    
*[JUT]:  Java Ultimate Tools