---
layout: post
title: "Moving all files in a folder from one bucket to other in ASW S3 in ruby"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-07-04T12:20:59+0530
comments: true
---

Their is no option like cut and paste is available in aws s3. So for moving a folder from one bucket to other first 
copy the files from source path to destination and then destroy the source folder or file.

First create a new bucket where the folder with files need to be copied.

{% highlight ruby %}
s3_client = Aws::S3::Client.new(
  access_key_id: 'your_access_key_id',
  secret_access_key: 'your_secret_access_key'
)
{% endhighlight %}

{% highlight ruby %}
s3_client.create_bucket({
  bucket: 'DestinationBucketName'
})
{% endhighlight %}


then copy the files from previous bucket to new bucket by:

{% highlight ruby %}
s3_client.list_objects({bucket: 'SourceBucketName'}).each do |obj|
   s3_client.copy_object({bucket: 'DestinationBucketName', copy_source: "/#{obj.key}", key: "DestinationBucketName/#{obj.key}"})
end
{% endhighlight %}

s3_client.list_objects will return only 1000 objects. So if you have more than 1000 files then repeate the process until all files are copied.

After copying the files to destination path destroy the old bucket by first destroying all objects of bucket followed by destroying bucket.

{% highlight ruby %}
obj_arr = s3_client.list_objects({bucket: bucket_name}).contents.collect{|obj| {key: obj.key}}
s3_client.delete_objects({bucket: 'SourceBucketName', delete: {objects: obj_arr, quiet: true}})
{% endhighlight %} 
  
Destroy bucket:

{% highlight ruby %}
s3_client.delete_bucket({
  bucket: 'SourceBucketName'
})
{% endhighlight %} 
