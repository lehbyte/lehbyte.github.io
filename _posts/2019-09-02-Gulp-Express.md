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

I would like to add just one word in that banner; `frontend` because that is what it does best.

More concisely, gulp is a frontend build system;
> It is a task runner built on Nodejs and npm used for automation of time-consuming and repetitiev tasks involved in web development line minification, concatenation , cache busting, unit testing, linting, optiization, etc. 
<!--more-->

I recently made an `expressjs` app and was looking for a way to simplify compiling to css, watching my files and serving them so I searched around for a `plugin` that would help me with these tasks. 

Now I was little bit familiar with `Grunt` but found it cumbersome and some of it's facilities weird so I looked around for a suitable alternative. 
I discovered a lot of developers praising `gulpjs` for it's ease of use. 
Moreover, many were hailing it as a potential replacement of `Grunt`. 

Consequently, I looked up some examples and tried to copy and paste, not because I was a crappy programmer who couldn't be botherd to learn the proper way of using a tool/technology, but because I wanted to gauge `gulpjs`'s ease of use, especially among first users like me. 

Needless to say, I was a litte disappointed. I had expected the problem to be easily fixable because I had brought into the narrative that `gulpjs` was essentially a plug and play plugin.
But most of the dated comments and `stackoverflow` answers were specific to `gulpjs 3.0`, therein was the problem.

So I did what any responsible programmer would do, I took off my know-it-all hat, and started googling around for information specific to  `gulp 4.0`. 
However, all I could find about that and about `gulpjs - expressjs` integration  was outdated information. Most tutorials only worked for `gulpjs 3.0` and not `gulp 4.0`. It looked like I was stuck with this problem. 

I tried a couple of hacks which hardly got me anywhere. 
Finally, I gave up :( and went to `gulpjs.com` for some quick solution, 
Much to my surprise, the site was sprinkled with snippets of code, I wondered why I hadn't visited the website first. Then I began integrating `gulpjs 4.0` into my `expressjs 4.0` app, 

Unfortunately, nothing worked. So much for those code `snippets`. 
I had to accept that I really didn't understand how gulp worked.
This humbled me enought to tackle the API and boy is that documentation all over the place. 
Anyway, I finally managed to make `Gulpjs` and `Expressjs` work together seamlessly which brings me to the point of this article; 

> If you don't feel like going through those pages, this article is for you. 

## What is missing?

This was my gulp file. 
{% highlight js %}
const { src, series, paralell, dest, watch } = require("gulp");
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

Lets take a look at the default task;

```js
export.default = parallel(series(transpile, watchCss) bsync);
```

So what is missing here?  

***A `server`. Yep.***

Contrary to what you might think, `browserSync` does not lauch your express app, you have to launch it your self for `browserSync` to proxy it or you can specify an `index.html` (or just `html`) file directory otherwise `browserSync` will be stuck in a loop trying to look for a server, `localhost:3000` for example, to proxy when said `server` is actually not running.

## Nodemon to the rescue

Ok, so let’s write a function to launch our server so that we can call it before the browser sync task.

```js
const nodemon = require('gulp-nodemon');

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
 parallel( series(transpile, watchCss, serve), bsync );
```

Happy coding. 🙂