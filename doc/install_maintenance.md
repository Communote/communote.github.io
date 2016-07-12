---
title: 3 Maintenance
tags: [installation]
keywords: installation, maintenance
last_updated: March 10, 2016
---

## 3.1 Log files

The location of the log files depends on the chosen installation type. 
In case you installed the Linux or Windows installation package with bundled Tomcat the Communote logs will be stored in
`COMMUNOTE_INSTALL_DIR/communote/logs/communote.log`.

In case you used the installation by WAR file the logs are by default written to `CATALINA_BASE/logs/communote.log` where `CATALINA_BASE` refers to a Tomcat environment variable which defaults to the directory of your Tomcat installation. In some Tomcat distributions the logs directory does not exist and thus, needs to be created. If you want to change this default behavior a custom log configuration can be defined by creating a file named `log4j.properties` in Communote's configuration directory (this is the directory which also contains the file startup.properties, see the [installation documentation](install_communote.html#installation-by-war-file) for details). An example of the log4j.properties configuration is shown below.

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

```batch
COMMUNOTE_INSTALL_DIR/communote/bin/startup.bat
COMMUNOTE_INSTALL_DIR/communote/bin/shutdown.bat
```
