#User Role Manager

##Introduction
Package Name: [com.jgcomptech.tools.authc.UserRoleManager](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authc/UserRoleManager.html)

The User Role Manager class contains all methods needed for managing
User Roles that can be assigned to a User Account.

##User Role Explanation
Role-based Access Control (RBAC) is a common approach to restricting system access to authorized users.
A user role defines permissions for a user to perform a group of tasks.
Although every role has a predefined set of permissions,
new permissions can be added and removed from each role.
New custom roles can also be created as a new role or a copy of an existing role.

The base permissions are (Descriptions are based on a CMS implementation and are just examples):

* Admin - Allows changes to system settings.
* Edit - Allows editing of all content even other user's content.
* Create - Allows creation of new content and editing of own content.
* Read - Allows viewing of all content.

The base user roles are (Descriptions are based on a CMS implementation and are just examples):

* Admin(Admin, Edit, Create, Read) - Has all permissions.
* Editor(Edit, Create, Read) - Can create and edit all content but not change system settings.
* Author(Create, Read) - Can create new content but not edit other users content.
* Basic(Read) - Can read all content but not create or edit content.
* None() - Has no permissions.

!!! note
    Currently JUT is designed to allow a user to be assigned only one user role.
    This will be changing in future releases to allow for assignment of multiple roles.

##Setup
The User Role Manager is a singleton class and on first use is initialized automatically.
The following code can be used to retrieve the instance:
``` java
final var manager = UserRoleManager.getInstance();
```

##User Role Manager Methods

###Create A New User Role (createUserRole)
To create a new user role use the createUserRole method supplying the following parameter:

- Name - The name of the new user role

!!! note
    When using this method the UserRole object related to the new user role is returned.
    If a user role already exists with the specified name the already existing UserRole
    object is returned instead.
    
!!! example
    ``` java
    final var userRoleManager = UserRoleManager.getInstance();
    final var newRole = userRoleManager.createUserRole("moderator");
    newRole.modify().add("edit", "create", "read");
    ```
    
###Add Existing User Role (addExistingUserRole)
You can manually create a new UserRole object and directly add that to the list.
To add an existing user role use the addExistingUserRole method supplying the following parameter:

- UserRole - The UserRole object to add
    
!!! example
    ``` java
    final var userRoleManager = UserRoleManager.getInstance();
    final UserRole role = new UserRole("moderator");
    role.modify().add("edit", "create", "read");
    userRoleManager.addExistingUserRole(role);
    ```
    
###Get List Of All Installed User Roles (getUserRoles)
Retrieves an unmodifiable list of all user roles.

!!! example
    ``` java
    final var userRoleManager = UserRoleManager.getInstance();
    final Map<String, UserRole> roles = userRoleManager.getUserRoles();
    ```
    
###Get Specific User Role (getUserRole)
Retrieves the specified user role.

To retrieve a user role use the getUserRole method supplying the following parameter:

- Name - The name of the User Role to lookup

!!! example
    ``` java
    final var userRoleManager = UserRoleManager.getInstance();
    final UserRole role = userRoleManager.getUserRole("admin");
    ```

*[JUT]: Java Ultimate Tools
*[CMS]: Content Management System
*[RBAC]: Role-based Access Control