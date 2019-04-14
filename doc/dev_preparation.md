---
title: 1 Preparation
tags: [development]
keywords: core, plugin, setup, java, maven
last_updated: January 10, 2019
---
The [Communote core](https://github.com/Communote/communote-server) is a Java servlet based web application which can be extended with plugins (OSGi bundles).
This page describes what you need to do to setup your development environment to build the Communote core and Communote plugins.

## 1.1 Setup Java and Maven

For building OpenJDK 8 (tested with AdoptOpenJDK) or Oracle's JDK 8 and a current version of Maven 3 needs to be installed. To test whether the correct JDK is already available you can open a command prompt and run the following command which should output the installed version.

```shell
javac -version
```

If you don't have the JDK you can get OpenJDK 8 from any provider like AdoptOpenJDK or if you prefer Oracle's version got to [Oracle's download page](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). In a similar fashion you can check if Maven is already setup correctly:

```shell
mvn --version
```

This should print the version of the installed Maven and the version of Java Maven is going to use. If Maven is still missing you can follow the instructions at the [Maven homepage](https://maven.apache.org/install.html) to set it up.

## 1.2 Configure Maven

For building plugins against a specific Communote release (without compiling the core of that version) or creating a plugin skeleton with the Communote Maven Archetype you have to add the Communote Maven repository to your [settings.xml](https://maven.apache.org/settings.html):

```xml
<repository>
    <id>bintray-communote-maven</id>
	<name>bintray</name>
    <url>http://dl.bintray.com/communote/maven</url>
	<releases>
        <enabled>true</enabled>
    </releases>
    <snapshots>
        <enabled>false</enabled>
    </snapshots>
</repository>
```

This snipped can be added to the ```repositories``` element of an existing or new ```profile``` element (see [Maven documentation](https://maven.apache.org/settings.html#Profiles) for details).

Since most of Communote's dependencies are available in the Maven central repository you can insert its definition into the same ```repositories``` element right before Communote's repository definition. This can speed up the build because Maven will then check the central repository first. The XML snippet looks like this:

```xml
<repository>
    <id>maven-central</id>
    <name>Maven Central Repository</name>
    <url>http://repo1.maven.org/maven2/</url>
    <releases>
        <enabled>true</enabled>
    </releases>
    <snapshots>
        <enabled>false</enabled>
    </snapshots>
</repository>
```
