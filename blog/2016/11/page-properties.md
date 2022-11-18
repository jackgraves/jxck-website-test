+++
date = "2016-11-16"
title = "Using the Page Properties Macro in Confluence"
math = "true"
categories = "test"
+++

Atlassian Confluence, the wiki web application is used to share and collaborate within companies and is used for a variety of content.

Confluence pages generally contain text and other content such as pictures, but they also allow you to embed enhanced functionality with macros.

It comes with a large number of macros out-of-the-box, but with a lot of macros available out-of-the-box, as well as a number that come with other add-ons, it can be hard to find the ones that can help the most with your particular use-case.

The macro being covered in this article should apply to almost all Confluence users and could be used for the following content:

* If you store information on particular clients on their own pages, you can use this technique to collate their details into a table which can then be used for easy comparison.
* If you store metadata on server and other infrastructure, using this technique can provide you with an easy way to store different, varied information on each configuration (with diagrams, images etc) and still produce a table of this information and order it by IP Address for example.
* A common scenario is having a parent page, such as 'Cars', which doesn't contain any information and is just used to contain other pages (children). You may set up the 'Children Macro' to show a basic Table of Contents, however, you can make much better use of the page and turn it into a talking-point by using this technique.

The screenshots below show a basic example of the functionality that can be achieved, with a basic example involving a Garage in which cars are bought and sold.

First, lets take a look at the simple page tree for some context:

![Pic 1](/images/blog/2016-11-16/pic1.png)

A page showing one of the cars in the garage, with some information (photos in this case, but could be any data) that we don't want to show on the report:

![Pic 2](/images/blog/2016-11-16/pic2.png)

The Current Cars page shows the Page Propeties Report macro showing the data from all the tables within 'Page Properties' macros on pages which have the label 'car' and 'current':

![Pic 3](/images/blog/2016-11-16/pic3.png)

The Garage page is the top-level page which contains 2 titles with the 'Include Page' macro used to show the contents of the Current Cars and the Sold Cars pages to save time:

![Pic 4](/images/blog/2016-11-16/pic4.png)

As you can see from the above example, it can turn pages being used to create a hierarchy into useful comparison pages which can help contextualise your Confluence Spaces.

1.  First you should create the hierarchy that your child pages will sit under.
_e.g. Servers -> UKLON_01929_
2.  Add the **Page Properties** macro to the child page. When adding the macro, you can choose whether the table will be hidden.
_e.g. add it to UKLON_01929 in a section called Metadata_
3.  Now, you will have an empty **Page Properties** macro on the page which allows you to insert data. Next, insert a table into it and change the shaded header row so that it is shaded on the left-most column instead of the standard top-row
4.  Enter the labels of the data you wish to compare. You can include Emoticons, such as (/) for a tick and (x) for a cross for easy comparison
_e.g. Name, IP Address, VLAN, Status (you could even get a [macro](https://marketplace.atlassian.com/plugins/aptis.plugins.serverStatus/server/overview) which checks their status)_
5.  Enter the values for these labels.
_e.g. IP Address: 192.16.1.1_
6.  Add a common Confluence label to the page that you will use on all pages which have comparable information.
_e.g. server-active or server-inactive_
7.  Repeat steps 2-6 on each page which has the same type of data.
**TIP: Copy the entire Page Properties macro from the first page and just edit the values.**
8.  Now we can report! On the parent page, add the **Page Properties Report** macro and choose to restrict it to the label you placed on the child pages which contain the macro.
_e.g. server-active_
9.  Click on **Show Options** and enter the columns you would like to show in the 'Columns to Show' option as a comma separated list.
**TIP: You'll also need to use this option if you want to order the columns.**
10.  You should now have a dynamic report that you can use to highlight information from other linked pages, compare their values and reduce maintenance by only needing to update a single page and having all reports update automatically.

See some of the instructions graphically below:

[Steps 2 and 3] The Child Page in 'Edit Mode' showing the table nested inside the macro:

![Pic 5](/images/blog/2016-11-16/pic5.png)

[Step 6] The shared label that is used to select the contents of the report:

![Pic 6](/images/blog/2016-11-16/pic6.png)

[Step 8] The parent page in 'Edit Mode' showing the macro inside a section:

![Pic 7](/images/blog/2016-11-16/pic7.png)

[Step 9] The parent page's report macro settings showing the label selector:

![Pic 8](/images/blog/2016-11-16/pic8.png)

[Step 9] The 'Columns to Show' field filled in to give the columns the right order (or to only show certain values)

![Pic 9](/images/blog/2016-11-16/pic9.png)

The documentation on the macros can be found on the Atlassian website if you need to use it for reference:

*   [Page Properties Macro Documentation](https://confluence.atlassian.com/doc/page-properties-macro-184550024.html)
*   [Page Properties Report Macro Documentation](https://confluence.atlassian.com/doc/page-properties-report-macro-186089616.html)

Hopefully this post has explained how to use the Page Properties and Page Properties Report macros in your own Confluence installation. Your use-case may be different, but it is extremely versatile and should be adaptable to your needs.