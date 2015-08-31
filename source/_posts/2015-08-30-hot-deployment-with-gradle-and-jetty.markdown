---
layout: post
title: "Hot deployment with Gradle and Jetty"
date: 2015-08-30 21:35:05 +0100
comments: true
categories: [gradle, jetty, java, deployment]
---

In web development it's very handy to see updates to static content immediately, without a server restart.

In this blog post I'll write how to configure Gradle to support hot deployment with Jetty web server.

<!--more-->

First of all we need to create the following directory structure for the project:


```
├── build.gradle
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   └── webapp
    │       ├── WEB-INF
    │       │   └── web.xml
    │       └── ws # static content goes here
    │           └── index.html
    └── test
```

Here is the content of build.gradle:

```groovy

group 'my-project'
version '0.0'

// Plugin to support Java code compilation
apply plugin: 'java'
// Plugin to support assembling application into a war
apply plugin: 'war'
// Gretty plugin for hot deployment
apply from: 'https://raw.github.com/akhikhl/gretty/master/pluginScripts/gretty.plugin'

sourceCompatibility = 1.8

repositories {
    mavenCentral()
}
```

Here is a content of settings.gradle:

```groovy
rootProject.name = 'my-project'
```

Now we need to configure web.xml how to serve static content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">

    <!-- "default" servlet is used for serving static content -->
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>*.html</url-pattern>
    </servlet-mapping>
    <!-- We can add other file types like *.js or *.css if necessary -->

    <!-- When our application is opened in a browser ws/index.html will be displayed -->
    <welcome-file-list>
        <welcome-file>ws/index.html</welcome-file>
    </welcome-file-list>
</web-app>
```

Now we can deploy our application in Jetty with a single Gradle command:

```sh
# "appRun" goal was added by Gretty plugin
gradle appRun
```

When you run this command you should see an output like:

```
...
:appRun
21:53:07 INFO  Jetty 9.2.10.v20150310 started and listening on port 8080
21:53:07 INFO  jersey sample runs at:
21:53:07 INFO    http://localhost:8080/my-project
Press any key to stop the server.
```

The web site should be available via the following url: http://localhost:8080/open-board

If index.html is updated new changes would be reflected if you refresh a page in a browser. No server restart is needed.
