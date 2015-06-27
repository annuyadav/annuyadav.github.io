---
layout: post
title: "Nginx subdomain rewrite"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-06-19T13:52:00+0530
comments: true
---

To redirect all http requests to https:

{% highlight bash %}
server {
    listen 80;
    rewrite ^/(.*) https://$host$request_uri permanent;
}
{% endhighlight %}


to remove sub domain from request and redirect to domain:

{% highlight bash %}
server {
  if ($host ~* (.*)\.(domain\.com)){
      set $host_without_subdomain $2;
      rewrite ^(.*)$ https://$host_without_subdomain$1 permanent;
  }
}
{% endhighlight %}
