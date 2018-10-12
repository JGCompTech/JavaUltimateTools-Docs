#Dialogs Documentation

##Introduction
Package Name: com.jgcomptech.tools.dialogs

Contains JavaFX dialogs to use in your application.
Platform.runLater is not needed to be called since all dialogs use a
wrapper class that automatically calls runLater if it is needed.

##Features
- LoginDialog
    * Creates a Login Dialog for use to authenticate passwords.
    Allows for display of error message in case of login failure.
- MessageBox
    * Displays message box with specified options.
- FXMLDialogWrapper
    * Allows custom creation of a dialog using a specified JavaFX
    fxml file.

!!! note
     The MessageBox.Show() method is used throughout this documentation to show messages
     for demo example purposes. These code statements are not required for the example
     to function.
     
!!! info
    This page is a WIP and more documentation is coming soon.
    
*[WIP]: Work In Progress