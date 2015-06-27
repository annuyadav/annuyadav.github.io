---
layout: post
title: "Creating has_and_belongs_to_many association in rails"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-01-31T10:20:00+0530
comments: true
---

Let's have two models having has and belongs to many associations.

{% highlight ruby %}
class Contact < ActiveRecord::Base
    has_and_belongs_to_many :phones
end
{% endhighlight %}

and

{% highlight ruby %}
class Phone < ActiveRecord::Base
    has_and_belongs_to_many :contacts
end
{% endhighlight %}

Now migrations will be written as
{% highlight ruby %}
class CreateContacts < ActiveRecord::Migration
   def change
     create_table :contacts do |t|
       t.string :name
       t.string :email
       t.timestamps
     end
   end
end
{% endhighlight %}


{% highlight ruby %}
class CreatePhones < ActiveRecord::Migration
   def change
     create_table :phones do |t|
       t.string :number
       t.timestamps 
     end
   end
end
{% endhighlight %}

{% highlight ruby %}
class CreateJoinTableContactPhone < ActiveRecord::Migration
  def change 
    create_join_table :contacts, :phones, column_options: {null: true} do |t|
    end 
  end
end
{% endhighlight %}
