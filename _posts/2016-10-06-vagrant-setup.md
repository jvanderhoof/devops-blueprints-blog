---
layout: post
title:  "Rails Appliction Setup with Vagrant."
date:   2016-10-06 01:01:37 +0000
categories: vagrant ruby rails
---

# Running a Rails Application on Vagrant

## Introduction

We're going to run through building a Rails application server on Vagrant.  For this, we'll be installing:

* Ruby 2.3.1 from source
* Unicorn as an application server
* Nginx for 



First, install Vagrant:

{% highlight bash %}
$ brew update
# if you don't have cask installed, install it
$ brew tap caskroom/cask
$ brew cask install vagrant
{% endhighlight %}

Or install from the available Hashicorp packages: [Download Vagrant](https://www.vagrantup.com/downloads.html)

Step into the root of your project, and create a new Vagrant configuration file:

{% highlight bash %}
$ vagrant init
{% endhighlight %}

Now let's download a VM to use.  In this case, we're going to use Ubuntu 14.04
{% highlight bash %}
$ vagrant box add ubuntu/trusty64
{% endhighlight %}

This will take some time depending on your download speed.

Now, let's setup our Vagrant machine.  In your favorite editor, open `Vagrantfile`.  Find the following line:

{% highlight ruby %}
config.vm.box = "base"
{% endhighlight %}

and replace it with:

{% highlight ruby %}
config.vm.box = "config.vm.box = "ubuntu/trusty64""
{% endhighlight %}

This insures Vagrant will load the Ubuntu 14.04 virtual machine we just downloaded.

Now, uncomment the following line:
{% highlight ruby %}
# config.vm.network "private_network", ip: "192.168.33.10"
{% endhighlight %}

so it reads as follows.  Optionally, you can change the IP address as you wish:
{% highlight ruby %}
config.vm.network "private_network", ip: "192.168.33.10"
{% endhighlight %}

Now let's build ourselves a virtual machine that can run a Ruby application. You can use a variety of tools to provision your virtual machine.  I'm a big fan of straight up shell scripts. It forces simple server setup. It's better to have a couple of simple components rather than one large, complex component.  Within the Vagrant configure block, add the following:

{% highlight ruby %}
config.vm.provision "shell", inline: <<-SHELL
  # run updates
  apt-get update

  # add a bunch of dependencies
  apt-get install -y git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev

  # add Postgres repo
  sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
  wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

  # install postgres client
  apt-get install -y postgresql-client

  # download and compile Ruby
  mkdir /opt/src
  cd /opt/src && curl -O https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.1.tar.gz && tar -xvzf ruby-*
  cd /opt/src/ruby-* && ./configure && make && make install

  # install Bundler
  gem install bundler --no-rdoc --no-ri
SHELL
{% endhighlight %}

Now let's build ourselves a VM:

{% highlight bash %}
$ vagrant up --provision
{% endhighlight %}

The provision flag is re-runs the configuration block if the machine has already been initially provisioned.
