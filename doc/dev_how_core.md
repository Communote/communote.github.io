---
title: 2.3 Build the Core
tags: [development]
keywords: core, build, git clone
last_updated: March 10, 2016
---
The Communote core is a Java servlet based web application with an embedded OSGi container which is available in the [Communote-server](https://github.com/Communote/communote-server) repository on GitHub. It consists of the core components and a set of OSGi plugins, the so called 'core plugins', which are bundled in the WAR file of an official Communote release.  
Communote uses [Apache Maven](https://maven.apache.org) as build automation tool. To build the Communote core from source just follow the steps outlined in the next paragraphs.

## 2.3.1 Get the source code
To get the source code you can use Git's clone mechanism or download it as a zip archive directly from [GitHub](https://github.com/Communote/communote-server). The download can be triggered in the 'Clone or download' dropdown. For cloning you can use the following snippet:

```
git clone https://github.com/Communote/communote-server.git
```

## 2.3.2 Build
The Communote server repository contains the two directories ```build``` and ```communote```. The former contains Maven modules to build the final WAR file and the latter contains all modules to create the artifacts which should be assembled to the final WAR file.

For building you have to setup your development environment as described in the [preparations section](dev_preparation.html). As soon as these requirements are met you can change into the directory ```communote-server/communote``` and execute the following commands:

```
mvn -Ppre
mvn -Pmda
```

Few minutes later you will hopefully see a "Success" message on your screen. Now you can go to the directory ```communote-server/build/war-standalone```and call ```mvn``` again. This will create the final WAR file in the ```target``` subdirectory. This WAR file can be deployed into your prepared Tomcat server as described in our [installation documentation](http://communote.github.io/doc/install_communote.html#by-deploying-war-file).

Calling Maven with the profiles 'pre' and 'mda' is usually only necessary in the first build or after pulling changes from GitHub. To rebuild your local changes you typically just have to invoke ```mvn``` in the directory ```communote-server/communote``` and recreate the final WAR file afterwards.
