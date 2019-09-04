---
layout: post
title: Gulpjs 4 + Expressjs 4 = &#9829;
tags: [Gulp, ExpressJS, NodeJS, Javascript, Web, Programming]
author-id: lehbyte
color: orangered
excerpt_separator: <!--more-->
---

If you go to [gulpjs.com](https://gulpjs.com) you will be greeted with the following banner;
> Automate and enhance your workflow

More precisely, gulp is a frontend build system. Some would call it a;
> task runner built on Nodejs and npm used for automation of time-consuming and repetitiev tasks involved in web development line minification, concatenation , cache busting, unit testing, linting, optiization, etc. 
<!--more-->
I recently made an express app and wanted to use Gulp, however the latest info I could find on how to integrate gulp in an expressjs app was outdated. It only applied to version 3 of Gulp, the latest was 4.0. 

I tried some hacks that didn't get me far until I gave up and decided to finally headed over to the gulp website for some quick solution and what do I see, code, yes a full page example showing a gulp 4.0 file. I quickly tried some fixes and....

Voila! Nothing worked. So I decided that I didn't really understand how gulp worked and decided to read the API. If you don't feel like going through those pages, this article is for you. 
## What is missing?

Here is my gulp file. 
{% highlight js %}
cont { src, series, paralell, dest, watch } = require("gulp");
const sass = require('gulp-sass');
const plumber = require('gulp-plumber');
const cleanCSS = require('gulp-clean-css');
const browserSync = require("browser-sync");

//transpile
function transpile(){
    return src( 'assets/sass/*.scss' )
        .pipe(plumber()
        .pipe(sass())
        .pipe(cleanCSS())
        .pipe(dest( 'assets/css/' ))
        .pipe(browserSync.reload( stream ));
}

function bwsync(cb){
    browserSync.init({
        proxy: "localhost:3000",
        port: "5000"
    } cb();
}

function watchCss(cb){
    watch( 'assets/sass/*.scss', transpile );
    cb();
}
{% endhighlight %}

Thats OK.
Lets take a look at the default task;

```js
export.default = parallel(series(transpile, watchCss) bsync);
```

Have you figured out what is missing? 

A server.

Contrary to what you might think, `browserSync` does not lauch your express app, you have to launch it your self for `browserSync` to proxy it unless you specify an `index.html` (or just `html`) file directory. Otherwise `browserSync` will be stuck in a loop trying to look for `localhost:3000`  to proxy it when the `server` is actually not running.

## Nodemon to the rescue

Let’s write a function to launch our server so that we can call it before the browser sync task but first, we have to import the package up top.
`const nodemon = require('gulp-nodemon');`

```js
function serve(cb){
    let started = false;
    return nodemon({
            script: 'bin/www'
    }).on('start', function(){
         if(!started){
             cb();
             started = truel
          }
     });
}
```

Now let us modify our exports to reflect these changes;

```js
 parallel( series(transpile, watchCss), serve, bsync );
```

Now we should be able to modify our sass files in realtime.