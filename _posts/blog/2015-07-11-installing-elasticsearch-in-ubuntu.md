---
layout: post
title: "Install Elasticsearch in Ubuntu"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-07-11T13:10:59+0530
comments: true
---

Elasticsearch require Java, so first install that. You can install java with the help of Synaptic Package Manager or by:

Add Java PPA to apt:
{% highlight bash %}
sudo add-apt-repository -y ppa:webupd8team/java
{% endhighlight %}

Update packages:
{% highlight bash %}
sudo apt-get update
{% endhighlight %}

And then install java:
{% highlight bash %}
sudo apt-get -y install oracle-java8-installer
{% endhighlight %}

Now java is available on your system, So lets proceed for installing Elasticsearch

Import the Elasticsearch public GPG key into apt:
{% highlight bash %}
wget -O - http://packages.elasticsearch.org/GPG-KEY-elasticsearch | sudo apt-key add -
{% endhighlight %}

It may ask for password for executing sudo command.

Create the Elasticsearch source list:
{% highlight bash %}
echo 'deb http://packages.elasticsearch.org/elasticsearch/1.4/debian stable main' | sudo tee /etc/apt/sources.list.d/elasticsearch.list
{% endhighlight %}

Update packages:
{% highlight bash %}
sudo apt-get update
{% endhighlight %}

And then install Elasticsearch:
{% highlight bash %}
sudo apt-get -y install elasticsearch=1.4.4
{% endhighlight %}


Now Elasticsearch is installed. To restrict the access only to local system add a line to your configuration file elasticsearch.yml.

{% highlight bash %}
sudo vi /etc/elasticsearch/elasticsearch.yml
{% endhighlight %}
 
and then uncomment the network.host option and replace it with:
 
 {% highlight bash %}
 network.host: localhost
 {% endhighlight %}
 
Start elasticsearch by:
{% highlight bash %}
 sudo service elasticsearch start
 {% endhighlight %}

If you want to start elasticsearch on bootup then execute:
{% highlight bash %}
sudo update-rc.d elasticsearch defaults 95 10
{% endhighlight %}

