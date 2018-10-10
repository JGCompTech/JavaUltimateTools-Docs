#Auth Manager

##Introduction
Package Name: [com.jgcomptech.tools.authc.AuthManager](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authc/AuthManager.html)

The Auth Manager class contains all methods needed for controlling the Auth System.
This includes methods to manage user accounts, login sessions, user roles and user permissions.

##Setup
The Auth Manager is a singleton class but a new instance still needs to be initialized upon first use.
When a new instance is created a "users" table is automatically created in the specified database.
The following code can be used to create a new instance:
``` java
try(final var db = new Database("./mydb.db", DatabaseType.H2)) {
    final var manager = AuthManager.getNewInstance(db);
}
```
    
After the new instance has been created you can use the following to retrieve the instance again:
``` java
try(final var db = new Database("./mydb.db", DatabaseType.H2)) {
    final var manager = AuthManager.getInstance();
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
    final var authManager = AuthManager.getInstance();
    authManager.createUser("admin", "1234", UserRoleManager.SystemUserRoles.ADMIN);
    authManager.createUser("editor", "1234", UserRoleManager.SystemUserRoles.EDITOR);
    ```
    
###Delete An Existing User (deleteUser)
To delete an existing user use the deleteUser method supplying the following parameter:

- Username - The username of the account to delete

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    authManager.deleteUser("admin");
    ```
    
###Get Username Object (getUser)
Retrieves a readonly immutable UserAccount object.
To retrieve the object supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final UserAccount user = authManager.getUser("admin");
    ```
    
###Check If User Exists (userExists)
Checks to see if the specified user account exists.
To check a user account, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.userExists("admin");
    if(result) MessageBox.show("Admin User Exists!");
    else MessageBox.show("Admin User Does Not Exist!");
    ```
    
###Get User Role (getUserRole)
Retrieves the UserRole object assigned to the specified user.
To retrieve the object, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final UserRole userRole = authManager.getUserRole("admin");
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
    final var authManager = AuthManager.getInstance();
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
    final var authManager = AuthManager.getInstance();
    authManager.setPassword("admin", "newPass");
    ```
    
###Check If Password Matches (checkPasswordMatches)
This method allows you to check if the supplied password matches
the user's stored password in the database.
To verify the password, supply the following parameters:

- Username - The username of the account to lookup
- Password - The password to verify
    
!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.checkPasswordMatches("admin", "1234");
    if(result) MessageBox.show("Password Matches!");
    else MessageBox.show("Password Does Not Match!");
    ```

###Lock/Unlock A User (lockUser and unlockUser)
A locked user account prevents login and prevents any new sessions
to be opened for the user. Locking an user account may be needed for numerous reasons.

To lock and unlock an account, supply the following parameter:

- Username - The username of the account to lock/unlock

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    //Lock The Account
    authManager.lockUser("admin");
    //Unlock The Account
    authManager.unlockUser("admin");
    ```
    
###Check If A User Is Locked (isUserLocked)
A locked user account prevents login and prevents any new sessions
to be opened for the user. Locking an user account may be needed for numerous reasons.

To check if an account is locked, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isUserLocked("admin");
    if(result) MessageBox.show("Admin Account Is Locked!");
    else MessageBox.show("Admin Account Is Unlocked!");
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
    final var authManager = AuthManager.getInstance();
    //Sets the date to now
    authManager.setPasswordExpirationDate("admin", LocalDateTime.now());
    //Sets the date to 30 days from today
    authManager.setPasswordExpirationDate("admin", LocalDateTime.now().plusDays(30));
    //First Method To Set the date to January 1st, 2019 at 8am
    authManager.setPasswordExpirationDate("admin", LocalDateTime.of(2019, 1, 1, 8, 0));
    //Second Method To Set the date to January 1st of 2019 at 8am
    authManager.setPasswordExpirationDate("admin", LocalDateTime.of(2018, Month.JANUARY, 1, 8, 0));
    //Third Method To Set the date to January 1st of 2019 at 8am
    final String str = "2019-01-01 08:00";
    final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    authManager.setPasswordExpirationDate("admin", LocalDateTime.parse(str, formatter));
    ```
    
###Disable Password Expiration (disablePasswordExpiration)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

To disable password expiration for a user, supply the following parameter:

- Username - The username of the account to update

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    authManager.disablePasswordExpiration("admin");
    ```
    
