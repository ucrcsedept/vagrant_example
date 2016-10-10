# vagrant_example
The project is a beginner's guide to vagrant. The project will walk you through the vagrant and show you the major features it offers with some examples.
Vagrant is a tool that allows you to easily create and configure lightweight, reproducible and portable virtual environments. As a developer, you can use vagrant to create a virtual environment for develepment and testing, which is completely isolated from the host execution environment including operating system, runtime libaries, configurations, etc. Using vagrant, multiple developers in a development team can easily make sure that their development and testing environments are consistent. When the software product is ready, this virtual environment can then be easily deployed on the production server. Certainly, vagrant is much more than these.


###Install vagrant:
1. Go to the [vagrant downloads page](https://www.vagrantup.com/downloads.html) and get the installer for your platform, then install it.  
2. VirtualBox is the default provider for Vagrant. Vagrant can also use VMware, AWS, or [any other provider](https://www.vagrantup.com/docs/providers/).  This project uses VirtualBox, so go to the [Virtualbox downloads page](https://www.virtualbox.org/wiki/Downloads) and get an appropriate version for your platform, then install it. 


###Get started:
1. Some concepts before we start up vagrant: 
  - Boxes: Boxes are vagrant packages running a specific operating system. These packages can be imported on any machine that runs vagrant.  In this project, we will use CentOS/7.
  - Vagrantfile: vagrantfile is a primary configuration file of vagrant. It is used to define what virtual machine you will use, and how to configure and provision it. The vagrantfile is written using Ruby syntax. It looks like:
  ```ruby
Vagrant.configure("2") do |config|     
	config.vm.hostname= "test"
 	config.vm.box = "centos/7"     
	config.vm.network :private_network, ip: "192.168.0.101" 
	config.vm.synced_folder "./", "/vagrant" end
  ```
2. Up and running:
  - Create project and a vagrantfile:
  ```
$mkdir vagrant_example
$cd vagrant_example
$vagrant init centos/7
  ```
  Now a vagrantfile is created in the current directory. You will have a virtual machine running CentOS/7 inside VirtualBox. The vagrantfile looks like:
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
  - About 1.8.5 patch here:  
  - Start up:
   
  ```
$vagrant up
  ```

