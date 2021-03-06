<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.3.1/css/all.css" integrity="sha384-mzrmE5qonljUremFsqc01SB46JvROS7bZs3IO2EmfFsd15uHvIt+Y8vEf7N7fWAU" crossorigin="anonymous">

# Welcome to Java Ultimate Tools Docs
[![Build Status](https://travis-ci.org/JGCompTech/JavaUltimateTools.svg?branch=master)](https://travis-ci.org/JGCompTech/JavaUltimateTools) [![CircleCI](https://circleci.com/gh/JGCompTech/JavaUltimateTools.svg?style=svg)](https://circleci.com/gh/JGCompTech/JavaUltimateTools) [![Language grade: Java](https://img.shields.io/lgtm/grade/java/g/JGCompTech/JavaUltimateTools.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/JGCompTech/JavaUltimateTools/context:java) [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.jgcomptech.tools/java-ultimate-tools/badge.svg?style=flat-square)](https://maven-badges.herokuapp.com/maven-central/com.jgcomptech.tools/java-ultimate-tools/) [![Javadocs](http://www.javadoc.io/badge/com.jgcomptech.tools/java-ultimate-tools.svg?style=flat-square)](http://www.javadoc.io/doc/com.jgcomptech.tools/java-ultimate-tools)

## What is Java Ultimate Tools?

Java Ultimate Tools, or JUT for short, is a large powerful and flexible open-source repository for use in any Java program.

## Java Ultimate Tools Features
- OSInfo - Contains many classes that return information about the current Windows installation. This includes Architecture, Edition, Name, Product Key, Service Pack, User Info and Version.
- HWInfo - Contains many classes that return information about the current computer hardware. This includes BIOS, Network, OEM, Processor, RAM and Storage.
- SecurityTools - Contains methods surrounding hashing and encryption. Includes methods using MD5, SHA1, SHA256, SHA384 and SHA512. Also includes encryption/decryption with RSA.
- CommandInfo - Allows you to run any console command and will return the result to a string to use within your program. You can also run the command elevated and it will open in a new cmd window and show the results. Note: If elevated, result cannot be returned as a string.
- MessageBox and Login dialogs - Dialogs to use in JavaFX applications
- FXML Dialog Wrapper - Class to wrap a FXML dialog object, reducing the amount of required code
- DatabaseTools - Allows communication with H2, HyperSQL and SQLite databases
- SQL Statement Builders - Allows for using methods instead of native SQL code to generate Prepared Statements
- User Account Management Tools - Includes User Roles, Sessions and Permissions
- Custom Event Manager System - Creates Events similar to the JavaFX Events but not using any JavaFX classes
- Utility Classes - Includes classes for managing collections, numbers and strings
- And Much More!

**NOTE: This Project Has Now Been Updated To Use Java 10!!!**

## Development
Want to contribute? Great!
Any help with development is greatly appreciated. If you want to add something or fix any issues please submit a pull request and if it is helpful it may be merged. Please check out our [Code of Conduct for Contributors](https://github.com/JGCompTech/JavaUltimateTools/blob/master/code-of-conduct.md).

## Documentation
The documentation for JUT is currently a work in progress and new changes will be occurring soon.

## JavaDoc
The JavaDoc info is hosted at the following 2 locations:
<br/>[github.io(Current GitHub Branch)](https://jgcomptech.github.io/JavaUltimateTools/) - stored in the doc folder in the project repository on Github
<br/>[javadoc.io(Current Maven Release)](http://www.javadoc.io/doc/com.jgcomptech.tools/java-ultimate-tools) - the released JavaDoc jar on Maven Central

## Download JAR
**[<i class="fas fa-download"> Download v1.5.1</i>](https://github.com/JGCompTech/JavaUltimateTools/releases/tag/v1.5.1)**

The changelog can be found [[Changelog | here]].

## Using with Maven
If you are familiar with [Maven](http://maven.apache.org), add the following XML
fragments into your pom.xml file. With those settings, your Maven will automatically download our library into your local Maven repository, since our libraries are synchronized with the [Maven central repository](http://repo1.maven.org/maven2/com/jgcomptech/tools/java-ultimate-tools/).
 
``` tab="Maven"
<dependencies>
    <dependency>
        <groupId>com.jgcomptech.tools</groupId>
        <artifactId>java-ultimate-tools</artifactId>
        <version>1.5.1</version>
    </dependency>
</dependencies>
```
 
``` tab="Gradle Groovy DSL"
compile 'com.jgcomptech.tools:java-ultimate-tools:1.5.1'
```
 
``` tab="Gradle Kotlin DSL"
compile(group = "com.jgcomptech.tools", name = "java-ultimate-tools", version = "1.5.1")
```
 
``` tab="Scala SBT"
libraryDependencies += "com.jgcomptech.tools" % "java-ultimate-tools" % "1.5.1"
```

``` tab="Apache Ivy"
<dependency org="com.jgcomptech.tools" name="java-ultimate-tools" rev="1.5.1" />
```

``` tab="Groovy Grape"
@Grapes(
  @Grab(group='com.jgcomptech.tools', module='java-ultimate-tools', version='1.5.1')
)
```
 
## License
[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/)

JavaUltimateTools by J&G CompTech is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

&copy;2018 J&amp;G CompTech

*[JUT]:  Java Ultimate Tools