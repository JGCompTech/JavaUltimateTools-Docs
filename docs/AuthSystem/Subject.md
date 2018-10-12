#Subject

##Introduction
Package Name: [com.jgcomptech.tools.authc.Subject](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authc/Subject.html)

A Subject is just fancy security term that basically means a security-specific ‘view’ of an application user.
In JUT the Subject object handles the management of one specific user account.
The subject allows management of the user settings, role, permissions, and sessions.

##Setup
A Subject object instance can be created by providing an instance of the Auth Manager or by
calling the getSubject method on the Auth Manager. Multiple subject instances can exist but multiple
instances are only useful if using the multi-user context.

####Create New Instance Via AuthManager
``` java
final var manager = AuthManager.getInstance();
final Subject subject = manager.getSubject();
```

####Create New Instance Via New Subject Object
``` java
final var manager = AuthManager.getInstance();
final Subject subject = new Subject(manager);
```

##General Subject Methods

###Login User (login)
A new instance of a subject is considered anonymous until a user is logged-in.
A subject login requires a UsernamePasswordToken to pass in the user's credentials.
For security reasons, passwords supplied to the UsernamePasswordToken must be converted to a char array.

There is 4 methods supplied for logging in a user:

####Login Under Single-User Context
To login a user, supply the following parameter:

- Token - The UsernamePasswordToken containing user credentials

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final var token = new UsernamePasswordToken("admin", "1234".toCharArray());
    final boolean result = subject.login(token);
    if(result) MessageBox.show("Login Succeeded!");
    else MessageBox.show("Login Failed!");
    ```

####Login Under Either Context
To login a user, supply the following parameters:

- Token - The UsernamePasswordToken containing user credentials
- True/False - If true, login occurs under the Multi-User Context, if false, Single-User Context

Multi-User Context Example:

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final var token = new UsernamePasswordToken("admin", "1234".toCharArray());
    final boolean result = subject.login(token, true);
    if(result) MessageBox.show("Login Succeeded!");
    else MessageBox.show("Login Failed!");
    ```

####Login Under Single-User Context With Previous Token
For previous token to be saved, setRememberMe needs to be set to true during token creation.
Also if no token is saved CredentialsException will be thrown.
To login a user, no parameters are needed.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final var token = new UsernamePasswordToken("admin", "1234".toCharArray());
    final boolean result = subject.login();
    if(result) MessageBox.show("Login Succeeded!");
    else MessageBox.show("Login Failed!");
    ```

####Login Under Either Context With Previous Token
For previous token to be saved, setRememberMe needs to be set to true during token creation.
Also if no token is saved CredentialsException will be thrown.
To login a user, supply the following parameters:

- True/False - If true, login occurs under the Multi-User Context, if false, Single-User Context

Multi-User Context Example:

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final var token = new UsernamePasswordToken("admin", "1234".toCharArray());
    final boolean result = subject.login(true);
    if(result) MessageBox.show("Login Succeeded!");
    else MessageBox.show("Login Failed!");
    ```
    
###Logout User (logout)
When logging out a user, unless the setRememberMe is set to true during the token
creation, the subject username and token are cleared thus making the subject anonymous again.
This method will return false if the user is not already logged-in.

There is 4 methods supplied for logging out a user:

####Logout Under Single-User Context
To logout a user, no parameters are needed.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.logout();
    if(result) MessageBox.show("Logout Succeeded!");
    else MessageBox.show("Logout Failed!");
    ```

####Logout Under Either Context
To logout a user, supply the following parameter:

- True/False - If true, login occurs under the Multi-User Context, if false, Single-User Context

Multi-User Context Example:

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.login(true);
    if(result) MessageBox.show("Logout Succeeded!");
    else MessageBox.show("Logout Failed!");
    ```
    
###Get Last Session Duration Time (getLastSessionDuration)
When a session is opened a timer starts recording the duration the session was open.
This duration can be retrieved after a session is closed with the subject.

There is 3 methods supplied for returning this value:

