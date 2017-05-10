---
title: Internet Explorer Out of Memory Error
description: We are used to deal with some really weird Internet Explorer bugs and I thought that nothing can really surprise me when it comes to the Microsoft's browser. The article describes another weirdness of the JavaScript engine in IE.
author: Kuba Kuźma
---

Currently I'm implementing a single page application that's going to be embedded in my client's Microsoft SharePoint. The application has to run as smoothly as possible on Internet Explorer, because majority of clients connecting to the SharePoint service uses Windows and Internet Explorer. It's basically a tool for visualising and filtering hundreds of megabytes of geospatial data coming from a local ArcGIS server. It turns out to be a pretty challenging task for a browser, especially for IE.

The application loads data from the local ArcGIS server, running on a different machine than the SharePoint. Fortunately, ArcGIS is kind enough to support cross domain JSONP requests, so I decided to use this technique to communicate with the server. Although it seemed to work fine at the beginning, it stopped working immediately after seeding the database with some real-life data set. It still worked in Chrome and Firefox obviously, but a strange looking `SCRIPT14: error -2147024882` appeared during the loading process in IE.

> Error -2147024882. There is not enough memory available to perform the operation. MAPI_E_NOT_ENOUGH_MEMORY

The JSONP response weighted roughly around 1 MB, so it looked like I exceeded a JavaScript size limit in IE. But after googling a while I found out that it's not about a size of the script at all, but <b>IE actually limits a number of number literals in your JavaScript</b>. For instance a script:

~~~ javascript
var count = 2,
    people = [{
       name: "Joe",
       age: 44,
       updatedAt: 1382351154540
    },
    {
       name: "Alice",
       age: 37,
       updatedAt: 1382351205213
    }];
~~~

…contains 5 number literals. As soon as you reach 2<sup>16</sup>-2 (65,534) number literals in your script, it simply stops working. JSONP requests is nothing more than a dynamically added `<script>` tag, so as you may guess the bug affects every script that you load in IE, not only JSONP. The limit is present in IE 9 and probably in earlier versions as well. It seems to be fixed in IE 10.

In some cases the problem is easy to fix - you can just split your script to a few separate files and load them consecutively. Another option is using strings instead of number literals, like `Number("3.13")`. But sometimes, like in my particular use case, you can't alter the script's structure and you can't really predict how many numbers you'll get in the script. You can use `LIMIT` clause in ArcGIS, but you never know if a single object contains a polygon with more than 32,767 edges for instance. There's no way to fix JSONP, so I had to get rid of it. Fortunately for me, ArcGIS supports CORS as well and it works fine in IE 8 and later versions, so <b>there's no limit of number literals in `JSON.parse` method</b>. That's another example why you should always use `JSON.parse` instead of `eval`.

*[JSONP]: JSON with padding
*[IE]: Internet Explorer
*[CORS]: Cross-origin resource sharing
