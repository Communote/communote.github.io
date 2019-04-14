---
title: 1 System requirements
tags: [installation]
keywords: installation, system requirements, java, apache tomcat, windows or linux, database
last_updated: April 14, 2019
---

**Communote has the following software requirements:**

* 32-bit or 64-bit Linux or Windows system
* Oracle's Java 8 runtime environment or OpenJDK 8 (tested with AdoptOpenJDK)
  * note on OpenJDK: you actually only need the Java runtime environment (JRE) for running Communote, but some providers and Linux distibutions only provide the full Java development kit (JDK). Since JDK includes the JRE, taking the full package is OK.
* a database management system. Supported are
  * PostgreSQL 9.5 or 9.6
  * MySQL 5.6
  * Microsoft SQL-Server 2012
  * Oracle 11g
* depending on the installation package: Apache Tomcat server
  * if you choose the WAR file based installation a Tomcat server is required. Although Tomcat 8 might work, we recommend a current Tomat 8.5.x (at least 8.5.32).
* optional: an Apache HTTP server (version 2.2.5 or newer) can be installed in front of the Tomcat server

**The following hardware requirements are recommended:**

* 64bit Dual core system
* 10 – 100 GiB data storage (The actual size depends on the usage of the file upload feature. The installation takes less than 1 GiB.)
* 2 GiB of RAM