####Return Duration Object

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final Duration duration = subject.getLastSessionDuration();
    ```
    
####Return Text String With Milliseconds

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final String duration = subject.getLastSessionDurationStringFull();
    ```
    
####Return Text String Without Milliseconds

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final String duration = subject.getLastSessionDurationString();
    ```
    
###Check If Subject Is Anonymous (isAnonymous)
Checks if the subject is anonymous.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isAnonymous();
    if(result) MessageBox.show("Subject Is Anonymous!");
    else MessageBox.show("Subject Is Not Anonymous!");
    ```
    
###Check If Subject Is Authenticated Under Single-User Context (isAuthenticated)
Checks if the subject is anonymous.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isAuthenticated();
    if(result) MessageBox.show("Subject Is Anonymous!");
    else MessageBox.show("Subject Is Not Anonymous!");
    ```
    
###Check If Subject Is Authenticated Under Either Context (isAuthenticated)
Checks if the subject is anonymous.

To check status, supply the following parameter:

- True/False - If true, login occurs under the Multi-User Context, if false, Single-User Context

Multi-User Context Example:

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isAuthenticated(true);
    if(result) MessageBox.show("Subject Is Anonymous!");
    else MessageBox.show("Subject Is Not Anonymous!");
    ```
    
###Get Current Logged-In Session Under Single-User Context (getSession)
Retrieves the current logged in session or returns null if no session is open.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final Session session = subject.getSession();
    ```
    
###Get Current Logged-In Session Under Either Context (getSession)
Retrieves the current logged in session or returns null if no session is open.

To retrieve the current session, supply the following parameter:

- True/False - If true, login occurs under the Multi-User Context, if false, Single-User Context

Multi-User Context Example:

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final Session session = subject.getSession(true);
    ```
    
###Check If Username And Token Are Remembered (isRemembered)
Checks to see if the username and token are set to be saved after logout.
For credentials to be saved, setRememberMe needs to be set to true during token creation.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isRemembered();
    if(result) MessageBox.show("Subject Credentials Is Remembered!");
    else MessageBox.show("Subject Credentials Is Not Remembered!");
    ```
    
##User Management Methods

###Get Current Username (getUsername)
Retrieves the current username. Returns null if the subject is anonymous.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.getUsername();
    MessageBox.show("The Current Username Is " + result + '!');
    ```
    
###Set New Password (setPassword)
Sets a new password for the current user.

To set a new password, supply the following parameter:

- Password - The new password to set

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    subject.setPassword("pass");
    ```

###Get Current User Role (getUserRole)
Retrieves the UserRole object assigned to the current user.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final UserRole role = subject.getUserRole();
    ```

###Set User Role (setUserRole)
Sets the user role assigned to the specified user.
To set the user role, supply the following parameter:

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
    final var subject = AuthManager.getInstance().getSubject();
    subject.setUserRole(UserRoleManager.SystemUserRoles.ADMIN);
    ```

###Check If User Has Permission (hasPermission)
Checks if the current username has the specified permission.

To check for the permission, supply the following parameter:

- Permission - The name of the permission to check

The permission manager has the following built in permissions:

- admin
- edit
- create
- read

####Single Permission

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.hasPermission("admin");
    if(result) MessageBox.show("User Has Admin Permission!");
    else MessageBox.show("User Does Not Have Admin Permission!");
    ```
    
####HashSet Of Permissions

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final HashSet<String> list = new HashSet<>(Arrays.asList("admin", "edit"));
    final boolean result = subject.hasPermissions(list);
    if(result) MessageBox.show("User Has Specified Permissions!");
    else MessageBox.show("User Does Not Have Specified Permissions!");
    ```
    
####Comma Separated list Of Permissions:

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.hasPermissions("admin", "edit");
    if(result) MessageBox.show("User Has Specified Permissions!");
    else MessageBox.show("User Does Not Have Specified Permissions!");
    ```

###Get Account Creation Date (getUserCreationDate)
Returns the date and time the user was initially created.

