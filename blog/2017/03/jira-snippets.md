+++
date = "2017-03-21"
title = "Jira Administrator Snippets"
math = "true"
categories = "test"
+++

These examples use the Atlassian User Interface (AUI) library that JIRA and other Atlassian applications use for their GUI, written in JavaScript and CSS.

## Announcment Banners

A lot of the time when working on migrations, we need to let the users know about upcoming maintenance and downtime. Generally, it is best to make the message fit in with the rest of the application. This section covers messages that users will see when logged into the system or when they try to log in.

There are a few plugins that offer to show information to users, however, most of the time we can get away with modifying the Announcment Banner available to JIRA Administrators in the System settings.

Note, with both of these message displays, you can cut down on the amount of vertical space used by excluding the 'body' of the message and just using the title.

### Static Message (HTML)

The simplest way to show a message is to use HTML. The annoucement banner will not be closeable, but simplifies the set up.

See the screenshot below for the result:

![Static Message](/images/blog/2017-03-21/pic1.png)

You can customise the color of the box and the symbol shown by changing the 'aui-message-error' part on the first line:

* Error Message - Red Background and Stop Symbol - Use 'aui-message-error'
* Warning Message - Yellow Background and Warning Symbol - Use 'aui-message-warning'
* Informational Message - Blue Background and (i) Symbol - Use 'aui-message-info'

Enter the code snippet below in the Annoucement Banner field to add the above message, with an error message. Note how we add the 'style' snippet to prevent the uneven padding that JIRA adds automatically.

~~~~
<div class="aui-message aui-message-error">
    <p class="title"><strong>Error!</strong></p>
    <p>And this is just content in a Default message.</p>
    </div>
    <style>
        #announcement-banner { padding-bottom: 0px; }
        </style>
        ~~~~

### Closeable Message (JavaScript)

You may also want to allow users to close the message, as it can get in the way of work. We can do this by creating the message with the JavaScript API of AUI, AJS.

This will add a 'close' button on the right. It will not save the state between page refreshes, but helps on a small screen, or when presenting (e.g. standups).

![JS Message](/images/blog/2017-03-21/pic2.png)

![JS Message](/images/blog/2017-03-21/pic3.png)

![JS Message](/images/blog/2017-03-21/pic4.png)

This can be created by adding the following code into the Annoucement Banner section, replacing the 'generic' with 'error', 'warning' if you want a yellow or red border respectively:

~~~~
<div id="aui-message-bar"></div>
<script type="text/javascript">
    AJS.messages.generic({
        title: 'Informational Message.',
        body: '<p> Some information for users...</p>'
        });
        </script>
        <style>
            #announcement-banner { padding-bottom: 0px; }
            </style>
            ~~~~

## Tweak the UI with CSS/Javascript

### Replace the "Forgotten Password" Links

Your organisation may use JIRA with Active Directory for authentication and may be one of the majority that do not allow JIRA to edit the User Directory. In these cases, the 'Reset password' and 'Can't remember your login' links are not very useful.

You can change the links to direct users to your own portal using the following snippet:

~~~~
<script type="text/javascript">
    var intervalID = window.setInterval(myCallback, 100);
    function myCallback() {
        AJS.$("#login-form-cancel").attr("href","https://passwordreset.yourcompany.com");
        AJS.$("#forgotpassword").html("Your Company Password Reset");
        AJS.$("#login-form-cancel").html("Your Company Password Reset");
        AJS.$("#forgotpassword").attr("href", "https://passwordreset.yourcompany.com");
        }
        </script>
        ~~~~

This will change the links so they work and appear as follows:

![Login Message](/images/blog/2017-03-21/pic5.png)