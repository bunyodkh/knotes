---
title: Static website with 11ty and github/netlify  
description: IMHO the easiest way of building from scratch and deploying the static site with 11ty static site generator and netlify.
date: 2020-04-18
---

# {{ title }}

<span class="post-detail-date">Posted on {{ date | readableDate }}</span>

## self-reflection

Skip this and go to **building steps** part below if you dont like reading

My previous blog was built on Django, but I've never used its powerful capabilities cause turns out I just needed basic functionality where I can add some text and pics, style them. At the end of the day share my experience on various issues.

Django needed server side configuration (including database and required libraries for python to do web stuff). Even it was cheap in terms of hosting, in my case it was still to much for personal blog.   

So, I went through several [static site generators](https://www.staticgen.com/) and built demo blogs exactly with Hugo, Nuxt, and Gatsby (mostly github starred). Just tried which one is more suitable for me in terms of the efforts needed to create and manage the app.  

I also created sites with Vue, React + firebase to see if these could suit my needs. Pure Vue and React now I think was too much for something simple that I eventually created. 

So and I came up to [Eleventy](https://www.11ty.dev/) while I was watching [live coding videos](https://www.youtube.com/playlist?list=PLz8Iz-Fnk_eTpvd49Sa77NiF8Uqq5Iykx) with Jason Lengstorf. This time he was doing some basic stuff with [Zach Leatherman](https://twitter.com/zachleat) - creator of the very static site generator. 

Personally for me it seemed so intuitive and user-friendly (compared to others) for building something with mininal effort. But I think docs still need some improvement. I had hard time with them. 

I downloaded it and built + deploy this blog in like 2-3 hours. And I didn't need much time to code but rather reading and trying things. Cause I don't like reading and thus did waste some time for guessing things out. 

## building steps

So here is the stack:
  - [Eleventy](https://www.11ty.dev/) - free
  - [Node](https://nodejs.org/en/) - free
  - [Github](https://github.com/bunyodkh/knotes) - free / my boilerplate
  - [Netlify](https://www.netlify.com/) - free for personal projects I think
  - $5 for domain registration by [Namecheap](https://www.namecheap.com/). I could register domain name on Netlify directly but for some reason it was too expensive.  

Eleventy and Node is enough to start and test locally. First, we gonna (1) create project folder go there (2) init project (create package.json) and (3) download eleventy as a project dependency.

Init project
``` js
//create project folder
mkdir demo-project && cd demo-project

//init package.json file with default options
npm init -y

//got this
Wrote to ~/demo-project/package.json:
{
  "name": "demo-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
Install Eleventy

``` js
//download eleventy
npm install @11ty/eleventy --save 
```

Create at least one file (md, index, njk, etc.) which will serve as a home page let's say.

``` js
touch index.md
```
And here is the project structure

``` js
node_modules
package-lock.json		
package.json
index.md // here will add content
```

Editing index.md file

``` md
# Hello World!

Lorem ipsum dolor sit amet consectetur, adipisicing elit.
Lorem ipsum dolor sit amet consectetur, adipisicing elit. 
```

And "compile" the content to the static html file 
``` js
// here use npx to 'run the library' instead of npm
npx eleventy

//get this - indicates that files are ready to be deployed
Writing _site/index.html from ./index.md.
```

So **_site** is default name given to the folder that keeps ready files that can be deployed to i.e. netlify static hosting and made publicly available. 

Basically this is it. You can run locally your site. 
``` js
npx eleventy --serve // this will also watch for changes you are making to i.e. index.md or other files and automatically restart server for your convinience

//go to indicated local host and check the result
[Browsersync] Access URLs:
 --------------------------------------
       Local: http://localhost:8081
    External: http://192.168.1.103:8081
 --------------------------------------
          UI: http://localhost:3002
 UI External: http://localhost:3002
 --------------------------------------
[Browsersync] Serving files from: _site
```

Result / ready website
![Homepage image](/assets/post-images/homepage.jpg)
<span class="image-caption">Homepage image</span>

**index.md** in the root folder will be recognised as the file that must be served by the root '/' url.

For other files (i.e. about.md ) - their names will be used as a URL slug. 

``` js
index.md - localhost:8080
about.md - localhost:8080/about
```
You just add file, name it properly and re-build the project. 

``` js
//create file for about route
touch about.md

//edit it, add some content
# About
this is about page

//save and re-build 
npx eleventy

//run locally
npx eleventy --serve 
```
That is all. The basic logic is super simple. 

## reusability and additional configuration

I want to list the things that someone needs beyond the basic logic. 
  - template languages
  - building pages from global and 'local' data files
  - serving static files (i.e. *.css, *.jpg, etc.)
  - additional configuration (i.e. custom filters, changing default setup, etc.)
  - pagination


**Template languages**

A few words about template language support. Eleventy out of the box supports multiple template languages (html, md, liquid, njk, hbs, ejs, [etc.](https://www.11ty.dev/docs/)). You can pick one or use them all together in a single project. You can combine them in one file (i.e. markdown + nunjucks):

``` md
---
text: Some cool title
---
# Combining markdown with nunjucks

markdown text 
<h1>{ text }</h1>
```

Better to use template language that allows you to add some logic (i.e. iterations over collections, conditionals, etc.).

Nunjucks or Liquid are great options because they are quite close to HTML in terms of syntaxs. So for more sophisticated pages Nunjucks in combination with the markdown can be used. 

So homepage can be redone appropriatly. Replace *.md extension with *.njk for Nunjucks. And add biolerplate Nunjucks code. It is really close to regular HTML. 
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Static site with 11ty</title>
</head>
<body>
  <h1>Hello World!</h1>
</body>
</html>
```
to be continued...