There is two methods to retrieving the user creation date.
The date can either be returned as a LocalDateTime object or as a formatted string.

####LocalDateTime Object

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    final LocalDateTime result = subject.getUserCreationDate();
    MessageBox.show("Account Was Created On " + result.format(formatter));
    ```
    
####Formatted String Object
To retrieve the user creation date, supply the following parameter:

- Format - The string that represents the format to return the date

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final String result = subject.getUserCreationDate("yyyy-MM-dd HH:mm");
    MessageBox.show("Account Was Created On " + result);
    ```

###Check If User Is Locked (isUserLocked)
A locked user account prevents login and prevents any new sessions
to be opened for the user. Locking an user account may be needed for numerous reasons.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isUserLocked();
    if(result) MessageBox.show("Admin Account Is Locked!");
    else MessageBox.show("Admin Account Is Unlocked!");
    ```

###Lock/Unlock A User (lockUser and unlockUser)
A locked user account prevents login and prevents any new sessions
to be opened for the user. Locking an user account may be needed for numerous reasons.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    //Lock The Account
    subject.lockUser();
    //Unlock The Account
    subject.unlockUser();
    ```

###Check If Password Is Expired (isPasswordExpired)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isPasswordExpired();
    if(result) MessageBox.show("Admin Account Password Is Expired!");
    else MessageBox.show("Admin Account Password Is Not Expired!");
    ```

###Check If Password Has Expiration Date (isPasswordSetToExpire)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.isPasswordSetToExpire();
    if(result) MessageBox.show("Admin Account Password Has An Expiration Date!");
    else MessageBox.show("Admin Account Password Does Not Have An Expiration Date!");
    ```

###Get Password Expiration Date (getPasswordExpirationDate)
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

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    final LocalDateTime result = subject.getPasswordExpirationDate();
    MessageBox.show("Admin Account Password Expires At " + result.format(formatter));
    ```
    
####Formatted String Object
To retrieve the password expiration date, supply the following parameter:

- Format - The string that represents the format to return the date

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final String result = subject.getPasswordExpirationDate("yyyy-MM-dd HH:mm");
    MessageBox.show("Admin Account Password Expires At " + result);
    ```

###Set Password Expiration Date (setPasswordExpirationDate)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

To set a password expiration date, supply the following parameter:

- Expiration Date - A LocalDateTime object representing the expiration date

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    //Sets the date to now
    subject.setPasswordExpirationDate(LocalDateTime.now());
    //Sets the date to 30 days from today
    subject.setPasswordExpirationDate(LocalDateTime.now().plusDays(30));
    //First Method To Set the date to January 1st, 2019 at 8am
    subject.setPasswordExpirationDate(LocalDateTime.of(2019, 1, 1, 8, 0));
    //Second Method To Set the date to January 1st of 2019 at 8am
    subject.setPasswordExpirationDate(LocalDateTime.of(2018, Month.JANUARY, 1, 8, 0));
    //Third Method To Set the date to January 1st of 2019 at 8am
    final String str = "2019-01-01 08:00";
    final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
    subject.setPasswordExpirationDate(LocalDateTime.parse(str, formatter));
    ```

###Disable Password Expiration (disablePasswordExpiration)
Setting a password to expire after a specified date allows the prevention of login and
new session creation if the password is expired. Setting a password to expire could
be used to force regular password changes or allow for temporary accounts to be made.

!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    authManager.disablePasswordExpiration();
    ```

###Check If Password Matches (checkPasswordMatches)
This method allows you to check if the supplied password matches
the user's stored password in the database.
To verify the password, supply the following parameter:

- Password - The password to verify
    
!!! example
    ``` java
    final var subject = AuthManager.getInstance().getSubject();
    final boolean result = subject.checkPasswordMatches("1234");
    if(result) MessageBox.show("Password Matches!");
    else MessageBox.show("Password Does Not Match!");
    ```
    
*[JUT]: Java Ultimate Tools