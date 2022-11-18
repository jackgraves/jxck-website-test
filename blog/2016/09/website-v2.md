+++
date = "2016-09-04"
title = "How my (old) website was created using JSON"
math = "true"
categories = "test"
+++

> Note that this blog post is out of date and doesn't refer to this version of the website.

I recently started this website using simple, static HTML pages, without a dynamic backend. Over time, this became unmanageable and I needed a way of managing my content. Commonly this is done using a Content Management System (CMS) which provides you with a front-end (like Wordpress) for creating new articles and automatically generating index pages and other navigation so that you don't need to update every page on the website when you add a new blog post for example.

At university, I took several courses in Data Formats, which included studying XML, XPath, JSON and how efficient/easy to manipulate web transmission formats are when compared. At University, I used Java, the Document Object Model (DOM) and transformation methods like [Extensible Stylesheet Language Transformations](https://en.wikipedia.org/wiki/XSLT "XSLT on Wikipedia") (XSLT). More recently, I have been using Node.js and using JavaScript Object Notation (JSON) and its other, concise cousins (like [YAML](https://en.wikipedia.org/wiki/YAML "YAML on Wikipedia")) as storage formata, due to them being lightweight and efficient - JSON is also the primary transmission format for a [NoSQL database](https://en.wikipedia.org/wiki/NoSQL "NoSQL on Wikipedia") - such as [MongoDB](https://en.wikipedia.org/wiki/MongoDB "MongoDB on Wikipedia"). To this end, I decided that a quick, simple way to generate the content on this website is using a JSON file for all the content and then using a templating engine which iterates over the data and generates pages - which once generated can be heavily cached as they are static once the server has been started. This approach had several benefits:

*   It separates the content and the markup
*   Reduces the size of the the content to a single file
*   You can utilise cacheing as you only need to generate the pages once for each update you make
*   By using the [JSON.parse()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse "JSON.parse on Mozilla Developer") method in Javascript, you can read the file in as an object or array, depending on it's contents and query it
*   There are several packages available to perform querying on the JSON Object, this enables you to e.g. bring back all categories in the blog articles to make a word cloud (see the sidebar on the Blog page).

This should also make the transition to a full-blown database more fluid when it is necessary, as it can be moved to a NoSQL document-based database with very little code changes - in fact, none of the rendering code will need reworking, it will be changed at a higher level (the index.js router).

See an excerpt of the homepage object below:

~~~~
[
{
    "path": "/",
    "title": "Home",
    "template": "index",
    "background": "bg_architect01.jpg",
    "profileimage": "images/avatars/avatar_square_sm.jpg",
    "showIndexElements": true,
    "typetext": "Jack Graves MBCS. <br/><b>IT Professional</b>",
    "block": [
    {
        "label": "Born year",
        "value": "1993"
        },
        {
            "label": "Experience",
            "value": "2+ years"
            },
            ...
            ~~~~

I use the ['json-query'](https://github.com/mmckegg/json-query "JSON-Query on GitHub") and ['lodash.where'](https://lodash.com/docs/2.4.2#where "Where on lodash.com") packages to filter and query the JSON datafile and create the links and navigation elements - it can query the JSON as it is a common format:

![](/images/blog/2016-09-04/pic1.png)

Using the schema above, all the blog posts can be obtained using a single line of code, and they can be manipulated, for example by getting the latest 3 blog posts for the sidebar links. These two queries are shown below.

~~~~
var fullblogposts = where(json, {"template": "blogpost"}).reverse();
var top3blogposts = fullblogposts.slice(0,3)
~~~~

This can be expanded upon in many ways, another way this is utilised is for generating the blog categories from the distinct categories in all pages, the category index pages can be automatically generated.