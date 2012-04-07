---
layout: post
title: "Replacing rails views with ember.js (dash-it-app.herokuapp.com)"
category: Programming
tags: ["ruby", "rails", "ember.js", "coffeescript"]
---
{% include JB/setup %}

In the past year or so, there has been a lot of discussions about client side javascript model-view-controller (MVC) frameworks. In this category, both
[backbone.js](http://documentcloud.github.com/backbone/) and [ember.js](http://emberjs.com/) have been particularly in focus. They both have in common the
interest in providing structure and architecture to web applications and also to allow one to build rich user interfaces.

The idea of building rich and structured web applications is very appealing. Furthermore, combining this with ruby on rails, which also provides structure and an
asset pipeline that processes your assets (processes, minifies and compresses javascript), makes it the perfect combination.

I then decided to try out ember.js with ruby on rails.

After seeing a number of tutorials and examples, I decided to build an app which would, to some extent, replace rails views with ember.js and take advantage of 
the the fact that it is not necessary to download a new page every time a user interacts with the server. I built a simple todo list that allows one to create projects 
and a list of todos per project. The projects page is almost entirely done using ember.js.

You can either try it online ([http://dash-it-app.herokuapp.com](http://dash-it-app.herokuapp.com)) or download the code and try it locally ([http://github.com/carvil/dash-it](http://github.com/carvil/dash-it)). 

Disclaimer: this is my first experience with client side MVC frameworks and therefore I am not an expert in the matter. Thus, I kindly ask you to take it with a grain of salt.

### Practicalities

On the practical side, there are a few things that made it easier for me to use ember.js with rails. 

- create structure: in general, ember.js examples have some structure, however you will still find controllers, models and views in the same file. I find it hard to understand the code if one just puts everything in the same file. One of the great things about rails is the structure one finds in the code. Therefore, I created a directory in `assets/javascripts` which contains four directories: `controllers`, `models`, `templates` and `views`. 

- use coffeescript: I also find it much easier to write coffeescript instead of javascript. The syntax is much cleaner and rails automatically converts the coffee files to javascript. I personally find this:

{% highlight coffeescript %}
    DashIt.Project = Ember.Resource.extend
      init: ->
        @set "todos", DashIt.todosController.create(project: @ )
        @set "user_id", DashIt.projectsController.get("user_id")
{% endhighlight %}

much more readable than this:

{% highlight javascript %}
    DashIt.Project = Ember.Resource.extend({
      init: function() {
        this.set("todos", DashIt.todosController.create({
          project: this
        }));
        return this.set("user_id", DashIt.projectsController.get("user_id"));
      }
    });
{% endhighlight %}

### Pain points

- Nested resources: I also decided to use [ember-rest](https://github.com/cerebris/ember-rest) instead of [ember-data](https://github.com/emberjs/data), for RESTful resources, due to its simplicity. Since every decision comes with a price, ember-rest doesn't have support for nested resources. Since I have defined that my `todos` depend on `projects`, I had to write the dependencies explicitly in ember.js.

- Debug ember.js: ember.js objects are very complex. It takes quite a long time to understand Ember.Object and what properties you can use. For more details, I recommend you to read [Understanding Ember.Object](http://www.cerebris.com/blog/2012/03/06/understanding-ember-object).

- MVC on top of MVC: at first, it seems a bit awkward to use an MVC framework (ember.js) on top of another MVC framework (rails). However, since rails exposes the resources in a nice way, it is easy to see rails as an API and have ember.js handle the view part. Once one sees rails as an API, then it becomes easier.

- Rails helpers: using pure ember views means one won't be able to use rails helpers in the views, which means writing more code. One example is the checkbox I added to each todo item. In rails, you only need one line of code to create a checkbox. In ember, due to the fact that I needed it to perform custom actions on click, it takes one view with around 20 lines of code.

### The good part

- No more page loads: one doesn't need to reload the page when a user executes an action. This means a better user experience and a quicker UI.

- Consistent user interface: one can also see the changes being propagated across the same page opened by different users. Since ember.js keeps the data consistent and uses the underlying API, it is possible to see the changes done by a user in one browser being propagated in the same page opened by different users in real time.

### Conclusion

Ember.js has proven to be a very interesting framework. One can build great applications with it, and the integration with rails makes it even easier. However, since it's still in its early days, there is still not a lot of documented examples on how to use it or which are the best practices to follow. It is a trial and error process, however, one that is worth the time and effort.

