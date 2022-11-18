+++
date = "2017-02-07"
title = "Setting up a Blog based on KeystoneJS"
math = "true"
categories = "test"
+++

> Note that this blog post is out of date and doesn't refer to this version of the website.

In this article, I run through the steps to create a Blog based on [KeystoneJS](http://keystonejs.com/).

## Installing KeystoneJS (Generator)

Assuming that you already have [Node.js](https://nodejs.org/en/) installed, generating a template project is as simple as installing the Keystone Generator:

~~~~
npm install -g generator-keystone
yo keystone
~~~~

This will guide you through several questions to choose from e.g. what templating engine you want to use.

You can then open up the Project folder with your favourite IDE - I'm using [JetBrains WebStorm](https://www.jetbrains.com/webstorm/), which is very similar to their primary product [IntelliJ IDEA](https://www.jetbrains.com/idea/?fromMenu) - the editor that I'm most familiar with.

![IntelliJ](/images/blog/2017-02-07/pic1.png)

## Heroku Set Up

You can host your Keystone on your own infrastructure, on your local machine, or with a Cloud hosting provider. I'm using [Heroku](http://www.heroku.com), which costs $7 per month and takes care of a lot of the maintenance and work that goes into running your own server. [AWS](https://aws.amazon.com/) is another solid choice and you can run it in a container using something like ElasticBeanstalk or EC2.

![IntelliJ](/images/blog/2017-02-07/pic2.png)

If you are using Heroku, the following steps should be taken:

1. Set up a [new account](https://signup.heroku.com/dc) if you haven't already
2. Install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) onto your machine
3. Open a Command Prompt and navigate to the directory where your code is stored (the Project Folder)

* Press Windows Key + R
* Run `cmd`
* Type `cd C:/Users/Neil/Desktop/keystone_test`

4. Log into the Heroku CLI

* Run heroku login
* Type in your account details

5. Set up your Project Folder on your local machine to be a Git repository

~~~~
git init
git add .
git commit . --message "First Commit"
~~~~

6. Set up a new Dyno (a server node) in Heroku

~~~~
heroku apps:create keystone_test
~~~~

7. Push the code to the Remote

~~~~
git push heroku master
~~~~

8.  You can now perform the following through your web browser:

* View the [Heroku Logs](https://dashboard.heroku.com/apps/)
* View the Website

However, the site won't work yet, as we need to set up the other requirements for KeystoneJS, which is covered in the next few sections.

### MongoDB Set Up

Next, we need to set up the database that the application is going to use. For this we can use the [mLab MongoDB Service](https://mlab.com/), which similarly to Heroku Apps, also has a Free tier which can be used for sandbox purposes.

Steps:

1.  Navigate to the [Heroku Console](https://dashboard.heroku.com/apps/)
2.  Click on the Application you set up in the previous section
3.  In the 'Add-ons' section, click on 'Install Add-on'
4.  Search for mLab and install the Add-on onto the application.

*   This will add a new Environmental Variable to your application with the address and credentials to log into the database*

6.  You have now set up a MongoDB Database for the application. Once we have completed the last requirements we will be able to start up the application.

*I used [MongoChef](http://3t.io/mongochef/) to log into the database to get a view of what was going on under the hood while using the application. You can copy the Environmental Variable for the MongoDB URL in Heroku (MONGODB_URI) straight into the 'From URL' panel in MongoChef and log into it using DBMS.

### Cloudinary Set Up

![Settings](/images/blog/2017-02-07/pic3.png)

Keystone uses the [Cloudinary CDN](http://cloudinary.com/) system for storing images - you can also locally host all your images, but in this tutorial, I am using the Cloudinary service, as I don't use enough pictures to hit their free limit - however, if your website is going to have lots of pictures, or large sized ones, you should look into hosting them locally.

1.  Go to the [Cloudinary website](http://cloudinary.com/) and get an account.
2.  Copy the 'Environment variable' string that is shown on the Cloudinary console and put the value in the Heroku Environmental Variables section (App -> Settings).

### Tweaking the Source Code and starting KeystoneJS

The keystone.js file contains many useful switches and strings that we will want to change to wire everything up correctly:

Firstly, ensure that you are picking up the Environmental Variables from Heroku:

~~~~
keystone.set('cloudinary config', process.env.CLOUDINARY_URL);
keystone.set('cookie secret', process.env.COOKIE_SECRET);
keystone.set('mongo', process.env.MONGODB_URI);~~~~
~~~~

Set the Cookie Secret in Heroku Environmental Variable - this can and should be a random string of alphanumeric characters, like the following:

~~~~
Key:    COOKIE_SECRET
Value:  a808u0qd09d02h3081xbas12zxzx
~~~~

![Settings](/images/blog/2017-02-07/pic4.png)

Next, add the following to the keystone.init() string, which adds numerous non-default features to the editor, including the Cloudinary images:

~~~~
'wysiwyg cloudinary images': true,
'wysiwyg skin': 'lightgray',
'wysiwyg additional buttons': 'searchreplace visualchars,'
+ ' charmap ltr rtl pagebreak paste, forecolor backcolor,'
+ 'media ',
'wysiwyg additional plugins': 'table, advlist, anchor,'
+ ' autolink, autosave, charmap, contextmenu, '
+ ' hr, media, pagebreak,'
+ ' paste, searchreplace, textcolor,'
+ ' visualblocks, visualchars, wordcount'
~~~~

You can now push your changes to Heroku using:

~~~~
git add .
git commit . --message "Updated Keystone Config"
git push heroku master
~~~~

Your website will now work when you navigate to it with a web browser - it won't be customised yet, but you can see how the library works and try creating a blog post or user.

### Customising and Expanding the Website

Next time I will go into more detail on how to expand the basic functionality provided by Keystone into things other than Blog Posts (like Applications and elements on the homepage), including:

*   Adding to Data Model (Mongoose)
*   Editing and Creating Templates
*   Add the Routes, Controllers