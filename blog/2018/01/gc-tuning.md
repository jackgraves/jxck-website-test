+++
date = "2018-01-18"
title = "Basic Garbage Collection Tuning in Atlassian Applications"
math = "true"
categories = "test"
+++

One important factor with any Java-based Web Application is the Heap, which is a by-product of Java being a Garbage Collected (GC) language.

In Java, you do not need to declare memory space as unused, as an automatic system will automatically evacuate unneeded data from memory periodically when more memory space is required – this is called a ‘garbage collection’ and is a computationally complex operation that can slow down (or pause) the system when it is initiated – there are soft and hard garbage collections which affect the system in a different way:

* **Partial GC** – Process which has minimal impact on runtime and does not cause a large pause.
* **Full GC** – More complex process which causes a longer pause.

A GC will be initiated when the Heap Utilisation reaches it’s maximum amount and must free up memory to allow more work to take place. Theoretically, if you had an infinite memory computer, you would never need to do a Garbage Collection, so adding to the memory is a good way to reduce the amount of Garbage Collections that take place, but there are other ways we can change its behaviour:

* Changing the GC Type
* Serial Garbage Collector
* Parallel Garbage Collector
* CMS Garbage Collector
* G1 Garbage Collector
* Increasing the Maximum Heap Size.
* Setting the Minimum and Maximum Heap Size to the same value to prevent dynamic resizing.

#### Monitoring

The only way we can effectively determine the correct options is by profiling the system during normal usage, using a monitoring tool which will record how long each Garbage Collection took, how much CPU it used in doing so and how long the pause was to the other threads. Using an APM tool like AppDynamics allows you to correlate other metrics (like active sessions, http requests or even particular users' requests).

Below is the AppDynamics interface, showing the amount of heap in use, how long each GC took and can show this over days, weeks, months or years and you can also compare different periods with one another, which helps when comparing different GC options if you change it.

![](/images/blog/2018-01-18/pic1.png)

Having an effective Load Testing suite is another useful tool to use in combination with Heap Optimisation, as you can experiment with different methods/settings in a separate environment and monitor with an APM solution. You can create your own Dashboards which have traffic light status symbols which respond to rules you configure.

#### Connecting to JMX using a Client

If you need to troubleshoot a performance issue separately from a monitoring suite, you can also use JMX client and a JMX connection to view and record this information on our own machine using a JMX client like [JConsole](https://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html), [VisualVM](https://visualvm.github.io/) and [Java Mission Control](http://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) (JMC), all of which are included in the Oracle [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) distribution (in the /bin folder). This is useful when you are performing a load test and you want to record the performance profile, as you can use Java Mission Control to record with it's Flight Recorder function.

You must make sure you have enabled the JMX function on the Tomcat server using [these instructions](https://tomcat.apache.org/tomcat-7.0-doc/monitoring.html).

![](/images/blog/2018-01-18/pic2.gif)

If you don’t want to use a Client, you could install the [JavaMelody plugin](https://marketplace.atlassian.com/plugins/net.bull.javamelody/cloud/overview), which uses JMX extensions within the Application Server container, but this is discouraged as it places more load on your system and is not resilient to outages.

#### Setting Goals for Garbage Collection

Use the following syntax to set the Garbage Collection options for [G1GC](http://www.oracle.com/technetwork/articles/java/g1gc-1984535.html):

| Command Line Option                     | Explanation                      |
|-----------------------------------------|----------------------------------|
| `-XX:+UseG1GC`                          | Use G1GC Algorithm               |
| `-XX:MaxGCPauseMillis=200`              | Aim for a Maximum Pause of 200ms |
| `-XX:ParallelGCThreads=20`              | Use 20 parallel threads for GC   |
| `-XX:ConcGCThreads=6`                   | Use 6 concurrent threads for GC  |
| `-XX:InitiatingHeapOccupancyPercent=80` | Start a GC at 80% Heap Usage     |

To set these, the process is the same for all Atlassian applications - for example - when running Bamboo as a service, you will need to run the following command to set the options and then put the commands in the ‘Java’ tab which shows up after running these in a 'command prompt':

~~~~
cd <bamboo-install>/bin
    tomcat8w //ES//<bamboo-service-name>
        ~~~~

If running in a console, these can be run as command line arguments in the set-env.sh or set-env.bat scripts in the `<bamboo-install>/bin` directory.

Remember, you should set the Heap Size appropriately before looking at changing your GC algorithm. If you are seeing a large amount of GC events, this usually means the memory requirement of the application is higher than the available resources.

||Command Line Option | Explanation       |
|---------------------|-------------------|
| `-Xms`              | Minimum Heap Size |
| `-Xmx`              | Maximum Heap Size |

I recommend setting these to the same size, so that dynamic resizing does not impact the application performance.

It is not possible to provide an one-size-fits-all set of options and you must test the options, monitoring the effect it has on GC times and make your own conclusions as to the optimal values.