#Session Manager

##Introduction
Package Name: [com.jgcomptech.tools.authc.SessionManager](https://static.javadoc.io/com.jgcomptech.tools/java-ultimate-tools/1.5.0/com/jgcomptech/tools/authc/SessionManager.html)

!!! warning
    This class is embedded in the [[../AuthManager | AuthManager]],
    [[../Subject | Subject]], and [[../UserManager | UserManager]] classes
    and is not meant to be accessed directly except in advanced use cases.<br/><br/>
    To access the AuthManager's embedded SessionManager instance use the following code:
    ``` java
    final var authManager = AuthManager.getNewInstance(db);
    final var sessionManager = authManager.getSessionManager();
    ```
    To access the UserManager's embedded SessionManager instance use the following code:
    ``` java
    final var userManager = new UserManager(db);
    final var sessionManager = userManager.getSessionManager();
    ```
    
The Session Manager class contains all methods needed for managing
login sessions to allow users to login to your application.
This includes completing login via either using the UsernamePasswordToken
or requesting credentials from the user via a login dialog.

##Setup
The Session Manager is not a singleton class so a new instance needs to be initialized.
Since the Session Manager is a low level class it also requires an instance
of the User Manager in addition to the database instance.
The following code can be used to create a new instance:

!!! example
    ``` java
    try(final var db = new Database("./mydb.db", DatabaseType.H2)) {
        final var userManager = new UserManager(db);
        final var sessionManager = new SessionManager(userManager);
    }
    ```
    
An icon path and program name may also be optionally provided as parameters to change the
icon and program name for all login dialogs.

##Session Explanation
A login Session is a temporary connection established by a user with a login,
until either the user or the application/service terminates the connection.

The user must only provide credentials for authentication at the start
of a login session. The user is then authorized to use some or all of the services
offered by the application/service depending on the permissions assigned to the user.

Within JUT there are two types of login sessions, single-user and multi-user.
All of the following documentation will be separated into two sections one
for each session type/context.

###Single-User Session Context
Single-User sessions are meant for frontend login to an application.
These sessions can either be initiated via a UsernamePasswordToken or
via a login dialog. Only one user can be logged in at one time thus meaning
only one session can be open at a time. Some of the methods that are part
of the Session Manager can only be used under a Single-User context.

###Multi-User Session Context
Multi-User sessions are meant for backend login to an application, usually
via a remote client software. These sessions can only be initiated
via a UsernamePasswordToken. Almost an unlimited amount of users can be logged in
at one time and is only dependant on system resources and can be limited
via the setMaxSessions method. Some of the methods that are part
of the Session Manager can only be used under a Multi-User context.

##Single-Session Methods

###Get Logged-In Username (getLoggedInUsername)
Returns the username of the currently logged in user under the single session context.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final String currentUsername = sessionManager.getLoggedInUsername();
    ```
    
###Is Any User Logged-In (isUserLoggedIn)
Checks if a user is currently logged in under the single-user context.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.isUserLoggedIn();
    if(result) MessageBox.show("A user is logged-in!");
    else MessageBox.show("No user is logged-in!");
    ```
    
###Is A Specific User Logged-In (isUserLoggedIn)
Checks if the specified user is currently logged in under the single-user context.
To check if a specific user is logged-in, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.isUserLoggedIn("admin");
    if(result) MessageBox.show("Admin user is logged-in!");
    else MessageBox.show("Admin user is not logged-in!");
    ```
    
###Get Currently Logged-In User Session (getSession)
Retrieves the current session for the specified username, under the single-user context.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final Session currentSession = sessionManager.getSession();
    ```
    
###Get Specific User Session (getSession)
Retrieves the current session for the specified username, under the single-user context.

To retrieve a specific user session, supply the following parameter:

- Username - The username of the account to lookup

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final Session session = sessionManager.getSession("admin");
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.showLoginWindow(true);
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.loginUser("admin");
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.logoutUser();
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.logoutUser("admin");
    if(result) MessageBox.show("User was logged-out successfully!");
    else MessageBox.show("User logout failed!");
    ```

###Get Currently Logged-In User's UserRole (getLoggedInUserRole)
Retrieves the user role of the currently logged in user under
the single user context. If no user is currently logged-in then this
method will return null.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final UserRole result = sessionManager.getLoggedInUserRole();
    ```
    
###Check If An Admin User Is Logged-In (isAdminLoggedIn)
Checks if an admin user is currently logged in under the single user context.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.isAdminLoggedIn();
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.getAdminOverride();
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.getUserVerification();
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.requireAdmin();
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.requireAdmin();
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.isUserLoggedIn("admin", true);
    if(result) MessageBox.show("Admin user is logged-in!");
    else MessageBox.show("Admin user is not logged-in!");
    ```
    
###Get Max Allowed Sessions (getMaxSessions)
Retrieves the maximum number of allowed sessions,
under the multi session context, before login is disabled.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final int result = sessionManager.getMaxSessions();
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
    final var sessionManager = new SessionManager(userManager);
    //Set max sessions to 30.
    sessionManager.setMaxSessions(30);
    //Block all session creation by setting to 0
    sessionManager.setMaxSessions(0);
    ```
    
!!! note
    Setting this value to a negative number will disable the limit
    because a negative amount of sessions can never be created.
    When the SessionManager is initialized the value is initially set to -1.
    
###Get Session Count (getSessionsCount)
Retrieves the current number of logged in sessions under the multi-user context.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final int result = sessionManager.getSessionsCount();
    MessageBox.show("The max number of logged-in sessions is " + result + '!');
    ```
    
###Get All Sessions (getSessions)
Retrieves the current logged in sessions as a HashMap under the multi session context.
The returned HashMap key is the session username and the value is the related Session object.

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final Map<String, Session> sessions = sessionManager.getSessions();
    MessageBox.show("The max number of logged-in sessions is " + sessions.size() + '!');
    ```
    
###Get Specific User Session (getSession)
Retrieves the current session for the specified username, under the multi-user context.

To retrieve a specific user session, supply the following parameters:

- Username - The username of the account to lookup
- true - The true boolean is required to operate under the multi-user context

!!! example
    ``` java
    final var sessionManager = new SessionManager(userManager);
    final Session session = sessionManager.getSession("admin", true);
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.loginUser("admin", true);
    if(result) MessageBox.show("User was logged-in successfully!");
    else MessageBox.show("User login failed!");
    ```
    
This example adds password checking to verify credentials before login:

!!! example
    ``` java
    final var userManager = new UserManager(db);
    final var sessionManager = new SessionManager(userManager);
    if(userManager.checkPasswordMatches("admin", "1234")) {
        final boolean result = sessionManager.loginUser("admin", true);
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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.logoutUser("admin");
    if(result) MessageBox.show("User was logged-out successfully!");
    else MessageBox.show("User logout failed!");
    ```
    
##Other Methods

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
    final var sessionManager = new SessionManager(userManager);
    final boolean result = sessionManager.isNewSessionAllowed("admin", true);
    if(result) MessageBox.show("New User Session Is Allowed For Admin!");
    else MessageBox.show("New User Session Is Not Allowed For Admin!");
    ```
    
*[JUT]: Java Ultimate Tools