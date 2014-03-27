---
layout: post
title: "How to make a Web Component"
date: 2014-03-27 17:12:53 +0000
comments: true
published: false
categories: [web components, polymer, tutorial, html5, css, js]
---

I choose the display-rss component because it's simple and has most of the use cases you would find in a web component.

What the component does basically loads any RSS feed and displays it

##Starting up

We'll be using [Polymer](http://www.polymer-project.org/) as the library for the polyfills and creation of the element, and the help of [Yeoman](http://yeoman.io/)  the tool I told you about in [another post](http://www.martiplanellas.info/blog/24/03/2014/the-yeoman-workflow/), and you will need to have [NodeJS](http://nodejs.org/) installed too.

We'll begin by stalling Yeoman:

	npm install -g yo
	
Then we'll install the Polymer Generator

	npm install -g generator-polymer
	
Then create a new folder `mkdir my-element && cd $_`

And start the generator `yo polymer`

This will create the base app structure, and will also install [grunt](http://gruntjs.com/) and [bower](http://bower.io/).

Once all dependencies are downloaded we can create our element using:

	yo polymer:element my-element
	
This command will ask us if we want to include a constructor, we mark the check, and we also mark that we want to import to our index.html using HTML imports.

Then it will ask if we want to import local elements, we leave blank.

Then it will ask if we want to import installed Bower elements, we are going to write `polymer-ajax` since it's a very handy element to have.

Finally this will create our element in `app/elements/my-element.html`.

Since we said we wanted the `polymer-ajax` element we have to install it: `bower install --save Polymer/polymer-ajax`

The `--save` option ensure that this gets added to your bower.json file, if you are asked to choose a suitable version for polymer, I usually choose the one that specifies a version number.

##Our web component

So we have our element in `app/elements/my-element.html`, you should replace every ocurrence of `polymer-my-element` for simply `my-element`, this is a bug of the generator that they're going to fix very soon.
	
Now we have our element and it looks like this:

{% codeblock lang:html %}
<link rel="import" href="../bower_components/polymer/polymer.html">

<link rel="import" href="../bower_components/polymer-ajax/polymer-ajax.html">
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
{% endcodeblock %}What we see here is the base structure of our web component, the `<template>` tag you see is what creates the shadow DOM, so everything inside should be encapsulated, we have our style rules, our html and templates and our js.