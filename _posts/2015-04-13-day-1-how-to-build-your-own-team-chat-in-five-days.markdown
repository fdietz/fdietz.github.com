---
layout: post
title: Day 1 - How to build your own team chat in five days
published: true
---

Hi and welcome to my little experiment! I'd like to built my own team chat in five days and invite you to join me along the way.

You probably know [Slack](https://slack.com/), [Hipchat](https://www.hipchat.com/), [Campfire](https://campfirenow.com/) and the like. All these are without a doubt excellent services but they are not as open as I'd like them to be. This starts with the fact that there are all data silos and use proprietary protocols and obviously the source code is not open to contribution.

In the next 5 days I'd like to show you how to build your own chat service and will give you a basis you can work with to add your own unique ideas. So, the emphasize here will be learning how to build your own system in comparison to all these hipster startups out there who focus on saving the world ;-)

<div class="centered-image">
  <img src="/images/DSCF2636.JPG">
  <p>Obligatory hipster workspace pic</p>
</div>

Since I haven't really done any upfront planing or detailed product definition we both won't really know how this is going to work out or how the final product is going to look like. But, I promise this will not be just another polished blog serious about how to use Socket.IO to implement a chat room :) Instead I'm going to write my progress in this blog on a daily basis and you can join me along for the ride. It'll be hopefully much more personal and give you more insights how I approach such a task.

I will focus much more on the design and implementation challenges on the frontend instead of the backend. So, this will be especially interesting for AngularJS developers who want to learn how to structure more complex projects. For some more AngularJS background knowledge I recommend [Recipes with AngularJS](http://fdietz.github.io/recipes-with-angular-js/) from your truly! Since I'd like to challenge myself, I'm going to use ES6 which is currently not really well documented. But, it is certainly the future of Javascript development and I'm eager to get to know it better in these upcoming days.

As a prerequisite I assume you have basic knowledge of HTML, CSS, Javascript and AngularJS - bonus points for NodeJS and things like [gulp](http://gulpjs.com/).

So, take a deep breath and a cup of tea and let's get started!

<hr>

Usually when I start working on a "new" idea, I first search on Google what's out there already. Resources as for example [dribbble](https://dribbble.com/search?q=UI) or [behance](https://www.behance.net/search?field=51) are a great source of inspiration for me - especially since I'm not a designer! So without further gequassel I proudly present you "chitchat".

<div class="centered-image shadow">
  <img src="/images/chitchat_screen_1.png">
  <p>HTML mockup of chitchat</p>
</div>

I know - it's not pixel perfect. But, it's a first iteration done in HTML, SCSS and is therefore a nice basis for adding interaction using AngularJS later on.

You can already see where this is heading from the mockup. The sidebar shows some public channels and some private one-to-one conversations. So, we will definitely need some user management and a way to join a public channel. The little red badge shows two recent messages and I should probably come up with a distinction for important messages as for example a person being mentioned vs an ongoing conversation in a public channel.

Then there's the main view showing the ongoing conversation and of course it would be awesome if we can get this to work real time using a server push mechanism. I'm not sure yet if adding support for file sharing should be part of this five days sprint.

In the right hand corner there's a search field, but we'll have to see if there's enough time for that at the end of the week.

<hr>

You can find the complete sourcecode on the accompanying [Github repo](https://github.com/fdietz/how_to_build_your_own_team_chat_in_five_days/tree/master/day_1). You can either download the repository as a zip file or use [git](http://git-scm.com/) to clone the repository.

Let's have a look at the content using the [tree](http://mama.indstate.edu/users/ice/tree/) command:

{% prism javascript %}
└── day_1
    ├── fonts
    ├── images
    ├── styles
    ├── gulpfile.js
    ├── index.html
    ├── package.json
    └── README.md
{% endprism %}

I've started with a simple `index.html` and created directories for fonts, images and css files. The `package.json` and `gulpfile.js` are used for my CSS workflow. I'm a fan of [Sass](http://sass-lang.com/) and the [SMACSS style guide](https://smacss.com/), hence the directory structure:

{% prism javascript %}
└── styles
    ├── application.css
    ├── application.scss
    ├── base
    │   ├── _fonts.scss
    │   ├── _globals.scss
    │   ├── _normalize.scss
    │   └── _variables.scss
    ├── layout
    │   └── _scaffold.scss
    ├── modules
    │   ├── _badge.scss
    │   ├── _input.scss
    │   ├── _main_header.scss
    │   ├── _message_form.scss
    │   ├── _message_list.scss
    │   └── _sidebar.scss
    └── pages
{% endprism %}

I've used a gulp task to compile my `scss` files to `css` for me.

{% prism javascript %}
var gulp         = require('gulp');
var sass         = require('gulp-sass');
var sourcemaps   = require('gulp-sourcemaps');
var autoprefixer = require('gulp-autoprefixer');

gulp.task('styles', function() {
  gulp.src('styles/*.scss')
    .pipe(sourcemaps.init())
      .pipe(sass({ style: 'expanded' }))
      .pipe(autoprefixer('last 2 version'))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('styles'))
});

gulp.task('watch', function(){
  gulp.watch('./styles/**/*.scss', ['styles']);
});

gulp.task('build', ['styles']);

gulp.task('default', ['build', 'watch']);
{% endprism %}

You can run it by executing `gulp` on the commandline. This will trigger the `default` task which will in turn call the `build` task. A file watcher is used to rebuild the CSS on file changes using the `watch` task. The output will be place in `styles/application.css`.

<hr>

You can find the complete sourcecode on the accompanying [Github repo](https://github.com/fdietz/how_to_build_your_own_team_chat_in_five_days/tree/master).

<hr>

Okidoki. This is it for day 1 - hope you liked it! Tomorrow we will look into using AngularJS to structure our team chat and get some initial interaction going. See you tomorrow!




