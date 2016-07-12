---
title: 5 Communote Extensions
tags: [installation]
keywords: installation, plugins
last_updated: March 10, 2016
---

Communote can be extended with plugins. This section describes how these extensions can be installed and uninstalled. For an introduction to the development of Communote plugins you can have a look at our [tutorial](dev_how_plugin.html).

## 5.1 Install a Communote plugin

A Communote plugin is a JAR file which has to be copied to the Communote plugin directory to get installed. The Communote plugin directory is usually the subdirectory `plugins` of the Communote data directory defined during the [installation](install_communote.html#installation).

A plugin can be installed at runtime. A restart of Communote is normally not necessary and the new features should be available immediately. After installing a plugin which alters the Communote frontend the user has to refresh the page in the browser for the changes to become effective.

After the installation the plugin will be listed in the Communote administration area under **Extensions > Overview**.

## 5.2 Uninstall a Communote plugin

To uninstall a plugin the JAR file has just to be removed from the plugin directory. A restart of Communote is usually not necessary.

## 5.3 Update a Communote plugin

To update an installed Communote plugin you just have to uninstall it first and install the new JAR file afterwards.
