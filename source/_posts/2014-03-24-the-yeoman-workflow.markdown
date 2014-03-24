---
layout: post
title: "The Yeoman Workflow"
date: 2014-03-24 14:39:04 +0000
comments: true
categories: [yeoman, workflow, css, html, js, bower, grunt, frontend]
---

## Searching for tools

I've been searching and trying new workflows when starting a front end project/project phase, there are a lot of interesting open source projects for rapid prototyping 'static' websites in a reusable, collaborative, optimised and intelligent way.

I've been trying tools like: 

* [Grunt](http://gruntjs.com/) for automating build tasks like optimising images, minifying and uglifying javascript, css and html.
* [Bower](http://bower.io/) a package manager for the web (meaning front end).
* [Compass](http://compass-style.org/) I know it can do a lot of things but I'm using it only to compile [Sass](http://sass-lang.com/) files.

But it wasn't until I found [Yeoman](http://yeoman.io/) that everything came toghether.

It's a tool that scaffolds, downloads and prepares all these tools for you doing the tedious configuration part and using best practices approach in all parts. 

## Instalation

 If you have node already you just have to run:

	npm install -g yo

Yeoman has lots of generators which basically are different scaffold variations using different libraries and components depending on which tools you need for the project.

The basic one is called [generator-webapp](https://github.com/yeoman/generator-webapp) you can install it running:

	npm install -g generator-webapp
	
An then create a new project like this:

	yo webapp
	
It will ask you if you if you want some additional things:

![webapp-generator](https://camo.githubusercontent.com/5f126a73cb4f0a16ca7b9e9c7b748a099458089b/687474703a2f2f692e696d6775722e636f6d2f754b545432486a2e706e67)

And BOOM, you have a project structure ready to build using grunt, you can create a web server on the fly running `grunt serve` or build the project into the `/dist` folder running `grunt build`.

Then you can use bower to install other js libraries you need like:

	bower install --save underscore

The `--save` parameter ensures that the new library is saved on the `bower.json` file.
	
All your bower dependencies will end up in the `bower_components` folder which is not version controlled, when you want to install it somewhere else you'll run `bower install` inside and it will download all the libraries you need, just as [Composer](https://getcomposer.org/) does for the back end.

## Using other generators

That's one way of doing it, another way is to use other generators that are more specific to what you'll use in your project, for example I usually use [Backbone.js](http://backbonejs.org/) in my projects, I could install it via bower, or I could use the [generator-backbone](https://github.com/yeoman/generator-backbone), which also gives me other interesting tools when building my app like generate models, collections, routers or views:

	yo backbone # generates your application base and build workflow
	yo backbone:model blog
	yo backbone:collection blog
	yo backbone:router blog
	yo backbone:view blog
	
There's a very thorough and extensive post about how to use it in [NetTuts](http://code.tutsplus.com/tutorials/building-apps-with-the-yeoman-workflow--net-33254)

There're quite a few [official generators](http://yeoman.io/official-generators.html) and tons of [community generators](http://yeoman.io/community-generators.html), you can even [write your own](http://yeoman.io/generators.html#writing-your-first-generator) with your own specifications.