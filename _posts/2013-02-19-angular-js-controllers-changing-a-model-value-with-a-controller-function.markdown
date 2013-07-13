---
layout: post
title: "Angular.js Controllers: Changing a Model Value with a Controller Function"
published: true
categories:
---
This is a post of a series about [Angular.js](http://angularjs.org/), the Superheroic Javascript MVW Framework. It is extracted from my ebook [Recipes with Angular.js](https://leanpub.com/recipes-with-angular-js). If you like this post please support me and buy it!

### Problem
You want to increment a model value by 1.

### Solution
Implement an increment function which changes the scope.

We use the `ng-init` Directive to call the `incrementValue` function in the template.

{% prism markup %}
{% raw %}
<div ng-controller="MyCtrl">
  <p ng-init="incrementValue(5)">{{value}}</p>
</div>
{% endraw %}
{% endprism %}

The controller defines the default value and the `incrementValue` function.

{% prism javascript %}
function MyCtrl($scope) {
  $scope.value = 1;

  $scope.incrementValue = function(value) {
    $scope.value += 1;
  };
}
{% endprism %}

### Discussion
The `ng-init` directive is executed on page load and calls the function defined in `MyCtrl`.

You can find the complete [example](https://github.com/fdietz/recipes-with-angular-js-examples/tree/master/chapter2/recipe2) on Github.