###Check If User Password Is Expired (isPasswordExpired)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

To check if a user's password is expired, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isPasswordExpired("admin");
    if(result) MessageBox.show("Admin Account Password Is Expired!");
    else MessageBox.show("Admin Account Password Is Not Expired!");
    ```
    
###Check If User Password Has Expiration Date (isPasswordSetToExpire)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

To check if a user's password has an expiration date, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isPasswordSetToExpire("admin");
    if(result) MessageBox.show("Admin Account Password Has An Expiration Date!");
    else MessageBox.show("Admin Account Password Does Not Have An Expiration Date!");
    ```
    
###Get User Password Expiration Date (getPasswordExpirationDate)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

There is two methods to retrieving the password expiration date.
The date can either be returned as a LocalDateTime object or as a formatted string.

!!! note
    If the expiration date has been disabled or was never enabled this method will
    still return an expiration date. For logistical database reasons a date still
    has to be set. So the date will be set to 1000 years after the user account
    was created or 1000 years after the date when the password expiration date
    was last disabled. This number can be updated if any bugs occur but it seems
    like 1000 years is far enough in the future to not cause any problems.

####LocalDateTime Object
To retrieve the password expiration date, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    final LocalDateTime result = authManager.getPasswordExpirationDate("admin");
    MessageBox.show("Admin Account Password Expires At " + result.format(formatter));
    ```
    
####Formatted String Object
To retrieve the password expiration date, supply the following parameters:

- Username - The username of the account to lookup
- Format - The string that represents the format to return the date

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final String result = authManager.getPasswordExpirationDate("admin", "yyyy-MM-dd HH:mm");
    MessageBox.show("Admin Account Password Expires At " + result);
    ```
    
##Single-Session Methods

###Get Logged-In Username (getLoggedInUsername)
Returns the username of the currently logged in user under the single session context.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final String currentUsername = authManager.getLoggedInUsername();
    ```
    
###Is Any User Logged-In (isUserLoggedIn)
Checks if a user is currently logged in under the single-user context.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isUserLoggedIn();
    if(result) MessageBox.show("A user is logged-in!");
    else MessageBox.show("No user is logged-in!");
    ```
    
###Is A Specific User Logged-In (isUserLoggedIn)
Checks if the specified user is currently logged in under the single-user context.
To check if a specific user is logged-in, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isUserLoggedIn("admin");
    if(result) MessageBox.show("Admin user is logged-in!");
    else MessageBox.show("Admin user is not logged-in!");
    ```
    
###Get Currently Logged-In User Session (getSession)
Retrieves the current session for the specified username, under the single-user context.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final Session currentSession = authManager.getSession();
    ```
    
###Get Specific User Session (getSession)
Retrieves the current session for the specified username, under the single-user context.

To retrieve a specific user session, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final Session session = authManager.getSession("admin");
    ```
    
###Show Login Dialog (showLoginWindow)
Shows the login dialog window to log a user into the single-user context.
The method returns true if a user was logged in successfully.
Fires either the sessionLoginSuccess or the sessionLoginFailure event
allowing the getUser method to be called by the assigned EventHandler.
If the user does not exist, getUser will return null.

To show the login dialog, supply the following parameter:

- true/false - If true, if during the login process an error occurs
the login dialog will show again listing the error text above the
username field. If false, if during the login process an error occurs
the entire method will return false.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.showLoginWindow(true);
    if(result) MessageBox.show("User was logged-in successfully!");
    else MessageBox.show("User login failed!");
    ```
    
###Login A Specific User (loginUser)
Logs in a user, under the single-user context, with the specified username,
without checking for valid credentials.

To retrieve a specific user session, supply the following parameter:

- Username - The username of the account to lookup

!!! warning
    This method does not validate credentials and should not be used except
    in advanced use cases. A example case could be to login a user during unit testing.
    The method showLoginWindow should be used instead. With this method
    the user is immediately logged in as long as the user exists and other
    checks pass (account unlocked, user role enabled and password not expired).
    
