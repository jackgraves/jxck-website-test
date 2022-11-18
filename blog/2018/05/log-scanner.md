+++
date = "2018-05-13"
title = "Open Source Atlassian Log Scanner Released"
math = "true"
categories = "test"
+++

The built-in Log Scanner that comes with Atlassian applications (Hercules), is a useful tool for many administrators, as it allows you to check your current log file against a list of known issues with the Atlassian Knowledge Base. This can help find the most common problems.

This is not an alternative to manually looking at the log file, but can help turn up issues more quickly - especially if it is automated as part of the support cycle, as the scanning can be performed unattended.

The way the built-in log scanner works is by downloading a list of regular expressions from a URL hosted on the main Confluence site.

The problem is, the built-in log analyser is slow and it is not portable.

Here is a comparison on a small log file, 3MB in size (usually they are 20MB):

*   **Built-in Log Analyser:** 6 minutes 14 seconds
*   **Standalone Log Scanner:** 1 minute 48 seconds

This is a large improvement on the built-in log analyser and I hope to improve the performance of the tool by improving the loop construct and implementing multi-threading and eventually plan to release an Atlassian App (plugin) which will execute the log analysis when an atlassian log file is uploaded and post the results to the parent issue.

I have open-sourced the application and welcome contributions:

[https://github.com/jackgraves/standalone-atlassian-log-scanner](https://github.com/jackgraves/standalone-atlassian-log-scanner)

Instructions for compiling and executing the code is included in the readme.