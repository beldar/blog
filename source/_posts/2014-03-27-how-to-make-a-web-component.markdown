---
layout: post
title: "How to make a Web Component"
date: 2014-03-27 17:12:53 +0000
comments: true
published: true
categories: [web components, polymer, tutorial, html5, css, js]
---

I choose the [display-rss](https://github.com/beldar/display-rss)  component because it's simple and has most of the use cases you would find in a web component.

What the component does basically loads any RSS feed and displays it

##Starting up

We'll be using [Polymer](http://www.polymer-project.org/) as the library for the polyfills and creation of the element, and the help of [Yeoman](http://yeoman.io/)  the tool I told you about in [another post](http://www.martiplanellas.info/blog/24/03/2014/the-yeoman-workflow/), and you will need to have [NodeJS](http://nodejs.org/) installed too.

We'll begin by stalling Yeoman:

	npm install -g yo
	
Then we'll install the Polymer Generator

	npm install -g generator-polymer
	
Then create a new folder `mkdir my-element && cd $_`

And start the generator `yo polymer`

This will create the base app structure, and will also install [grunt](http://gruntjs.com/) and [bower](http://bower.io/), go make a cup of tea.

Once all dependencies are downloaded we can create our element using:

	yo polymer:element my-element
	
This command will ask us if we want to include a constructor, we mark the check, and we also mark that we want to import to our index.html using HTML imports.

Then it will ask if we want to import local elements, we leave blank.

Then it will ask if we want to import installed Bower elements, we are going to write `polymer-json` since we'll be using it later.

Finally this will create our element in `app/elements/my-element.html`.

Since we said we wanted the `polymer-jsonp` element we have to install it: `bower install --save Polymer/polymer-jsonp`

The `--save` option ensure that this gets added to your bower.json file, if you are asked to choose a suitable version for polymer, I usually choose the one that specifies a version number.

<!-- more -->

##Our web component

So we have our element in `app/elements/my-element.html`, you should replace every ocurrence of `polymer-my-element` for simply `my-element`, this is a bug of the generator that they're going to fix very soon.
	
Now we have our element and it looks like this:

``` html
<link rel="import" href="../bower_components/polymer/polymer.html">

<link rel="import" href="../bower_components/polymer-ajax/polymer-jsonp.html">
<polymer-element name="my-element"  constructor="MyElementElement" attributes="">
  <template>
    <style>
      /* styles for the custom element itself - lowest specificity */
      :host { display: block; }
      /* 
      style if an ancestor has the different class
      :host(.different) { } 
      */
    </style>
    <span>I'm <b>polymer-myElement</b>. This is my Shadow DOM.</span>
        <polymer-ajax></polymer-ajax>
  </template>
  <script>
    Polymer('my-element', {
      //applyAuthorStyles: true,
      //resetStyleInheritance: true,

      // element is fully prepared
      ready: function(){ },
      // instance of the element is created
      created: function() { },
      // instance was inserted into the document
      enteredView: function() { },
      // instance was removed from the document
      leftView: function() { },
      // attribute added, removed or updated
      attributeChanged: function(attrName, oldVal, newVal) { }
    });
  </script>
```What we see here is the base structure of our web component, the `<template>` tag you see is what creates the shadow DOM, so everything inside should be encapsulated, we have our style rules, our html and templates and our js.

##Creating the display-rss component

The first thing we need to do is learn how the `<polymer-jsonp>` element works, it behaves like the `<polymer-ajax>` one, you can find the documentation [here](http://www.polymer-project.org/docs/elements/polymer-elements.html).

To be able to get a cross domain feed we can use Google Feed url, which takes a feed url, transforms it to json and give us the result using jsonp.
	
The idea is that you can make it all work with declarative sintax, so we'll configure it like this:

{% raw %}
``` html
<polymer-jsonp  id="feed_data"
                    url="http://ajax.googleapis.com/ajax/services/feed/load?v=1.0&num={{entries}}&q={{url}}&callback="
                    auto="true"
                    response="{{data}}"
                    on-polymer-response="{{responseHandler}}"></polymer-jsonp>
```
{% endraw %}
To understand this code we need some notions of how data-binding works in Polymer, take a quick read at [this article](http://www.polymer-project.org/docs/polymer/databinding.html).
	
In the code we are saying that this element has the id _feed_data_ the {% raw %}`{{url}}`{% endraw %} means its data-binded to the variable _data_  and {% raw %}`{{entries}}`{% endraw %} with the _entries_ variable, fires up automatically, puts the response on the _data_ variable, and on-response calls the _responseHandler_ function.

We also going to define the _url_ and _entries_ variables in the attributes so can be changed from outside: `attributes="url entries"`

Then we modify the Polymer function to reflect those variables and functions:

``` js
    Polymer('my-element', {
      url: 'http://feeds.bbci.co.uk/news/england/rss.xml',
      entries: 10,
      data: null,
      // element is fully prepared
      ready: function(){ },
      
      responseHandler: function(){
        console.log(this.data);
      }
    });
```
	
Ok lets try if this works, before go to the `app/index.html` and change `<polymer-my-element></polymer-my-element>` for `<my-element></my-element>`, and lets fire up the server, run this in the root of your project:

	grunt serve
	
This command should prepare everything, and pop up a browser with the website running, if we take at the console in our dev tools we should see an object containing our feed.

Now is the time to display this data, we'll show the feed title, the last time it was updated and we'll loop through the entries using templates:

{% raw %}
``` html
    <div class="container">
        <h1 class="feed_title">{{feed.title}}</h1>
        <small>Last updated: {{lastUpdated}}</small>
        <ul class="entries">
        <template repeat="{{entry in feed.entries}}">
            <li>
                <a href="{{entry.link}}" target="_blank" class="entry_title">{{entry.title}}</a>
                <small class="entry_published">{{entry.publishedDate}}</small>
                <p>{{entry.contentSnippet}}</p>
            </li>
        </template>
        </ul>
    </div>
```
{% endraw %}
We also can refresh the feed every X milliseconds, the whole thing looks like this:

``` js
    Polymer('my-element', {
      //applyAuthorStyles: true,
      //resetStyleInheritance: true,
      url: 'http://feeds.bbci.co.uk/news/england/rss.xml',
      entries: 10,
      refresh: 1000,
      lastUpdated: 'Not updated yet',
      
      ready: function() {
          var that = this;
          if (this.refresh) {
              setInterval(function(){
                  that.$.feed_data.go();
              }, this.refresh);
          }
      },
              
      responseHandler: function() {
        if (this.data.responseStatus === 200) {
            this.lastUpdated = new Date()
            this.feed = this.data.responseData.feed;
        }
      }     
      
 });
```
The code is pretty self explanatory, the only curious line is perhaps line 13 where we select by id directly using `this.$.feed_data`  _feed_data_ is the id of our _polymer-jsonp_ element that has a js API called `go()` to fire up the ajax call.

And that's it! Is **that** easy!We now have a functional custom element that we can configure via attributes like this:

	<my-element url="http://rss.news.yahoo.com/rss/topstories" entries="20" refresh="10000"></my-element>
	
Fire up your `grunt serve` and you should see a news feed.