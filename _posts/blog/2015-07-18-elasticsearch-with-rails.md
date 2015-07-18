---
layout: post
title: "Elasticsearch with ruby on rails."
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-07-18T13:10:59+0530
comments: true
---

Elasticsearch is a full-text search engine with a RESTful web interface based on Lucene. Elasticsearch is developed in 
Java and is released as open source under the terms of the Apache License.

Elastic search installation is covered in my previous [blog](/blog/installing-elasticsearch-in-ubuntu).


Elastic search in rails is split into three separate gems:

* ```elasticsearch-model```, which contains search integration for Ruby/Rails models such as ActiveRecord::Base and Mongoid
* ```elasticsearch-persistence```, which provides a standalone persistence layer for Ruby/Rails objects and models
* ```elasticsearch-rails```, which contains various features for Ruby on Rails applications

By default, our Rails app will connect to Elasticsearch at localhost:9200, but if you need to connect to a different 
server, uses the config/elasticsearch.yml file to overwrite the defaults.

{% highlight yaml %}
config = {
  host: "http://localhost:9800/"
}

if File.exists?("config/elasticsearch.yml")
  config.merge!(YAML.load_file("config/elasticsearch.yml").symbolize_keys)
end

Elasticsearch::Model.client = Elasticsearch::Client.new(config)
{% endhighlight %}

First include elasticsearch in your model, so that elasticsearch methods and callbacks can be used in model.
 
{% highlight ruby %}
class Merchant < ActiveRecord::Base
  include Elasticsearch::Model
  include Elasticsearch::Model::Callbacks
end
{% endhighlight %}

in console execute the code:  
{% highlight console %}
2.1.5 :001 > Merchant.search('ram').results.count
=> 2
{% endhighlight %}

Before searching we have to call a method import on the table we want to search, because initially our search index is 
empty. By importing all changes will be added to index automatically. So to populate data we need to execute:

{% highlight console %}
Merchant.import
{% endhighlight %}

By deafult all columns are looked up in search. But these can be restricted by:

{% highlight ruby %}
class Merchant < ActiveRecord::Base
 def as_indexed_json(options={})
    as_json(
        only: [:name, :about, :price, :gender]
     )
  end
end
{% endhighlight %}

When Elasticsearch indexes our Merchant object, we are telling it that we only want to search the name, about, price and gender attribute.

Their are two methods of getting the result.

{% highlight ruby %}
Merchant.search('ram').results.count
Merchant.search('ram').records.count
{% endhighlight %}

records method will return collection of ActiveRecord objects and results will return collection Elasticsearch objects.

Now lets discuss the case of associations. Let's have a model opening for merchants:

{% highlight ruby %}
class Opening < ActiveRecord::Base
   belongs_to :merchant
end
{% endhighlight %}

{% highlight ruby %}
class Merchant < ActiveRecord::Base
  include Elasticsearch::Model
  include Elasticsearch::Model::Callbacks

  has_many :openings
end
{% endhighlight %}

For adding openings in search index lets add that in as_indexed_json method of Merchant

{% highlight ruby %}
class Merchant < ActiveRecord::Base
  include Elasticsearch::Model
  include Elasticsearch::Model::Callbacks

  has_many :openings

  def as_indexed_json(options={})
    as_json(
      only: [:name, :about, :price, :gender],
      include: [:openings]
    )
  end
end
{% endhighlight %}

Now all fields of opening models will also be added in index. To add specific column of association modify the method as:

{% highlight ruby %}
 def as_indexed_json(options={})
    as_json(
      only: [:name, :about, :price, :gender],
      include: {openings: {only: [:name, :section]}}
    )
  end
{% endhighlight %}
  
Now try fetch the method in console:
{% highlight console %}
> rails c
2.1.5 :001 > Merchant.first.as_indexed_json
=> {"name"=>"Annu", "about"=>"Software engineer", "gender"=>1, "price"=>100.0, "openings"=>[{"name"=>"tution", "section"=>"C"}, {"name"=>"seminar", "section"=>"java"}]} 
{% endhighlight %}

Now searching can be made for openings fields also.
 To make methods as searchable alter the as_indexed_json method as:
 
{% highlight ruby %}
 def as_indexed_json(options={})
    as_json(
      only: [:name, :about, :price, :gender],
      methods: [:availabilities, :specializations_id],
      include: {openings: {only: [:name, :section]}}
    )
  end
{% endhighlight %}

