+++ date = "2017-02-01" title = "Choosing a CMS Library for Node.js" math = "true" categories = "test" +++

Note that this blog post is out of date and doesn't refer to this version of the website.

My personal website has been a side project of mine recently - I think I have spent more time developing it than I have writing blog posts.

The website started out as an Express Web Application which used the Handlebars Templating Engine and generated the pages/routes using Express.js from a single JSON file, which was a JSON Array of 'page' Objects, which stored their content, template name, title and description along with their path and this generated the site structure using the routing functionality.

This is a major improvement on the old fashioned web design that used to take place (I remember these days) where the Content and HTML were one and the same.

However, this had several drawbacks:

The Source Code had to be changed, committed and pushed whenever an update was made to the content.
It was a nuisance to write Blog Posts in a text editor or JSON editor, as it didn't offer a WYSIWYG editor (What You See Is What You Get)
HTML Tags were included in the JSON for links, images etc.
Any double quotes in the 'scripts' or 'blog' section had to be escaped and special characters needed their HTML representation.
In order to overcome these drawbacks, I decided to move to using a Content Management System (CMS) which provides a web interface through which you can create/edit Blog Posts and other pieces of information such as Scripts or Applications. The package that I moved to had to be Node.js based and allow an easy migration of the codebase and content that I had already put together.

I investigated several CMS libraries:

Ghost - Ghost is a fully open source, hackable platform for building and running a modern online publication. It powers blogs, magazines and journalists from Zappos to Sky News. They offer a paid-for version for commercial sites.
Keystone - An open source framework for developing database-driven websites, applications and APIs in Node.js. Built on Express and MongoDB.
Apostrophe - Apostrophe is a CMS framework for Node.js that supports in-context editing, schema-driven content types, flexible widgets, and much more.
Cody - Cody is built upon 15 years of experience with many other CMS and web application frameworks ranging from widely available aging systems like Wordpress, Drupal, Sharepoint and DotNetNuke over in house developed systems in C and Java/JSP till state of the art stuff like Ruby on Rails and Google's Angular.
I decided on Keystone, as it would be the easiest way to migrate from my existing JSON and Handlebars solution to a CMS system while maintaining the control over the back-end rendering code (no Wordpress!). It also clearly seperated the User and Admin sections and required minimal configuration.

Keystone CMS itself is built on:

Node.js - Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.
Express - Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.
Handlebars - Handlebars provides the power necessary to let you build semantic templates effectively with no frustration in Node.js.
Mongoose/MongoDB - Mongoose provides a straight-forward, schema-based solution to model your application data. It includes built-in type casting, validation, query building, business logic hooks and more, out of the box.
Because all of the above libraries are Javascript based, except in MongoDB's case, which stores data as JSON 'documents', this means that you can use Javascript across the entire stack from defining the data model, making the server controller/views, examining the database to making the front-end javascript - this means developing is much easier and there isn't the inevitable loss of time to language-switching, requirement to learn multiple languages and convert between the different systems.