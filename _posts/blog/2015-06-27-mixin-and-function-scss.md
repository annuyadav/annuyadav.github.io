---
layout: post
title: "Mixins and  functions in sass"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-06-27T13:52:00+0530
comments: true
---

In sass we can add our logic for calculation which is not possible in CSS. By the help of mixins and functions in sass 
we can add reusable logic.

Sass has mixins, we all know that. Instead of outputting lines of Sass the way mixins do, functions return a value. 
This makes them a super powerful behind-the-scenes player in Sass recipes.

Functions and mixins are very similar in nature. Because they can both accept variables. In the following examples, we 
review the creation, usage and output of both a mixin and a function.


### Mixin:
The following mixin can accept arguments and do calculations. This will be available where ever you @include it.

{% highlight css %}
@mixin font-size-mixin($size-value) {
  font-size: $size-value;
}
{% endhighlight %}

use @include directive add mixin's code in some class:

{% highlight css %}
.summary-para {
  @include font-size-mixin(12px);
}
{% endhighlight %}

The final outcome of above code is:

{% highlight css %}
.summary-para {
  font-size: 12px;
}
{% endhighlight %}


### Function:
Function is similar to mixin, however the output from a function is single value.
The following function will accept 2 arguments and will return sum of both arguments.

{% highlight css %}
@function sum-function($first-value, $second-value){
  @return $first-value + $second-value
}
{% endhighlight %}

Now instead of using @include we simply call this method.

{% highlight css %}
.combination-para {
  padding: sum-function(5px, 7px);
}
{% endhighlight %}

and the out put will be:

{% highlight css %}
.combination-para {
  padding: 12px;
}
{% endhighlight %}
