---
title: 1.4 Build the core
tags: [development]
keywords: core, build, git clone
last_updated: March 10, 2016
---
The Communote core is Java servlet based web application with an embedded OSGi container which is available in the [Communote-server](https://github.com/Communote/communote-server) repository on GitHub. It consists of the core components and a set of OSGi plugins, the so called 'core plugins', which are bundled in the WAR file of an official Communote release.  
Communote uses [Apache Maven](https://maven.apache.org) as build automation tool. To build the Communote core from source just follow the steps outlined in the next paragraphs.

## 1.4.1 Get the source code
To get the source code you can use Git's clone mechanism or download it as a zip archive directly from [GitHub](https://github.com/Communote/communote-server). The download can be triggered in the 'Clone or download' dropdown. For cloning you can use the following snippet:

```
git clone https://github.com/Communote/communote-server.git
```

## 1.4.2 Build
The Communote server repository contains the two directories ```build``` and ```communote```. The former contains Maven modules to build the final WAR file and the latter contains all modules to create the artifacts which should be assembled to the final WAR file.

For building a current version of Maven 3 needs to be installed. If you don't have it yet follow the instructions at the [Maven homepage](https://maven.apache.org/install.html) to set it up. After this you can change into the ```communote``` directory and execute the following commands:
```
mvn -Ppre
mvn -Pmda
```
Few minutes later you will hopefully see a "Success" message on your screen. Now you can go to the directory ```build/war-standalone```and call ```mvn``` again. This will create the final WAR file in the ```target``` subdirectory. This WAR file can be deployed into your prepared Tomcat server as described in our [installation documentation](http://communote.github.io/doc/install_communote.html#by-deploying-war-file).

Calling Maven with the profiles 'pre' and 'mda' is usually only necessary in the first build or after pulling changes from GitHub. To rebuild your local changes you typically just have to invoke ```mvn```.
