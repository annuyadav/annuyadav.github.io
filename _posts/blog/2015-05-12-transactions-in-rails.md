---
layout: post
title: "Transactions in rails"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-05-12T11:06:00+0530
comments: true
---

We use transactions as a protective wrapper around SQL statements to ensure changes to the database only occur when all
actions succeed together. An example of transactions is the banking method where funds are withdrawn from one account and deposited into the next. If either of these steps fail, then the entire process should be reset.
Example:

{% highlight ruby %}
ActiveRecord::Base.transaction do
   rohit.withdrawal(100)
   rita.deposit(100)
end
{% endhighlight %}

In rails transactions are available for both class and instance levels. An example is as follows:


{% highlight ruby %}
User.transaction do
   @user.create!
   @user.company(true).first.destroy!
   Product.first.destroy!
end
@user.transaction do
   @client.users.create!
   @user.company(true).first.destroy!
   Product.first.destroy!
end
{% endhighlight %}

Transaction will rollback the thing only when exception occurs else it will not be handled. In rails many time exception will not be thrown because the code will return nil or false instead of raising an exception. An example situation is:

{% highlight ruby %}
    rohit.update_attribute(:amount, 1000)
{% endhighlight %}    

this will return false if unable to update the attribute. So we should use the code which will raise an exception in transaction block. Instead of using update_attribute we can use update_attribute! which will throw an exception upon failure.

{% highlight ruby %}
    rohit.update_attribute!(:amount, 1000)
{% endhighlight %}   


While updating or performing some action on an object instead of checking for its presence and proceeding for updates throw an exception of RecordNotFound so that it can rollback the complete things. example:

{% highlight ruby %}
ActiveRecord::Base.transaction do
   user = User.where(name: "rohit").first
   if(user.id != rita.id)
      user.update_attributes!(:amount => -100)
      rita.update_attributes!(:amount => 100)
   end
end
{% endhighlight %}

in this case nothing will be rollback because even if user is nil then also it will have an id attribute so it will not cause any error in transaction. So we can raise an exception here:

{% highlight ruby %}
ActiveRecord::Base.transaction do
   user = User.where(name: "rohit").first
   raise ActiveRecord::RecordNotFound if user.nil?
   if(user.id != rita.id)
      user.update_attributes!(:amount => -100)
      rita.update_attributes!(:amount => 100)
   end
end
{% endhighlight %}
