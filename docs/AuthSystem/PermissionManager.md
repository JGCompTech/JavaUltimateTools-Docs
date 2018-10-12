#Permission Manager

##Introduction
Package Name: [com.jgcomptech.tools.authz.PermissionManager](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authz/PermissionManager.html)

The Permission Manager manages all role and user based permissions.
Permissions have a hierarchy and thus if a permission is assigned to a
user or role then all its child permissions are also assigned. 

The Permission Manager has the following built in permissions:

- admin
- edit
- create
- read

##Setup
The Permission Manager is a singleton class and on first use is initialized automatically.
The following code can be used to retrieve the instance:
``` java
final var manager = PermissionManager.getInstance();
```

##Permission Methods

###Add New Custom Permission (addCustomPermission)
Adds new permission with the specified name and parent name, disabled by default.

To add a new custom permission, supply the following parameters:

- Name - The name of the permission to lookup
- Parent Name - The name of the parent permission

!!! note
    If you want the new permission to not have a parent and want it to be root level
    then set the Parent Name to null or an empty string.
    
!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.addCustomPermission("change_global_settings", "admin");
    ```
    
###Add And Enable New Custom Permission (addAndEnableCustomPermission)
Adds new permission with the specified name and parent name, enabled by default.

To add a new custom permission, supply the following parameters:

- Name - The name of the permission to lookup
- Parent Name - The name of the parent permission

!!! note
    If you want the new permission to not have a parent and want it to be root level
    then set the Parent Name to null or an empty string.
    
!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.addAndEnableCustomPermission("change_global_settings", "admin");
    ```
    
###Load Permissions (loadPermissions)
Loading permissions enables or disables permissions in bulk.
There is 3 ways of doing this, enable all, disable all or apply a user role's permissions.

####Enable All Permissions
Parameter must be true.

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.loadPermissions(true);
    ```
    
####Disable All Permissions
Parameter must be false.

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.loadPermissions(false);
    ```
    
####Enable/Disable Based On User Role
When run all permissions are disabled and then the permissions assigned to
the specified user role are enabled.

Parameter must be the name of a user role.

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.loadPermissions("admin");
    ```
    
###Remove Permission (removePermission)
Removes the specified permission.

To remove a permission, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.removePermission("admin");
    ```
    
###Check If Permission Exists (doesPermissionExist)
Checks if the specified permission exists.

To check if a permission exists, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final var result = permissionsManager.doesPermissionExist("admin");
    if(result) MessageBox.show("Permission Exists!");
    else MessageBox.show("Permission Not Found!");
    ```
    
###Enable Permission (enablePermission)
Enables the specified permission.

To enable a permission, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.enablePermission("admin");
    ```
    
###Disable Permission (disablePermission)
Disables the specified permission.

To disable a permission, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    permissionsManager.disablePermission("admin");
    ```
    
###Check If Permission Is Enabled (isPermissionEnabled)
Checks if the specified permission is enabled.

To check if a permission is enabled, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final var result = permissionsManager.isPermissionEnabled("admin");
    if(result) MessageBox.show("Permission Is Enabled!");
    else MessageBox.show("Permission Is Disabled!");
    ```
    
###Check If Permission Is Disabled (isPermissionDisabled)
Checks if the specified permission is disabled.

To check if a permission is disabled, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final var result = permissionsManager.isPermissionDisabled("admin");
    if(result) MessageBox.show("Permission Is Disabled!");
    else MessageBox.show("Permission Is Enabled!");
    ```
    
###Get List Of All Child Permissions (getPermissionChildren)
Retrieves an unmodifiable list of all the specified permission's child permissions.

To retrieve a list of all child permissions, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final HashSet<String> children = permissionsManager.getPermissionChildren("admin");
    ```

###Get List Of All Permissions (getPermissions)
Retrieves an unmodifiable list of all permissions.

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final Map<String, Permission> roles = permissionsManager.getPermissions();
    ```
    
###Get List Of All Permission Names (getPermissionNames)
Retrieves an unmodifiable list of names of all permissions.

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final Set<String> names = permissionsManager.getPermissionNames();
    ```
    
###Get Specific Permission (getPermission)
Retrieves the specified permission.

To retrieve a permission, supply the following parameter:

- Name - The name of the permission to lookup

!!! example
    ``` java
    final var permissionsManager = PermissionManager.getInstance();
    final UserRole role = permissionsManager.getPermission("admin");
    ```
