---
layout: post
title: "Web Components"
date: 2014-03-25 16:56:32 +0000
comments: true
categories: [web components, polymer, frontend, css, html, js]
---

Web Components had become my latest obsession, I read about them a long time ago, but it wasn't until the major players on the web arena embraced them that I really saw them as something I could use today.

When I say major players I mean Mozilla and Google, both of them started writing javascript libraries that provide [polyfills](http://en.wikipedia.org/wiki/Polyfill) for the features needed to make web components work today.

<!-- more -->

Ok, but what are web components? In words of Zeno Rocha, project lead of [customelements.io](http://customelements.io/):

> Web Components are a collection of standards which are working their way through the W3C. They enable truly encapsulated and reusable components for the web. And if you think HTML5 changed the web, wait to see what Web Components will do.

Those standards working their way through the future are:

* **Custom Elements**: Enables authors to define and use new types of DOM elements in a document.
* **Shadow DOM**: Shadow DOM is designed to provide encapsulation by hiding DOM subtrees under shadow roots. It provides a method of establishing and maintaining functional boundaries between DOM trees and how these trees interact with each other within a document, thus enabling better functional encapsulation within the DOM.
* **HTML Imports**: HTML Imports are a way to include and reuse HTML documents in other HTML documents.

These are the foundation of web components.

Mozilla is developing [x-tags](http://www.x-tags.org/) library and Google is developing the [Polymer](http://www.polymer-project.org/) library, both of them have their own way of declaring the component and the interface, but the end result is the same:

	<some-component></some-component>
	
A new element that has an encapsulated structure, style and functionality; it's highly reusable, and customisable through attributes and maybe a javascript API.

In [customelements.io](http://customelements.io/) you can find a directory of web components ready to use, most of them also can be installed via bower, and they're really fun to use.

I've created three web components so far:

* [boris-bikes](https://github.com/beldar/boris-bikes): It provides a component that fetches the [Boris Bike](http://www.tfl.gov.uk/modes/cycling/barclays-cycle-hire) data (free bikes, free docks, etc) and provides and interface for it, getting your location and showing the bikes location on a map (which is another web component).
* [tfl-status](https://github.com/beldar/tfl-status): It provides a component that fetches the Tube Status updates and shows the line status in real time.
* [display-rss](https://github.com/beldar/display-rss): Displays any RSS feed that you put on the _url_ attribute

In a future post I'll explain how I did it, what challenges did I face and some advises too.