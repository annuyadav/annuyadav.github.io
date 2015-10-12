---
layout: post
title: "jQuery extend functionality"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-10-12T21:16:00+0530
comments: true
---

jQuery extend is used to merge the contents of two objects in a single object. For example if we have two objects as:

{% highlight js %}
var _first_data = {
        red: 1,
        green: {average: 51, density: 90},
        blue: 20
    };

var  _second_data = {
        yellow: 38,
        green: {average: 76}
    };
    
// Merge _second_data into _first_data
$.extend( _second_data, _first_data );
{% endhighlight %}

After this the vale of _first_data will be:

{% highlight js %}
    {
        "red": 1,
        "green": {"average": 76},
        "blue": 20,
        "yellow": 38
    }
{% endhighlight %}

If you want to merge the objects recursively, that is you want to store all the keys for the "green" key's value then use:

{% highlight js %}
// Merge _second_data into _first_data, recursively
$.extend( true, _second_data, _first_data );
{% endhighlight %}

Now the value of _first_data will be:

{% highlight js %}
    {
        "red": 1,
        "green": {
            "average": 76,
            "density": 90},
        "blue": 20,
        "yellow": 38
    }
{% endhighlight %}


Suppose we have two variables _user and _client. We need to merge these two without modifying the values for _user. 
This can be done by:

{% highlight js %}
var _user = {
        validate: false,
        limit: 15,
        name: "user"
    };

var _client = {
        validate: true,
        name: "client"
    };
   
// Merge _user and _client, without modifying _user
var _project = $.extend( {}, _user, _client );

{% endhighlight %}

Now the values of all the three variables are:

{% highlight js %}
_user -- {"validate":false,"limit":15,"name":"user"}
_client -- {"validate":true,"name":"client"}
_project -- {"validate":true,"limit":5,"name":"client"}
{% endhighlight %}


### jQuery.fn.extend()

Now lets talk about jQuery.fn.extend(). It Will merge the content of object in the jQuery prototype to provide new jQuery instance methods.
The jQuery.fn.extend() method extends the jQuery prototype ($.fn) object to provide new methods that can be chained to the jQuery() function.

{% highlight js %}
jQuery.fn.extend({
  check: function() {
    return this.each(function() {
      this.checked = true;
    });
  },
  uncheck: function() {
    return this.each(function() {
      this.checked = false;
    });
  }
});
 
// Use the newly created .check() method
$( "input[type='checkbox']" ).check();
{% endhighlight %}

If we use jQuery.extend for extending the feature then we don't need a selector for calling this function, 
like in the case of calling $.ajax(). But when we use jQuery.fn.extend then we need to have a selector for calling the function. 
Explained with the help of example:

{% highlight js %}
jQuery.extend({
    abc: function(){
        alert('abc');
    }
});
{% endhighlight %}
usage: $.abc(). (No selector required like $.ajax().)

{% highlight js %}
jQuery.fn.extend({
    xyz: function(){
        alert('xyz');
    }
});
{% endhighlight %}
usage: $('.selector').xyz(). (Selector required like $('#button').click().)



