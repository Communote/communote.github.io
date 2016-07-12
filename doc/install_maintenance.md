---
title: 3 Maintenance
tags: [installation]
keywords: installation, maintenance
last_updated: March 10, 2016
---

## 3.1 Log files

The location of the log files depends on the chosen installation type. 
In case you installed the Linux or Windows installation package with a bundled Tomcat the Communote logs will be stored in `COMMUNOTE_INSTALL_DIR/communote/logs/communote.log`.

In case you used the installation by WAR file the logs are by default written to `CATALINA_BASE/logs/communote.log` where `CATALINA_BASE` refers to a Tomcat environment variable which defaults to the directory of your Tomcat installation. In some Tomcat distributions the logs directory does not exist and thus, needs to be created.  
If you want to change this default behavior a custom log configuration can be defined by creating a file named `log4j.properties` in Communote's configuration directory (this is the directory which also contains the file startup.properties, see the [installation documentation](install_communote.html#installation-by-war-file) for details). An example of the log4j.properties configuration is shown below.

```properties
# info: lines starting with a hash character like this one are treated as comments

log4j.threshold=INFO
log4j.rootLogger=INFO

# Appender that logs all messages to a file. This definition supports log file rotation when the file reaches a given size.
log4j.appender.std_log=org.apache.log4j.RollingFileAppender
# Assign the absolute path to the file the logger should write to. The path separator has to be a
# slash (even on Windows systems). A colon must be escaped with a backslash, e.g. C:\temp\communote.log
# must be written as C\:/temp/communote.log
log4j.appender.std_log.File=ABSOLUTE_PATH_TO_LOG_FOLDER/communote.log
log4j.appender.std_log.MaxFileSize=5000KB
log4j.appender.std_log.MaxBackupIndex=5
log4j.appender.std_log.layout=org.apache.log4j.PatternLayout
log4j.appender.std_log.layout.ConversionPattern=%d{ISO8601} %p %t %c - %m%n
log4j.appender.std_log.Threshold=INFO

# Appender that logs only error messages to a file.
log4j.appender.error_log=org.apache.log4j.RollingFileAppender
# Assign the absolute path to the file the logger should write to.  See above for further details.
log4j.appender.error_log.File=ABSOLUTE_PATH_TO_LOG_FOLDER/communote-error.log
log4j.appender.error_log.MaxFileSize=5000KB
log4j.appender.error_log.MaxBackupIndex=5
log4j.appender.error_log.layout=org.apache.log4j.PatternLayout
log4j.appender.error_log.layout.ConversionPattern=%d{ISO8601} %p %t %c - %m%n
log4j.appender.error_log.Threshold=ERROR

#Rolling appender for missing message keys
log4j.appender.missingLocalization=org.apache.log4j.RollingFileAppender
# Assign the absolute path to the file the logger should write to.  See above for further details.
log4j.appender.missingLocalization.File=ABSOLUTE_PATH_TO_LOG_FOLDER/communote-missing-localization.log
log4j.appender.missingLocalization.MaxFileSize=5000KB
log4j.appender.missingLocalization.layout=org.apache.log4j.PatternLayout
log4j.appender.missingLocalization.layout.ConversionPattern=%d{ISO8601} - %m%n
log4j.appender.missingLocalization.Threshold=WARN
log4j.logger.missingLocalization.com.communote.server.persistence.common.messages=WARN, missingLocalization

log4j.logger.com.communote=INFO
log4j.logger.com.communote.server.web.commons.filter=ERROR
log4j.logger.de.communardo=INFO, std_log, error_log
log4j.logger.org=WARN, std_log, error_log
log4j.logger.com=WARN, std_log, error_log
log4j.logger.net=WARN, std_log, error_log
```

## 3.2 Starting and stopping Communote

For WAR file based installations just start and stop the Tomcat as usual. On Linux systems you should be careful to run the commands with the correct user account.

When using the Linux installation package you can start and stop Communote with the following commands:

