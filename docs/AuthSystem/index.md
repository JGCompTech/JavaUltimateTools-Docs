#Auth System Documentation

##Introduction
Package Name: com.jgcomptech.tools.authc
Package Name: com.jgcomptech.tools.authz

The JUT Auth System is a set of classes that allow for authentication and authorization.
This system uses the Database class to connect to a database to store all account info.

##Features
- Auth Manager
    * Contains all methods needed for controlling the Auth System.
- Subject
    * Manages all tasks related to the currently logged in user account.
- Session Manager
    * Manages all login sessions to allow users to login to your application.
- User Manager
    * Manages all user accounts in the database.
- User Role Manager
    * Manages all user account roles.
- Permission Manager
    * Manages permissions to be used to enable or disable parts of your application.