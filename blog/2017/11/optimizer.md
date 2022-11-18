+++
date = "2017-11-23"
title = "Improving the Cleaner for Jira with v2"
math = "true"
categories = "test"
+++

I released the Cleaner for Jira plugin/App back in June 2017 and the first version (1.0) solved a number of problems that kept coming up with our customers systems, such as:

*   Large numbers of schemes and other configuration items which are no longer required, making administration difficult and making pages in the administration section load very slow
*   Hundreds or thousands of Custom Fields which cause a massive performance impact across indexing, viewing, editing and creating issues
*   Numerous old or obsolete Projects which contribute to increased indexing time
*   Hard to see at-a-glance statistics on users systems and export to Excel/CSV – especially useful during inventory tasks which we perform on clients’ systems in order to quote realistically

However, the App had some downsides in its implementation which I meant to fix:

*   Scanning – The App had to perform a ‘scan’ of the Jira system before the tables were populated and a scan had to be performed after an action (such as a deletion) had occurred – which resulted in a slow turnaround time for administrators trying to use the App.
*   Database Traffic – During the ‘scan’ the App generated large amounts of database traffic, as it created its own model of the Jira Data Model in its own database tables (with constraints and primary keys
*   Activity Tracking – There were several features intended to be useful for tracking the state of a system, but they had several limitations – there was a history tab which tracked the amount of configuration items, users, projects and custom fields over time, however, it did not run at a constant time and instead ran after each scan (this meant that in a day you could remove your previous items)
*   Interface – The interface needed improvement and help needed to be added for some of the advanced features

Because of the reasons listed above I started to develop version 2.0 of the App, which was intended to rectify all these issues and turned out as a complete re-write of the App (quite a regular occurrence) and this resulted in some of the constraints used in the first version to be removed after a period of testing and scalability analysis.

This article takes is an in-depth investigation into the changes made – including the reasoning behind them and the challenges faced.

**Removing the scan requirement**

The most requested feature was to remove the requirement to scan the system before viewing items and performing deletions. As the most complicated part of the App, the scan built up a model of the system in the App’s own database and then used this model to render the tables and provide suggestions.

When version 2 development started, I was eager to remove the scanning mechanism and instead perform the analysis on-demand (asynchronously) – something which I was not sure would be possible in a timely manner, as the systems we generally run the App on extremely large Jira systems (most with over 1m issues and over 10,000 users) and also of concern is the memory footprint, as if the analysis data is not stored in the database, it must be stored temporarily in memory, which is much more finite than the database storage, but is much faster.

Therefore, the following constraints had to be met with the new implementation:

*   Memory (Heap) Usage (< 1GB)
*   Time Taken for Scan (< 60s)

I started with a dependency map of the configuration items, to determine what needs to be scanned in what order for each of the pages, for example, in order to show the ‘Screen Scheme’ page, with it’s column denoting the amount of Issue Type Screen Schemes that a scheme is attached to, both the Issue Type Screen Scheme and the Screen Scheme data needs to be analysed. This led me to modularise the ‘scanning’ components into ‘Generator’ classes and then use Dependency Injection to inject the classes as required and run any dependencies first.

To fit within the memory requirement, several of the ‘generator’ classes had to be modified to work in a low footprint mode and these were:

1.  Inactive User functionality - systems of a large size will generally have more objects of this kind than any other item that Cleaner for Jira scans
2.  Auto-Context Custom Fields functionality – the number of queries that are required for the automatic context feature is large – for each Project and each Issue Type applicable in the Project, a lucene query must be executed, resulting in a lot of processing - e.g. 100 projects * 10 issue types average per Project = approx. 1000 queries

For each one, a different approach was taken:

*   Inactive User functionality

1.  Iterate over the different characters (letters) that are possible in the username field
2.  Search iteratively over each character and each user that contains the character
3.  Only add users who fit the criteria (of inactivity) to the array which is returned to frontend

*   Auto-Context Custom Fields functionality
1.  Iterate over the Custom Fields and through the required lucene queries
2.  Add applicable Custom Fields and their ideal context to a database table
3.  Front-end will paginate and load asynchronously from the database table

**Improving the Interface**

The old interface of the Cleaner left a lot to be desired and a more consistent design language was needed for the App. Look at the screenshot below for the original navigation and design language used:

![](/images/blog/2017-11-23/pic1.png)

As you can see there are several problems with this setup:

*   You cannot see all the Configuration Items that the App supports from any screen
*   When scrolling down a large list, the ‘Delete’ or ‘Disable’ button is only shown at the top
*   While there is some text at the top of the screen, it is not very helpful and results in all the pages having a different offset for the table
*   The ‘Scan’ button and the date of the last scan is not prominent enough and resulted in users not scanning the system when it should be (before any action)

The new interface brings some of the Atlassian Design Language and some of a new structure being used:

![](/images/blog/2017-11-23/pic2.png)

The differences between this and the previous version are as follows:

*   All Features of the Cleaner are shown on the left-hand side of the screen, in a sidebar under a new top-level menu item ‘Cleaner’. This uses the standard Jira navigation structure and this also means any page can be returned by pressing ‘.’ (dot) on the keyboard and typing the first few letters of the page name from any location in Jira
*   There is no confusing ‘Scan’ button or date of last scan, as it will be when the page was last refreshed.
*   There is a help panel at the top, with an image, a brief overview of what item you are viewing and some helpful tips and tricks. This panel can be hidden/shown from the top-right action panel as per the users preference and their setting is stored in a Cookie.
*   The ‘Delete’, ‘Export to CSV’ and ‘Auto-Suggest’ buttons – all related to the current table, have been moved to the bottom of the screen, in a floating panel which follows the page down – so no more frantic scrolling!

**Implementing Activity Tracking and Audit History**

The previous version had a basic ‘History’ graph, but it was not on a consistent schedule and didn’t result in very useful graphs. The new version has a scheduler built-in which runs at the same time each night and populates a table which can then be used to graph the change in the Jira system. This has resulted in the graphs being much more insightful.

See the example below:

![](/images/blog/2017-11-23/pic3.png)

The above graph shows the last 2 weeks of changes in the amount of ‘licensed users’ on the system – we can see that on 2018-02-02, the User count increased by quite a lot. The other items, shown crossed out above the graph can be toggled to show different items change over time, or see them together.

The other essential feature that the App lacked was an Audit Log, which is needed to determine which administrator performed actions using the Cleaner for Jira. To simplify this, the ‘progress dialog’ which is shown when performing a bulk action, uses the Audit Log in the background so that both screens are consistent.

![](/images/blog/2017-11-23/pic4.png)

This helps determine if a problem has occurred (such as an error) during the execution, or whether a user performed an incorrect action on the system.

I hope you have enjoyed this retrospective on the previous version of Cleaner for Jira and how it has been improved in the second version. There are still improvements being made to the App and here are a few of the features we looking at implementing for the next version:

*   Duplicate Analysis of objects to determine when they can be merged (e.g. 2 identical Workflow Schemes) and if possible, automatic merging of the objects
*   Unused/Obsolete Agile Board detection and deletion
*   Unused/Obsolete Filter detection and deletion
*   Custom Field Merging (duplicate analysis, conflict resolution and automatic merging