```bash
su PRIVILEGED_USER
# start
COMMUNOTE_INSTALL_DIR/communote/bin/startup.sh
# stop
COMMUNOTE_INSTALL_DIR/communote/bin/shutdown.sh
```

Note: the ``PRIVILEGED_USER`` refers to the dedicated user which was selected during the installation.

When using the Windows installation package you can start and stop Communote with the following commands:

```bash
COMMUNOTE_INSTALL_DIR/communote/bin/startup.bat
COMMUNOTE_INSTALL_DIR/communote/bin/shutdown.bat
```

## 3.3 Updating a Communote installation

To update an existing Communote installation to a new version you can follow the steps below.

**Hint:** in case you are using additional Communote plugins you should ask the creators of the plugins whether they are still compatible with the new version of Communote or test them against the new Communote version on a test environment before updating your production environment.

### 3.3.1 Get the new release

The releases are published at [https://github.com/Communote/communote-server/releases](https://github.com/Communote/communote-server/releases). For updating your installation you always have to download the WAR file even if you are running an installation which was created with the Linux or Windows installation package.

### 3.3.2 Preparation

* Stop Communote (the Tomcat) and ensure that the process was stopped.
* Create a backup of the Communote database and the Communote data directory.
* Read the following update hints carefully and apply any which matches your current installation
  * Update of a Communote which is older than version 2.0
    Before version 2.0 Communote used only URLs which started with '/microblog/'. Starting with version 2.0 there are also URLs which do not start with this prefix. This has to be considered if a web server like the Apache HTTP server is running in front of the Tomcat.
  * Update of a Communote which is older than version 2.1
    Check whether the file `communote/conf/context.xml` in the Communote installation directory contains an `Environment` element whose name attribute contains the value 'communote.config.dir'. If this entry is missing you have to add it:
      1. Edit `communote/conf/context.xml` and insert the following block between the opening and closing `context` element. Modify the value of the value attribute accordingly. The path has to be provided in the notation of your platform (slashes on Linux and backslashes on Windows).

         ```xml
         <Environment name="communote.config.dir" 
             type="java.lang.String" 
             value="absolute path to COMMUNOTE_INSTALL_DIR/communote/conf/communote" />
         ```
      2. Create a file named `startup.properties` in the directory `communote/conf/communote` of your installation and add the following line. The part after the equals sign has to be replaced with the absolute path to the Communote data directory `communote/communote-data` in the installation directory. On windows you have to replace all backslashes with slashes and escape the colon with a backslash as shown in the example:

         ```properties
         communote.data.dir=C\:/CommunoteInstallation/communote/communote-data
         ```

  * Update to version 3.4
    * Ensure that you have an up-to-date Oracle Java Runtime Environment 8 (JRE 8) installed. If not you can download it from [http://www.java.com/en/download/](http://www.java.com/en/download/).
    * Although Tomcat 7 should still work, you should update to a current Tomcat 8.0 (e.g. Tomcat 8.0.36). The instructions for updating the bundled Tomcat of an installation from installation package can be found in the chapter [Update the Tomcat of a Linux or Windows installation package installation](#update-the-tomcat-of-a-linux-or-windows-installation-package-installation).

### 3.3.3 Update Communote

Depending on the type of your installation the update process differs slightly.

* Update a Linux or Windows installation package installation
  * Delete the content of the directory `COMMUNOTE_INSTALL_DIR/communote/communote` where `COMMUNOTE_INSTALL_DIR` refers to your Communote installation directory.
  * Delete the content of the directory `COMMUNOTE_INSTALL_DIR/communote/work`.
  * Delete the content of the directory `COMMUNOTE_INSTALL_DIR/communote/temp`.
  * Extract the downloaded WAR file, which is just a ZIP archive, into the directory `COMMUNOTE_INSTALL_DIR/communote/communote`. On Linux pay attention that the extracted files and directories have the correct owner / permissions.
  * Start Communote.
* Update a WAR file installation
  * Delete the directory `webapps/ROOT` and the file `webapps/ROOT.war` of your Tomcat installation.
  * Rename the downloaded WAR file to 'ROOT.war' and copy it into the `webapps` directory. On Linux make sure that the file can be read by the user who is starting the Tomcat.
  * Delete the content of the `work` directory of your Tomcat installation.
  * Delete the content of the `temp` directory of your Tomcat installation.
  * Start the Tomcat.

## 3.4 Update the Tomcat of a Linux or Windows installation package installation

The Linux and Windows installation packages contain a bundled Apache Tomcat which should be updated from time to time to the current minor version of the supported Tomcat release. This contributes to a secure and stable installation. The minor version refers to the last number in the version string. So for example if Communote supports the Tomcat major version 8.0, the minor versions are 8.0.1, 8.0.36 and so on.

There are two alternatives for updating the Tomcat which are described in the next sections.

### 3.4.1 Update with a new installation

New Communote releases typically bundle an up-to-date version of the supported Tomcat. Therefore, the Tomcat can be updated by doing a new installation as outlined below.

* Stop Communote (the Tomcat) and ensure that the process was stopped.
* Create a backup of the Communote database and the Communote data directory.
* Rename your current Communote installation directory and create a new installation directory with the original name at the same location.
* Download the new installation package for your platform from [GitHub](https://github.com/Communote/communote-server/releases).
* Install it as described in the [installation manual](install_communote.html#installation-with-an-installation-package) to the new installation directory and pay attention to the following hints:
  * You should **not** copy your old context.xml (or customized server.xml) to the new location, especially if the new Tomcat has another major version. Instead we recommend to edit the new files and apply your changes. You should also check your configuration against the Tomcat documentation to ensure it is still correct.
  * If your Communote data directory used to be inside the installation directory or you configured another location copy the data directory to the new location.
* Now you can start Communote and if everything works remove the renamed old installation directory.
 
### 3.4.2 Update with the latest upstream minor version

You can also update the Tomcat to the latest minor version of the supported release by downloading it from the Tomcat website.

* Stop Communote (the Tomcat) and ensure that the process was stopped.
* Download the Tomcat (ZIP or tar.gz file) package for your platform from the [Apache Tomcat website](http://tomcat.apache.org/).
* Rename your current Communote installation directory (`RENAMED_COMMUNOTE_INSTALL_DIR`) and create a new installation directory with the original name at the same location.
* Extract the Tomcat package to the new installation directory.
* Rename the extracted package 'apache-tomcat-<version-number>' to 'communote'.
* Empty the directory `COMMUNOTE_INSTALL_DIR/communote/webapps`
* Copy the JDBC driver from `RENAMED_COMMUNOTE_INSTALL_DIR/communote/lib` to `COMMUNOTE_INSTALL_DIR/communote/lib`.
* Copy the directory `RENAMED_COMMUNOTE_INSTALL_DIR/communote/communote` to `COMMUNOTE_INSTALL_DIR/communote/communote`.
* Copy the directory `RENAMED_COMMUNOTE_INSTALL_DIR/communote/conf/communote` to `COMMUNOTE_INSTALL_DIR/communote/conf/communote`.
* Copy the files setenv.bat and setenv.sh from `RENAMED_COMMUNOTE_INSTALL_DIR/communote/bin/` to  `COMMUNOTE_INSTALL_DIR/communote/bin/`.
* Configure the new installation according to the steps under 'Configuration' of the [installation manual](install_communote.html#installation-with-an-installation-package).
  * You should **not** copy your old context.xml (or customized server.xml) to the new location, especially if the new Tomcat has another major version. Instead we recommend to edit the new files and apply your changes. You should also check your configuration against the Tomcat documentation to ensure it is still correct.
  * If your Communote data directory used to be inside the installation directory or you configured another location copy the data directory to the new location.
* When you are using Linux make sure that the owner and permissions of the files in the new installation directory are set correctly.
* Now you can start Communote and if everything works remove the `RENAMED_COMMUNOTE_INSTALL_DIR`.

