#User Manager

##Introduction
Package Name: [com.jgcomptech.tools.authc.UserManager](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authc/UserManager.html)

!!! warning
    This class is embedded in the [[../AuthManager | AuthManager]]
    and [[../Subject | Subject]] classes and is not meant to be
    accessed directly except in advanced use cases.<br/><br/>
    To access the AuthManager's embedded UserManager instance use the following code:
    ``` java
    final var authManager = AuthManager.getNewInstance(db);
    final var userManager = authManager.getUserManager();
    ```
    
The User Manager manages all user accounts in the database.
This is where all the native SQL code is automatically
generated using the Database Builder classes.

##Setup
The User Manager is not a singleton class so a new instance needs to be initialized.
When a new instance is created a "users" table is automatically created in the specified database.
The following code can be used to create a new instance:

!!! example
    ``` java
    try(final var db = new Database("./mydb.db", DatabaseType.H2)) {
        final var userManager = new UserManager(db);
    }
    ```
    
An icon path and program name may also be optionally provided as parameters to change the
icon and program name for all login dialogs.

##User Account Methods

###Create A New User (createUser)
To create a new user use the createUser method supplying the following parameters:

- Username - The username of the new account
- Password - The password of the new account
- User Role - The user role of the new account

The user role can be a text string or one of the
items from the UserRoleManager.SystemUserRoles enum.

The UserRoleManager.SystemUserRoles list contains:

- ADMIN
- AUTHOR
- EDITOR
- BASIC
- NONE

!!! note
    The NONE user role can never have permissions assigned to it.
    Using the NONE user role is a quick way to remove all permissions from a user.

!!! example
    ``` java
    final var userManager = new UserManager(db);
    userManager.createUser("admin", "1234", UserRoleManager.SystemUserRoles.ADMIN);
    userManager.createUser("editor", "1234", UserRoleManager.SystemUserRoles.EDITOR);
    ```
    
###Delete An Existing User (deleteUser)
To delete an existing user use the deleteUser method supplying the following parameter:

- Username - The username of the account to delete

!!! example
    ``` java
    final var userManager = new UserManager(db);
    userManager.deleteUser("admin");
    ```
    
###Get Username Object (getUser)
Retrieves a readonly immutable UserAccount object.
To retrieve the object, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var userManager = new UserManager(db);
    final UserAccount user = userManager.getUser("admin");
    ```
    
###Check If User Exists (userExists)
Checks to see if the specified user account exists.
To check a user account, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var userManager = new UserManager(db);
    final boolean result = userManager.userExists("admin");
    if(result) MessageBox.show("Admin User Exists!");
    else MessageBox.show("Admin User Does Not Exist!");
    ```
    
###Get User Role (getUserRole)
Retrieves the UserRole object assigned to the specified user.
To retrieve the object, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var userManager = new UserManager(db);
    final UserRole userRole = userManager.getUserRole("admin");
    ```
    
###Set User Role (getUserRole)
Sets the user role assigned to the specified user.
To set the user role, supply the following parameters:

- Username - The username of the account to lookup
- User Role - The user role of the new account

The user role can be a text string or one of the
items from the UserRoleManager.SystemUserRoles enum.

The UserRoleManager.SystemUserRoles list contains:

- ADMIN
- AUTHOR
- EDITOR
- BASIC
- NONE

!!! note
    The NONE user role can never have permissions assigned to it.
    Using the NONE user role is a quick way to remove all permissions from a user.

!!! example
    ``` java
    final var userManager = new UserManager(db);
    userManager.setUserRole("admin", UserRoleManager.SystemUserRoles.ADMIN);
    ```
    
###Set User Password (setPassword)
Sets the password assigned to the specified user.
To set the new password, supply the following parameters:

- Username - The username of the account to lookup
- Password - The new password to set

!!! note
    If you want to prevent a user from setting an empty password
    or to check for password complexity requirements those checks need
    to be implemented separately. This method allows empty passwords.
    Even if the password is empty, the empty password is still encrypted
    with a random secure salt just like any supplied password would be.

!!! example
    ``` java
    final var userManager = new UserManager(db);
    userManager.setPassword("admin", "newPass");
    ```
    
###Check If Password Matches (checkPasswordMatches)
This method allows you to check if the supplied password matches
the user's stored password in the database.
To verify the password, supply the following parameters:

- Username - The username of the account to lookup
- Password - The password to verify
    
!!! example
    ``` java
    final var userManager = new UserManager(db);
    final boolean result = userManager.checkPasswordMatches("admin", "1234");
    if(result) MessageBox.show("Password Matches!");
    else MessageBox.show("Password Does Not Match!");
    ```
    
###Lock/Unlock A User Account(setLockStatus)
A locked user account prevents login and prevents any new sessions
to be opened for the user. Locking an user account may be needed for numerous reasons.

To lock and unlock an account, supply the following parameters:

- Username - The username of the account to lock/unlock
- true/false - The value to set, true to lock, false to unlock

!!! example
    ``` java
    final var userManager = new UserManager(db);
    //Lock The Account
    userManager.setLockStatus("admin", true);
    //Unlock The Account
    userManager.setLockStatus("admin", false);
    ```
    
###Set User Password Expiration Date (setPasswordExpirationDate)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

To set a password expiration date, supply the following parameters:

- Username - The username of the account to update
- Expiration Date - A LocalDateTime object representing the expiration date

!!! example
    ``` java
    final var userManager = new UserManager(db);
    //Sets the date to now
    userManager.setPasswordExpirationDate("admin", LocalDateTime.now());
    //Sets the date to 30 days from today
    userManager.setPasswordExpirationDate("admin", LocalDateTime.now().plusDays(30));
    //First Method To Set the date to January 1st, 2019 at 8am
    userManager.setPasswordExpirationDate("admin", LocalDateTime.of(2019, 1, 1, 8, 0));
    //Second Method To Set the date to January 1st of 2019 at 8am
    userManager.setPasswordExpirationDate("admin", LocalDateTime.of(2018, Month.JANUARY, 1, 8, 0));
    //Third Method To Set the date to January 1st of 2019 at 8am
    final String str = "2019-01-01 08:00";
    final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    userManager.setPasswordExpirationDate("admin", LocalDateTime.parse(str, formatter));
    ```
    
###Disable Password Expiration (disablePasswordExpiration)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

To disable password expiration for a user, supply the following parameter:

- Username - The username of the account to update

!!! example
    ``` java
    final var userManager = new UserManager(db);
    userManager.disablePasswordExpiration("admin");
    ```

##Other Methods

###Get All Username Objects (getUsersList)
The getUsersList method retrieves a readonly immutable UserAccount object for each user
account and returns the list as a HashSet.

!!! example
    ``` java
    final var userManager = new UserManager(db);
    final HashMap<UserAccount> users = userManager.getUsersList();
    ```
    
###Get List Of All Usernames (getUsernameList)
Retrieves a list of the user names in the database.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final HashSet<String> users = authManager.getUsernameList();
    ```