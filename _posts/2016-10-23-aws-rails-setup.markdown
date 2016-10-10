---
layout: post
title:  "Rails Applictiona on AWS with Terraform and Packer."
date:   2016-09-22 23:01:37 +0000
categories: aws terraform ruby rails
---

We're going to run through building the AWS setup to run a production Rails application. We'll break this down into a number of steps:

* Create Rails application server AMI using Packer
* Setup our VPC
* Setup a load balancer and nodes distributed across availability zones
* Setup the database across availability zones
* Deploy our code using Capistrano
* Setup centralized logging

Here is some new content.

And some more.

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
