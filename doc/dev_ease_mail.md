---
title: 3.1 Mail in Development Environment
tags: [development]
keywords: core, mail, smtp
last_updated: January 12, 2019
---
Communote sends mails in different situations like when new users are invited or notifications are sent. Therefore, an
SMTP server needs to be configured even in a development environment. You could install a local SMTP server but
this might be to cumbersome for developing a plugin.

Starting with Communote version 3.5 there is an alternative which allows storing the mails in the filesystem or using a
catch-all address (e.g. from a public ESP) for all outgoing mails. To configure one of these variants you'll have to
create a Java properties file named `development.properties` in the Communote configuration directory. This is the same
directory which also contains the `startup.properties` file. In this file the property `mailout.mode` defines how mails
should be sent. The following modes are supported:
- `FILESYSTEM`: don't send any mails but store them as files in the subdirectory `DevMimeMessageStore` of Communote's
data directory
- `CATCH_ALL`: send all mails via SMTP to a configurable catch-all address instead of the destined recipient. The
catch-all address can be configured in the same file with the property `mailout.catchall.address`. The original recipient
will be stored in one of the mail headers `Communote-Original-To`, `Communote-Original-Bcc` or `Communote-Original-Cc`
depending on whether it was found in the To, Bcc or Cc field.
- `SMTP`: send all mails via SMTP. This behaves exactly as if the `development.properties` doesn't exist or is empty.
- Any other value is ignored and `SMTP` is assumed.

As an example the `development.properties` for redirecting all mails to communote_dev@localhost would look like this:

```properties
mailout.mode = CATCH_ALL
mailout.catchall.address = communote_dev@localhost
```


After creating or modifying the `development.properties` file, Communote has to be restarted.