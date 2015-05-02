---
layout: post
title: Day 2 - How to build your own team chat in five days - ES6 Modules, AngularJS, SystemJS and JSPM
published: true
redirect_from:
  - /2015/04/14/day-2-how-to-build-your-own-team-chat-in-five-days.html
---
This is part 2 of an ongoing series of blog posts. Read [part 1](/2015/04/13/day-1-how-to-build-your-own-team-chat-in-five-days.html) first and then come back, please!

<hr>

Welcome again to Day 2! We have quite a lot to cover today. Since I'm very much into using ES6 syntax, I've spend lots of time looking into module loading and workflows today.

First we'll have a look at module loading, then we develop an AngularJS seed project which can be used as a basis for new projects and finally we discuss how to use ES6 classes and module loading with AngularJS.

## CommonJS vs AMD vs AngularJS Modules vs ES6 modules

Before we can get our hands dirty we have to look into how modules are loaded using Javascript. A module can mean various things to different people. For this article, let's use this definition:

<blockquote>
  <p>Modules allow code to be logically separated for developers. It additionally prevents namespace conflicts in Javascript.</p>
</blockquote>

Probably the most famous module in the Javascript community is [jQuery](https://jquery.com/) but it also could be some shared code in your team or even a component or feature in your codebase.

Let's look into a namespace conflict using jQuery as an example of the current state of module loading. If you use jQuery, it will set the global variable `$`. But, if you have an existing variable `$` in your codebase those variables will conflict. jQuery's solution was to introduce a `noConflict` mode which basically let's you define the variable name of the library.

A rather clumsy solution which is why the community created lots of workarounds. The most interesting standards in my opinion are [CommonJS](http://wiki.commonjs.org/wiki/CommonJS) due to its popularity in the [NodeJS](https://nodejs.org/) community and AMD (Asynchronous Module Definition) with the most popular implementation being [Require.JS](http://requirejs.org/).

CommonJS is commonly used with NodeJS where Javascript is executed on the server. In this case all modules are loaded synchronously. This is in contrast to AMD which was designed to be used in browsers on the client side where modules are loaded asynchronously. Somewhere in between lies [browserify](http://browserify.org/) which uses the CommonJS standard to bundle all Javascript code on the server before running it on the client. If you are interested in all the glory details I can recommend [Addy Osmani - Writing modular Javascript with AMD, CommonJS & ES Harmony](http://addyosmani.com/writing-modular-js/).

Now when AngularJS appeared it introduced dependency injection. AngularJS modules, controllers, directives etc. can define their dependencies and everything is taken care of for the developer. One can even use dependency injection to mock things like HTTP requests in unit tests. Great! But, it relies on all the Javascript to be available in the browser and is not able to request modules asynchronously.

There are are projects which combined [ui-router](https://github.com/angular-ui/ui-router) with [ocLazyLoad](https://github.com/ocombe/ocLazyLoad) to lazy load angular modules as defined by the routing configuration. But, this feels more like another workaround and even more specific due to its coupling with Angular's routing configuration.

Now here come EcmaScript 6 modules to the rescue. The standard tries to make both users of CommonJS and AMD happy and (hopefully) solves all our problems.

Let's have a look at an example with `named` exports:

{% prism javascript %}
// lib.js
export function sum(x, y) {
  return x + y;
}

export function multiply(x, y) {
  return x * y;
}

// main.js
import {sum, multiply} from "./lib";

console.log(sum(1, 2));
console.log(multiply(1, 2));
{% endprism %}

Here's another example with a `default` export:

{% prism javascript %}
// lib.js
export default function sum(x, y) {
  return x + y;
}

// main.js
import sum from "./lib";

console.log(sum(1, 2));
{% endprism %}

The design goals of the spec are the following:
* Default exports are favoured
* Static module structure
* Support for both synchronous and asynchronous module loading
* Support for cyclic dependencies between modules

For even more details read [Axel Rauschmayer - ECMAScript 6 modules: the final syntax](http://www.2ality.com/2014/09/es6-modules-final.html).

Now, the only problem is that browsers don't all support the new standard yet, so we have to transpile our code to ES5 in order to work with the new module syntax today.

## Gulp Workflow using gulp with SystemJS and JSPM
Let's have a look into a gulp workflow which uses [SystemJS](https://github.com/systemjs/systemjs) and [JSPM](http://jspm.io/).

SystemJS builds on top of the [es6-module-loader](https://github.com/ModuleLoader/es6-module-loader) polyfill and adds the capability to load modules that are defined in a variety of syntaxes:
* CommonJS
* AMD
* ES6 Modules (using either [Babel](https://babeljs.io/) or [traceur-compiler](https://github.com/google/traceur-compiler))
* modules that export a global variable (as for example jQuery) via a shim config

JSPM is a Javascript package manager that sits on top of SystemJS and is able to load modules directory from any registry such as npm, github or bower. In development mode it loads modules as separate files with ES6 and in production it optimizes the modules into a self-executing bundle (without runtime dependency).

This is how our index.html file looks using SystemJS:

{% prism markup %}
<!doctype html>
<html>
  <head>
    <script src="jspm_packages/system.js"></script>
    <script src="config.js"></script>
    <script>
      System.import('app/bootstrap');
    </script>
  </head>
  <body>
    // fancy stuff here
  </body>
</html>
{% endprism %}

First it loads the SystemJS library and then the `config.js` file which contains the configuration for your modules. Then we can use `System.import`, the proposed browser loader API for dynamically loading ES6 modules, to import the file `app/bootstrap`.

Now JSPM is used to install all our dependencies using the `jspm install` command, for example:

{% prism javascript %}
jspm install jquery
{% endprism %}

JSPM looks up jquery in its registry and installs jQuery in the current file system. Additionally, it adds jQuery to the `package.json`, so that your jQuery is only a `jspm install` away in the future.

If we look at `config.js`, we can see that JSPM has modified it:

{% prism javascript %}
System.config({
  "paths": {
    "*": "*.js",
    "github:*": "jspm_packages/github/*.js"
  }
});

System.config({
  "map": {
    "jquery": "github:components/jquery@^2.1.3"
  }
});

System.config({
  "versions": {
    "github:components/jquery": "2.1.3"
  }
});
{% endprism %}

These paths and mappings tell JSPM how to install and resolve your module. Most of the time you wont' need to change anything but sometimes it can be useful to map a longer package name to a smaller one.

Now, you can load jQuery using ES6 syntax:

{% prism javascript %}
import $ from 'jquery';

console.log($.fn.jquery);
{% endprism %}

JSPM has several advantages over tools like RequireJS or browserify. With RequireJS you have to install via tools like [bower](http://bower.io/), but then manage the mappings and naming of modules manually. Additionally, with SystemJS you don't need any type of build tool since it's all run and compiled in the browser.

## Angular Seed Project

Let's now check out the [Angular seed project](https://github.com/fdietz/how_to_build_your_own_team_chat_in_five_days/tree/master/day_2/angular_es6_seed
). It contains some Angular example code to request and render a list of Github users and to showcase Angular working together with ES6 features.

<div class="centered-image">
  <img src="/images/github_user_list.png">
  <p>Using Angular to render a list of Github users</p>
</div>

Let's have a look into the structure:

{% prism javascript %}
├── app
│   ├── bootstrap.js
│   ├── main.js
│   ├── user.controller.js
│   ├── user.service.js
│   ├── user_list
│   │   ├── reverse_name.service.js
│   │   ├── user_list.controller.js
│   │   ├── user_list.directive.js
│   │   ├── user_list.module.js
│   │   └── user_list_directive.html
│   └── vendor.js
├── styles
├── node_modules
├── jspm_packages
├── index.html
├── config.js
├── gulpfile.js
├── package.json
└── README.md
{% endprism %}

The `index.html` file contains the SystemJS based bootstrap as describe above and some initial Angular example code:

{% prism markup %}
<!doctype html>
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="styles/application.css">

    <script src="jspm_packages/system.js"></script>
    <script src="config.js"></script>
    <script>
      System.import('app/bootstrap');
    </script>
  </head>
  <body>

    <div ng-controller="UserController as ctrl">

      <h1 ng-hide="!ctrl.loading">Loading list of Github users</h1>

      <div class="medium progress" ng-show="ctrl.loading">
        <div>Loading…</div>
      </div>

      <user-list users="ctrl.users"><user-list>
    </div>

  </body>
</html>

{% endprism %}

Since we use SystemJS we cannot use the `ng-app` directive in our `index.html`. We need to load the main Angular module using SystemJS explicitly. Let's have a look into `app/bootstrap.js` which starts our Angular app:

{% prism javascript %}
import mainModule from './main';

angular.element(document).ready(function() {
  angular.bootstrap(document, [mainModule.name], { strictDi: true });
});
{% endprism %}

And here's our first `import` statement which contains our actual main module of our shiny new Angular app:

{% prism javascript %}
import './vendor';

import UserController from './user.controller';
import UserService from './user.service';

import userListModule from './user_list/user_list.module';

let mainModule = angular.module('mainApp', [userListModule.name])
  .controller(UserController.name, UserController)
  .service(UserService.name, UserService);

export default mainModule;
{% endprism %}

Note, the last line where the `mainModule` is exported using the default export mechanism which let's us import it using: `import mainModule from './main';`.

Next we are going to look into how to use ES6 classes to define Angular controllers and services.

## AngularJS using ES6 classes and modules

Let's start with the `UserController` class imported in `main.js` above since it is the first mentioned in `index.html`:

{% prism javascript %}
class UserController {

  constructor(UserService) {
    this.UserService = UserService;
    this.loading = true;
    this.init();
  }

  init() {
    this.UserService.getUsers().then(users => {
      this.users = users;
      this.loading = false;
    });
  }
}

UserController.$inject = ['UserService'];

export default UserController;
{% endprism %}

The `UserController` uses the new ES6 constructor to define it's dependency to the `UserService` - it is a best practice to handle all model-specific business logic (as for example loading users from the backend) in services. In the `init()` function it actually requests the users and assigns them to the `users` attribute. Note the use of the `=>` (hash rocket) syntax which is a shortcut for a function and additionally binds the `this` context to the `UserController` class.

The `loading` attribute is used in `index.html` to show a loading indicator while the user list is fetched.

Angular is now able to dependency inject the `UserService` into the `UserController` since we imported it and registered it in the main module definition. Here's the code again from our `main.js` file:

{% prism javascript %}
import UserService from './user.service';

let mainModule = angular.module('mainApp', [userListModule.name])
  .controller(UserController.name, UserController)
  .service(UserService.name, UserService);
{% endprism %}

Note that our service was registered using the `service` function instead of the `factory` function. The service function expects a constructor function (an ES6 class) - similar to the controller function - whereas the factory function is syntactic sugar in case you are not using classes.

Now since we use `strictDi` (strict dependency injection) we must declare our injected class explicitly for `UserController`. This is also required for production builds which minify the Javascript code.

{% prism javascript %}
UserController.$inject = ['UserService'];
{% endprism %}

Okay, so we have an initial Angular Controller and Service running. Let's have a look into the user list directive:

{% prism markup %}
<div ng-controller="UserController as ctrl">
  <user-list users="ctrl.users"><user-list>
</user-list>
{% endprism %}

The user list directive is defined very traditionally and not as an ES6 class:

{% prism javascript %}
import template from "./user_list_directive.html!text";
import UserListController from "./user_list.controller";

function userListDirective(ReverseNameService) {
  return {
    restrict: "E",
    scope: {
      users: "="
    },
    template: template,
    bindToController: true,
    controllerAs: "ctrl",
    controller: UserListController,
    link: function($scope, element) {
      element.on("mouseover", "li", function(event) {
        let name = $(event.currentTarget).find(".primary").text();
        let reverseName = ReverseNameService.reverse(name);

        console.log("hover... ", name, "reverse", reverseName);
      });
    }
  };
}

userListDirective.$inject = ["ReverseNameService"];

export default userListDirective;
{% endprism %}

Here I export the function as is, since this is the only straight-forward way to support dependency injection.

The user list is a "component" in the sense that it contains a controller and a template with the responsibility to render the user list and handle various events. I'm therefore using the `bindToController` syntax and an externally defined `UserListController` for the business logic and use the isolated scope to define the `users` attribute as the external interface for passing data from `index.html` to the directive. I'm already looking forward to use Angular 1.4 since it supports a much cleaner syntax for `bindToController`. There's currently a lot of code required to setup such a component.

Have you spotted the strange import statement for the template?

{% prism javascript %}
import template from "./user_list_directive.html!text";
{% endprism %}

I'm using here the SystemJS [text plugin](https://github.com/systemjs/plugin-text) to load the HTML template and then assign this template as a string to the directive. Note, the `!text` expression at the end of the import statement.

<blockquote>
  <p>This must be the most horrible example of a service usage inside a directive ever! Don't try this at home.</p>
</blockquote>

Never mind my weird `link` function where I tried hard to come up with an example to use the injected `ReverseNameService`, just to prove my point that you can use dependency injection in this context.

Now if you followed me closely you found that I'm using the Angular module syntax and the ES6 module syntax side by side. Why is that?

All controllers, directives or services which are dependency injected are required to be registered via Angular's `controller`, `service` and `directive` functions. So, I've used ES6 modules wherever possible and only used Angular modules where required by Angular. As we are heading towards Angular 2.0 all these Angular modules will be replaced with ES6 modules anyways and I tried to structure the code in a way which makes a migration as simple as possible.

Now, in my case I created a user list Angular module which contains everything required for the user list directive. It is therefore completely self contained and could be easily copy and pasted into another project. It does not have any dependencies to it's parent module.

To finish off for today I will show you the `UserListController` quickly:

{% prism javascript %}
class UserListController {

  constructor() {
  }

  onClick(user) {
    console.log("on user clicked", user)
  }
}

UserListController.$inject = [];

export default UserListController;
{% endprism %}

Nothing new here really. The `onClick` function is referenced in the template of the directive here:

{% prism markup %}
<ul class="user-list">
  <li ng-repeat="user in ctrl.users" ng-click="ctrl.onClick(user)">
    <img ng-src="{{user.avatar_url}}" width="36px" height="36px" alt="">
    <div class="meta">
      <span class="primary">{{user.login}}</span>
      <span class="secondary">{{user.repos_url}}</span>
    </div>
  </li>
</ul>
{% endprism %}

<hr>

If you want to use the Angular seed project for your own projects you can directly access and git clone it from [github.com/fdietz/angular_es6_seed](https://github.com/fdietz/angular_es6_seed).

<hr>

That's it for today! Hope I haven't lost you in the confusing world of Javascript modules. See you tomorrow but this time I'm really starting with "chitchat" ;-)

<hr>

This is part 2 of an ongoing series of blog posts. Continue reading [part 3](/2015/04/15/day-3-how-to-build-your-own-team-chat-in-five-days.html).

* [Day 1 - How to build your own team chat in five days](/2015/04/13/day-1-how-to-build-your-own-team-chat-in-five-days.html)
* [Day 2 - ES6 Modules, AngularJS, SystemJS and JSPM](/2015/04/14/day-2-how-to-build-your-own-team-chat-in-five-days-es6-modules-angularjs-systemjs-and-jspm.html)
* [Day 3 - ExpressJS, Socket.io and AngularJS](/2015/04/15/day-3-how-to-build-your-own-team-chat-in-five-days-expressjs-socket-io-and-angularjs.html)
* [Day 4 - ExpressJS, Socket.io and AngularJS component-based design patterns](/2015/04/16/day-4-how-to-build-your-own-team-chat-in-five-days-expressjs-socket-io-and-angularjs-component-based-design-patterns.html)
* [Day 5 - AngularJS Filters and Directives](/2015/04/17/day-5-how-to-build-your-own-team-chat-in-five-days-angular-filters-and-directives.html)
