+++
date = "2016-08-07"
title = "Monitoring Atlassian Server and Data Center Applications"
math = "true"
categories = "test"
+++

There are several methods to monitor your company's Atlassian applications, ranging from simple techniques such as HTTP Checks on the instance to more complex monitoring solutions which involve the [Java Management Extensions (JMX)](https://en.wikipedia.org/wiki/Java_Management_Extensions) to show metrics from within the Java Virtual Machine (JVM).

This article aims to describe the different methods and the possible applications that can be used to show historic data and see useful patterns.

### Introduction

As Atlassian applications (such as JIRA, Confluence, Bitbucket, Bamboo) are built using Java EE and run in a Java Application Server, they have certain metrics that are important to monitor, such as the Java Heap usage statistics (Java uses a fixed size heap with [Garbage Collection](https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/geninfo/diagnos/garbage_collect.html)), statistics from the other heaps (Permgen, Eden etc.) and the state of Managed Beans (MBeans) which are a feature of Java which allows the exposing of certain variables, such as the Index status of JIRA or Confluence.

Other metrics and controls can be read from the state of the Tomcat, such as the amount of HTTP Sessions, HTTP Errors and amount of threads that are available.

The main methods of monitoring are as follows:

* Make use of the Status Page which is used internally by the applications.
* Use the JavaMelody plugin to provide in-app monitoring of many internal application components (if supported).
* Use a JMX Client on the server or a client machine to monitor the application.
* Use an Agent-based JMX Client such as Zabbix, AppDynamics or Solarwinds.

Below is a breakdown of each option and how each could work:

### Detailed Explanation

#### 1. Use HTTP Requests

* There is a status page available on all Atlassian applications which returns a simple string `RUNNING` to confirm whether they are running.
* This page is used internally for the Data Center line of products that is available for some of the applications - the page does not use many resources to return, so you can safely poll the page as frequent as necessary without impacting the application performance.
You can use this page with your own monitoring solution, at the following address:
~~~~
https://JIRA-BASE-URL/status
https://jira.atlassian.com/status (example)
~~~~
* If your application is available externally (ie. on the internet), you can set up 2 checks - letting you know if an issue is affecting one or both:
* 1 for the internal address
* 1 for for the internet-accessible address

#### 2. Use the JavaMelody Plugin

There is an Open Source montoring solution called JavaMelody which is available in the form of a JIRA, Confluence and Bamboo plugin.

Unfortunately, a monitoring system which runs within the application is only so useful, as if the application is impacted or unavailable, the monitoring system is also not available and you will not have the historic data for the time period during the outage.

![JavaMelody](/images/blog/2016-08-07/pic1.png)

If you want to install JavaMelody, the download link and information on how to install it is available on their [homepage](https://github.com/javamelody/javamelody/wiki).

Be aware that you cannot install this through the UPM and it requires a restart, and additional work is required to get SQL/Database statistics to show up in the graphs (you must move from jdbc to jdni datasource).

#### 3. Use a JMX Client
> ...such as JConsole, VisualVM or Hyperic.

JConsole is a client for the JMX protocol and allows you to look into the internals of a Java Application, making it possible to monitor and show the historic data for various Java metrics and also metrics that have been exposed through MBeans.

![JavaMelody](/images/blog/2016-08-07/pic2.gif)

#### 4. Use a Monitoring Agent to capture the JMX Metrics

Many (mainly commercial) monitoring solutions supply 'agents' which are small programs which run on machines and collect monitoring data to send back to a centralised server.

![JavaMelody](/images/blog/2016-08-07/pic3.png)

### Conclusion

* Using a Monitoring Agent is the best for the long-term monitoring of applications, as the agents are available on many leading monitoring solutions and can be combined with other metrics. Zabbix supports both HTTP checks and JMX monitoring and lets you configure alarms. It's open source too. Combining the status checks gives you more data to use in analysing performance problems.
* Using a JMX Client is very useful when on-site and dealing with performance issues, as it gives you a real-time view of the internals of the application to see where the issues are - e.g. the memory may need increasing or the CPU may be overloaded or one of the other heaps may be overused.
* Using the JavaMelody Plugin is very useful as a System Administrator, as it is very easy to install and provides you with better monitoring than the built-in 1/2 pages.