!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.loginUser("admin");
    if(result) MessageBox.show("User was logged-in successfully!");
    else MessageBox.show("User login failed!");
    ```
    
###Logout Current Logged-In User (logout)
Logs out the specified user from the single session context
and clears any set permissions. This method will return false
if no user is currently logged-in.


!!! caution
    If the user was deleted from the database while logged in, then
    the getUser event method that is accessible from the
    assigned event handler will return null.
    
!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.logoutUser();
    if(result) MessageBox.show("User was logged-out successfully!");
    else MessageBox.show("User logout failed!");
    ```
    
###Logout Specific User (logout)
Logs out the specified user from the single session context
and clears any set permissions. This method will return false
if no user is currently logged-in or if the specified user does not exist.

To logout a specific user, supply the following parameter:

- Username - The username of the account to logout

!!! caution
    If the user was deleted from the database while logged in, then
    the getUser event method that is accessible from the
    assigned event handler will return null.
    
!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.logoutUser("admin");
    if(result) MessageBox.show("User was logged-out successfully!");
    else MessageBox.show("User logout failed!");
    ```

###Get Currently Logged-In User's UserRole (getLoggedInUserRole)
Retrieves the user role of the currently logged in user under
the single user context. If no user is currently logged-in then this
method will return null.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final UserRole result = authManager.getLoggedInUserRole();
    ```
    
###Check If An Admin User Is Logged-In (isAdminLoggedIn)
Checks if an admin user is currently logged in under the single user context.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isAdminLoggedIn();
    if(result) MessageBox.show("An Admin user is logged-in!");
    else MessageBox.show("An Admin user is not logged-in!");
    ```
    
###Get Admin Login Override (getAdminOverride)
Requests an admin user to authenticate to override permissions by
showing the login dialog using the single session context. During this
process the admin user is not actually logged-in, instead just their
username, password and user role is verified. Returns true if
admin override succeeded and false if the cancel button was pressed
or if override failed.
Fires either the sessionLoginSuccess or the sessionLoginFailure event
allowing the getUser method to be called by the assigned EventHandler.
If the user does not exist, getUser will return null.

To show the login dialog, supply the following parameter:

- true/false - If true, if during the login process an error occurs
the login dialog will show again listing the error text above the
username field. If false, if during the login process an error occurs
the entire method will return false.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.getAdminOverride();
    if(result) MessageBox.show("Admin override succedded!");
    else MessageBox.show("Admin override failed!");
    ```
    
###Get User Login Verification (getUserVerification)
Requests that the currently logged in username to re-login using
the single session context. During this process the user is not
actually logged-in again, instead just their username and password is
verified. Returns true if the user verification succeeded and false
if the cancel button was pressed or if override failed.
Fires either the sessionLoginSuccess or the sessionLoginFailure event
allowing the getUser method to be called by the assigned EventHandler.
If the user does not exist, getUser will return null.

To show the login dialog, supply the following parameter:

- true/false - If true, if during the login process an error occurs
the login dialog will show again listing the error text above the
username field. If false, if during the login process an error occurs
the entire method will return false.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.getUserVerification();
    if(result) MessageBox.show("User verification succedded!");
    else MessageBox.show("User verification failed!");
    ```
    
###Require That Admin Is Logged-In (requireAdmin)
Checks that the logged-in user, under the single session context,
is an admin and if not, requests an admin override.
During this process the user is not actually logged-in again,
instead their username, password and user role is just verified.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.requireAdmin();
    if(result) MessageBox.show("Admin check passed!");
    else MessageBox.show("Admin check failed!");
    ```
    
###Require That Admin Is Logged-In And Request Verification (requireAndVerifyAdmin)
Checks that the logged-in user, under the single session context,
is an admin, if true, prompts the admin to re-login and
if false, requests an admin override.
During this process the user is not actually logged-in again,
instead their username, password and user role is just verified.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.requireAdmin();
    if(result) MessageBox.show("Admin check passed!");
    else MessageBox.show("Admin check failed!");
    ```

##Multi-Session Methods

###Is A Specific User Logged-In (isUserLoggedIn)
Checks if the specified user is currently logged in under the multi-user context.
To check if a specific user is logged-in, supply the following parameters:

- Username - The username of the account to lookup
- true - The true boolean is required to operate under the multi-user context

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isUserLoggedIn("admin", true);
    if(result) MessageBox.show("Admin user is logged-in!");
    else MessageBox.show("Admin user is not logged-in!");
    ```
    
