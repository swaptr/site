---
title:  "GSOC: Week Two"
date:   2021-06-21 03:28:37 +0530
author: "swaptr"
description: "This blog post intricates my second week at KDE Community during the Google Summer of Code 2021."
tags: [gsoc, kde]
---
![GSOC banner](https://raw.githubusercontent.com/swaptr/site/master/static/images/gsoc.png "gsoc banner")

After few regressions and hours of fixing issues I got the headerbar and sidebar revamp MR merged so we start the week on a high and more than that we had the first official build of Kasts, yay!  
Sprucing up my content discovery this week and now with a plan to tackle it, I was able to use a [XmlListModel](https://doc.qt.io/qt-5/qml-qtquick-xmllistmodel-xmllistmodel.html) and parse the OPML file and was able to put together a MR for the same. I was supposed to add a C++ one instead, but the XmlListModel seemed like a perfect solution since it was supposed to be a read-only model and it was relatively easier to use (eh, lazy).  
Now lets discuss a bit what a model is and  how the XmlListModel works in Kasts.  
So a model is basically a adaptor class which is used to access structured data. Custom models can be subclasses of QAbstractItemModel, and we need to expose data at hand using a unified API. There are several QML types for eg the List Model, XML Modeln Integer Models, Object Models etc. and also you can create your own C++ data models that are used with Views such as ListView, GridView, PathView, Repeater, TableView, etc. to visualize the contents of these models and using a delegate to draw a single entry of data and interact with it.  
So the OPML file we were importing will contain our actual data. We will need a model to sort of wrap around the data and make it available for us in the form of roles and then ultimately display as a list for the users to check the search results. Remember that the data is not stored inside the model per se, the data is just provided to the model and its the models job to adapt to that data so that it can be used with a view.  
After that exposing the data to the XmlListModel is pretty easy, and I could follow along from the Qt docs.  
So all I was left to do was attach the search url which is quite simply-  
*https://gpodder.net/search.opml?q=**your-search-here***  
to a TextField and a Search button, voila, you can now search for podcasts from the internet and display the results. For more convenience I also added a drawer that would display the metadata for the podcast and also a button that would simply Subscribe the required podcast.  
Next week I would be looking to craft a massive discover page, even though this was not part of my GSoC timeline, I decided to start working on this because I just love QML :), I hope it to be a full blown podcast searching platform.