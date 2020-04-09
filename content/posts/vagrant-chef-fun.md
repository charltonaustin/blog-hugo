---
title: Vagrant Chef Fun
date: 2013-02-27T09:03:54-04:00
draft: false
---
A couple of days ago I got a chance to see some friends of mine (Matt Urbanski and Jinwoo Baek) give a presentation on chef. It was fantastic. Afterward we had a sort of impromptu demo session on using Vagrant and Chef to get an apache server up and running. So this is going to be a quick tutorial on how to use Vagrant and Chef to get a functioning apache server up and running on a vm.


#### Things you are going to need (or I expect to be installed):

1. Ruby 2.x.x or 1.9.x 
1. Git 
1. xcode and xcode command line tools (if you have mac os)
1. Virtual box

I would suggest using [rvm](https://rvm.io/) to manage Ruby. [Here](http://stackoverflow.com/questions/3696564/how-to-update-ruby-to-1-9-x-on-mac) is a post to help with the install.
Download and install [virtual box](https://www.virtualbox.org/wiki/Downloads).

Now make a test directory and cd into it.

```bash
$ mkdir test && cd test 
```


Install vagrant

```bash
$ gem install vagrant
```


Create a new vagrant file.

```bash
$ vagrant init
```

Next open up the vagrant file and make it look like this:


```vagrant 
  # Every Vagrant virtual environment requires a box to build off of.

  config.vm.box = "ubuntu"



  # Forward a port from the guest to the host, which allows for outside

  # computers to access the VM, whereas host only networking does not.

  config.vm.forward_port 80, 8080





  # Enable provisioning with chef solo, specifying a cookbooks path, roles

  # path, and data_bags path (all relative to this Vagrantfile), and adding 

  # some recipes and/or roles.

  #

  config.vm.provision :chef_solo do |chef|

     chef.cookbooks_path = "./cookbooks"

     chef.add_recipe "apache2::default"

  end

end
```


Next download a public base box with Ubuntu on it.

```bash
$ vagrant box add ubuntu http://files.vagrantup.com/lucid64.box  
```
  
Now clone a copy of the apache2 opscode cookbook into a directory for cookbooks.    

```bash
$ mkdir cookbooks && cd cookbooks 
$ git clone https://github.com/opscode-cookbooks/apache2.git
```

Okay next you need to modify some of the source code for the cookbooks.

```bash
vim recipes/default.rb 
```


Then add this to the default.rb file just below the comments. (Because the version of ubuntu I downloaded needed an apt-get update in order for the package install to work.)

```vagrant
bash "apt-get update" do
  user "root"
  code <<-EOH
  sudo apt-get update
  EOH
end
```

Finally use vagrant to start the server up.

```bash
$ vagrant up
```

Now we should be able to hit the url in your browser simply hit localhost:8080.  This should give you a 404 from Apache Server at localhost port 8080.

I think it is nothing short of amazing how easy it is to get a virtual machine up and running with an Apache server.