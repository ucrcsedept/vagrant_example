# vagrant_example
The project is a beginners guide to vagrant. The project will walk you through the vagrant and show you the major features it affords with some examples.
Vagrant is a tool that allows you to easily create and configure lightweight, reproducible and portable virtual environments. Developers often use vagrant as a test development environment, which has the same configurations as the production server. Vagrant isolates dependencies on operating systems, tools and specific software versions, development team use vagrant to create their consistent development environment. Vagrant is much more than these.


######Install vagarnt:
1.	Go to the [vagrant downloads page](https://www.vagrantup.com/downloads.html) and get the appropriate installer for your platform, then install it.  
2.	By default, VirtualBox is the default provider for Vagrant. Vagrant can also use VMware, AWS, or [any other provider](https://www.vagrantup.com/docs/providers/).  The project use virtualbox, so go to the [Virtualbox downloads page](https://www.virtualbox.org/wiki/Downloads) and get the appropriate version, then install it. 


######Get started:
1. Some concepts before we start up vagrant: 
 - Boxes: Boxes are vagrant packages running a specific operating system. These packages can be imported on any machine that runs vagrant.  In the project, we will use “centos/7”.
 - Vagrantfile: vagrantfile is a primary configuration file of vagrant. It is used to define virtual machine you will use, how to configure and provision it. The vagrantfile is written using Ruby syntax and looks like:
```ruby
Vagrant.configure("2") do |config|     
	config.vm.hostname= "test"
 	config.vm.box = "centos/7"     
	config.vm.network :private_network, ip: "192.168.0.101" 
	config.vm.synced_folder "./", "/vagrant" end
```

2. Up and running:
 - Create project and a vagrantfile
```
$mkdir vagrant_example
$cd vagrant_example
$vagrant init centos/7
```
Now a vagrantfile is created in current directory. The vagrantfile looks like:
```ruby
	# -*- mode: ruby -*-
	# vi: set ft=ruby :
	# All Vagrant configuration is done below. The "2" in Vagrant.configure
	# configures the configuration version (we support older styles for
	# backwards compatibility). Please don't change it unless you know what
	# you're doing.
	Vagrant.configure("2") do |config|
	  # The most common configuration options are documented and commented below.
	  # For a complete reference, please see the online documentation at
	  # https://docs.vagrantup.com.

	  # Every Vagrant development environment requires a box. You can search for
	  # boxes at https://atlas.hashicorp.com/search.
	  config.vm.box = "centos/7"
```
about 1.8.5 patch here:---

You will have a fully running virtual machine in VirtualBox running centos/7. you can start it up using:
```
$vagrant up
```

