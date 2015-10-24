---
layout: post
title: "Create AWS EC2 instance in rails"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-10-17T12:16:00+0530
comments: true
---

New instance can be added in AWS at run time by the following method:

{% highlight ruby %}
def aws_credentials
  @credentials ||= {access_key_id: 'access_key_id',
                    secret_access_key: 'secret_access_key',
                    region: 'region'}
end

def ec2_client
  @ec2 ||= Aws::EC2::Client.new(aws_credentials)
end
  
def ec2_resource
  Aws::EC2::Resource.new(client: ec2_client)
end
  
def create_aws_instance
  _instance = ec2_resource.create_instances({image_id: "image_id",
                                             min_count: "number_of_instance",
                                             max_count: "number_of_instance",
                                             subnet_id: "subnet_id",
                                             instance_type: "instance_type",
                                             security_group_ids: ["security_group_id"],
                                             key_name: "key_name"}).first
  puts "----------  new instance created with id:  #{_instance.id}"
  puts "----------  instance ip address is:  #{_instance.private_ip_address}"
end
{% endhighlight %}

Here image_id is the Aws::EC2::Image id from which the new instance is to be created (ex. ami-9bddcfab). number_of_instance 
is the number of new instance to be created, subnet_id is the id of subnet in which this instance will be created (ex. subnet-4b8cc82e), 
instance_type is the type of machine(ex. t2.small, t2 medium etc), security_group_ids is the array of security_groups you 
want to add to this machine. key_name is the key pair which you want to assign to the machine.

To add name to this newly created instance:

{% highlight ruby %}
 def add_name_to_instance(instance_id)
    _instance = ec2_resource.instance(instance_id)
    _name = "my_new_instance"
    _instance.create_tags({tags: [{
                                      key: 'Name',
                                      value: _name,
                                 }]
                          })
 end
{% endhighlight %}

Now if you want to terminate this instance than:

{% highlight ruby %}
def terminate_aws_instance(instance_id)
    instance = ec2_resource.instance(instance_id)
    instance.terminate.terminating_instances.first.current_state.code == 32
end
{% endhighlight %}

If you want to list all instances created from a given image:

{% highlight ruby %}

def available_instances(image_id)
    instances = ec2_resource.instances({
                                               filters: [
                                                   {
                                                       name: 'image-id',
                                                       values: [image_id]
                                                   },
                                                   {
                                                       name: 'instance-state-name',
                                                       values: ['running']
                                                   }
                                               ]
                                        })
    instances.count
end
{% endhighlight %}