###Get Max Allowed Sessions (getMaxSessions)
Retrieves the maximum number of allowed sessions,
under the multi session context, before login is disabled.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final int result = authManager.getMaxSessions();
    MessageBox.show("The max number of allowed sessions is " + result + '!');
    ```
    
###Set Max Allowed Sessions (setMaxSessions)
Sets the maximum number of allowed sessions,
under the multi session context, before login is disabled.
To disable the limit set the value to a negative number.
To set the maximum number of allowed sessions, supply the following parameter:

- value - The number of allowed sessions to set

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    //Set max sessions to 30.
    authManager.setMaxSessions(30);
    //Block all session creation by setting to 0
    authManager.setMaxSessions(0);
    ```
    
!!! note
    Setting this value to a negative number will disable the limit
    because a negative amount of sessions can never be created.
    When the SessionManager is initialized the value is initially set to -1.
    
###Get Session Count (getSessionsCount)
Retrieves the current number of logged in sessions under the multi-user context.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final int result = authManager.getSessionsCount();
    MessageBox.show("The max number of logged-in sessions is " + result + '!');
    ```
    
###Get All Sessions (getSessions)
Retrieves the current logged in sessions as a HashMap under the multi session context.
The returned HashMap key is the session username and the value is the related Session object.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final Map<String, Session> sessions = authManager.getSessions();
    MessageBox.show("The max number of logged-in sessions is " + sessions.size() + '!');
    ```
    
###Get Specific User Session (getSession)
Retrieves the current session for the specified username, under the multi-user context.

To retrieve a specific user session, supply the following parameters:

- Username - The username of the account to lookup
- true - The true boolean is required to operate under the multi-user context

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final Session session = authManager.getSession("admin", true);
    ```
    
###Login A Specific User (loginUser)
Logs in a user, under the multi-user context, with the specified username,
without checking for valid credentials.

To retrieve a specific user session, supply the following parameters:

- Username - The username of the account to lookup
- true - The true boolean is required to operate under the multi-user context

!!! warning
    This method does not validate credentials and should be used carefully.
    With this method the user is immediately logged in as long as the user
    exists and other checks pass (account unlocked, user role enabled and
    password not expired). An example is listed below on how to implement this.
    
!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.loginUser("admin", true);
    if(result) MessageBox.show("User was logged-in successfully!");
    else MessageBox.show("User login failed!");
    ```
    
This example adds password checking to verify credentials before login:

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    if(authManager.checkPasswordMatches("admin", "1234")) {
        final boolean result = authManager.loginUser("admin", true);
        if(result) MessageBox.show("User was logged-in successfully!");
        else MessageBox.show("User login failed!");
    } else MessageBox.show("User login failed!");
    ```
    
###Logout Specific User (logout)
Logs out the specified user from the single session context
and clears any set permissions. This method will return false
if no user is currently logged-in or if the specified user does not exist.

To logout a specific user, supply the following parameters:

- Username - The username of the account to logout
- true - The true boolean is required to operate under the multi-user context

!!! caution
    If the user was deleted from the database while logged in, then
    the getUser event method that is accessible from the
    assigned event handler will return null.
    
!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.logoutUser("admin");
    if(result) MessageBox.show("User was logged-out successfully!");
    else MessageBox.show("User logout failed!");
    ```
    
##Other Session Methods

###Is New Session Allowed (isNewSessionAllowed)
Checks if creation of a new session for the specified username is allowed
under the specified context. This method checks to see if the specified user
is already logged-in and if the second parameter is true checks to see if the
max sessions limit has been reached.

To retrieve a specific user session, supply the following parameters:

- Username - The username of the account to lookup
- true/false - The true for the multi-user context, false for the single-user context

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final boolean result = authManager.isNewSessionAllowed("admin", true);
    if(result) MessageBox.show("New User Session Is Allowed For Admin!");
    else MessageBox.show("New User Session Is Not Allowed For Admin!");
    ```

##Other Methods

###Get All Username Objects (getUsersList)
Retrieves a readonly immutable UserAccount object for each user
account and returns the list as a HashSet.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final HashSet<UserAccount> users = authManager.getUsersList();
    ```
    
###Get List Of All Usernames (getUsernameList)
Retrieves a list of the user names in the database.

!!! example
    ``` java
    final var authManager = AuthManager.getInstance();
    final HashSet<String> users = authManager.getUsernameList();
    ```