---
layout: post
title: Vue.js Component Composition with Scoped Slots
published: true
---

In the [previous post](/2018/09/05/vue-js-component-composition-with-slots.html) we looked into slots and named slots to compose our components and content in a very flexible way.
There's one catch though we have not discussed. The content we pass to our slot is in the context of the parent component and not the child component. This sounds quite abstract, let's build an example component and investigate the problem further!

## List of Items Example

Probably the most canonical example for this kind of scenario is a todo list which renders for each todo a checkbox with name.

<figure>
  {% asset list.png width="50%" alt="Example 1"%}
  <figcaption>Example 1</figcaption>
</figure>

{% prism markup %}
{% raw %}
<div id="demo">
  <div class="list">
    <div v-for="item in listItems" key="item.id" class="list-item">
      <input type="checkbox" v-model="item.completed" class="list-item__checkbox" />
      {{item.name}}
    </div>
  </div>
</div>
{% endraw %}
{% endprism %}

{% prism javascript %}
new Vue({ 
  el: '#demo',
  data: {
    listItems: [
      {id: 1, name: "item 1", completed: false},
      {id: 2, name: "item 2", completed: false},
      {id: 3, name: "item 3", completed: false}
    ]
  }
});
{% endprism %}

In the next step we refactor this code into a reusable list component and our goal is to leave it up to the client of the component to decide what and how to render the list item.

## Refactor to Reusable List component

Let's start with the implementation of the List component:

{% prism javascript %}
Vue.component("List", {
  template: "#template-list",
  props: {
    items: {
      type: Array, 
      default: []
    }
  }
});
{% endprism %}

{% prism markup %}
{% raw %}
<template id="template-list">  
  <div class="list">
    <div v-for="item in items" class="list-item">
      <slot></slot>
    </div>
  </div>
</template>
{% endraw %}
{% endprism %}

Following our previous examples we use the default slot to render a list item.

And now make use of our new component:

{% prism markup %}
{% raw %}
<div id="demo">
  <List :items="listItems">
    <div class="list-item">
      <input type="checkbox" v-model="item.completed" class="list-item__checkbox" />
      <div class="list-item__title">{{item.name}}</div>
    </div>
  </List>
</div>
{% endraw %}
{% endprism %}


But, when trying this example we run into a Javascript error message:

{% prism markup %}
{% raw %}
ReferenceError: item is not defined
{% endraw %}
{% endprism %}

It seems we cannot access `item` from our slot content. In fact the content we passed runs in the context of the parent and not the child component `List`.

Let's verify this by printing the total number of items in our `List` component using the `listItems` data defined in our Vue instance.

{% prism markup %}
{% raw %}
<div id="demo">
  <List :items="listItems">
    <div class="list-item">
      {{listItems}}
    </div>
  </List>
</div>
{% endraw %}
{% endprism %}


That works because we run in the context of the parent component which is in this example the Vue instance. But, how can we pass the `item` data from our child `<List>` to our slot? This is where "scoped slots" come to the rescue!

Our component must pass along `item` as a prop to the slot itself:

{% prism markup %}
{% raw %}
<template id="template-list">  
  <div class="list">
    <div v-for="item in items" class="list-item">
      <slot :item="item"></slot>
    </div>
  </div>
</template>
{% endraw %}
{% endprism %}

Note, that it is important to pass this with a binding `:item` instead of only `item`!

Okay let's try this again:

{% prism markup %}
{% raw %}
<div id="demo">
  <List :items="listItems">
    <div slot-scope="slotProps" class="list-item">
      <input type="checkbox" v-model="slotProps.item.completed" class="list-item__checkbox" />
      <div class="list-item__title">{{slotProps.item.name}}</div>
    </div>
  </List>
</div>
{% endraw %}
{% endprism %}

This time we use the `slot-scope` attribute and assign the name `slotProps` to it. Inside this scoped slot we can access all props passed along via this `slotProps` variable.

In Vue.js 2.5.0+, scope is no longer limited to the `<template>` element, but can instead be used on any element or component in the slot.

## Extending the rendering of the list item

Now that we know how to pass data along we are free to extend the list item with some new functionality without changing the List component. It would be awesome if we could remove a todo item!

First of all we define the Vue app with a method to remove a todo item:

{% prism javascript %}
new Vue({ 
  el: '#demo',
  data: {
    listItems: [
      {id: 1, name: "item 1", completed: false},
      {id: 2, name: "item 2", completed: false},
      {id: 3, name: "item 3", completed: false}
    ]
  },
  methods: {
    remove(item) {
      this.listItems.splice(this.listItems.indexOf(item), 1);
    }
  }
});
{% endprism %}

We use the Javascript [splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) function to remove the item using it's index from `listItems`.

Next, we use this method when rendering the list item:

{% prism markup %}
{% raw %}
<template slot-scope="slotProps" class="list-item">
  <input type="checkbox" v-model="slotProps.item.completed" class="list-item__checkbox" />
  <div class="list-item__title">{{slotProps.item.name}}</div>
  <button @click="remove(slotProps.item)" class="list-item__remove">×</button>
</template>
{% endraw %}
{% endprism %}

We add a button with a `click` event which calls our previously defined `remove` function. That's it!

## Using Destructuring for the `slot-scope`

We can further simplify this template by using a modern Javascript trick on the `slot-scope` attribute.

Here's an example of using Javascript "destructuring" to access an attribute of an object:

{% prism javascript %}
const item = slotProps.item;
// same as 
const { item } = slotProps;
{% endprism %}

Instead of using the value `slotProps` we can now access the `item` directly.

Let's use this in our template:


{% prism markup %}
{% raw %}
<template slot-scope="{item}" class="list-item">
  <input type="checkbox" v-model="item.completed" class="list-item__checkbox" />
  <div class="list-item__title">{{item.name}}</div>
  <button @click="remove(item)" class="list-item__remove">×</button>
</template>
{% endraw %}
{% endprism %}

This is easier to read because we can directly use the `item` variable instead of always going via `slotProps.item`.

## Summary

In this chapter we used scoped slots to allow the parent to access data from the child. This gives us lots of new possibilities which weren't possible before. This feature is especially useful in scenarios where you want to leave the rendering of the slot content to the user of the component. In our case the list component is very reusable by decoupling the rendering of the list items.

You can find the complete examples on [Github](https://github.com/fdietz/vue_components_book_examples/tree/master/chapter-4).

If you like this post also check out my new course [Vue.js Component Patterns Course](/vue-component-patterns-course.html).

Stay tuned for my upcoming post about headless components!
