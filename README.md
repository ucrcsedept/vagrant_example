# vagrant_example
The project is a beginner's guide to vagrant. The project will walk you through the vagrant and show you the major features it offers with some examples.
Vagrant is a tool that allows you to easily create and configure lightweight, reproducible and portable virtual environments. As a developer, you can use vagrant to create a virtual environment for develepment and testing, which is completely isolated from the host execution environment including operating system, runtime libaries, configurations, etc. Using vagrant, multiple developers in a development team can easily make sure that their development and testing environments are consistent. When the software product is ready, this virtual environment can then be easily deployed on the production server. Certainly, vagrant is much more than these.


###Install vagrant:
1. Go to the [vagrant downloads page](https://www.vagrantup.com/downloads.html) and get the installer for your platform, then install it. 
2. VirtualBox is the default provider for Vagrant. Vagrant can also use VMware, AWS, or [any other provider](https://www.vagrantup.com/docs/providers/).  This project uses VirtualBox, so go to the [Virtualbox downloads page](https://www.virtualbox.org/wiki/Downloads) and get an appropriate version for your platform, then install it. 


###Get started:
1. Some concepts before you start up vagrant: 
 * Boxes: Boxes are vagrant packages running a specific operating system. These packages can be imported on any machine that runs vagrant.  In this project, we will use CentOS/7.
 * Vagrantfile: vagrantfile is a primary configuration file of vagrant. It is used to define what virtual machine you will use, and how to configure and provision it. The vagrantfile is written using Ruby syntax. It looks like:
 
   ```ruby
   Vagrant.configure("2") do |config|     
	  config.vm.hostname= "test"
 	  config.vm.box = "centos/7"     
	  config.vm.network :private_network, ip: "192.168.0.101" 
	  config.vm.synced_folder "./", "/vagrant" end
   ```
2. Up and running:
 * Create project and a vagrantfile:
   
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
 * About 1.8.5 patch:
  
   Vagrant 1.8.5 has a bug that will not be fixed until Vagrant 1.8.6. You need to manually patch /opt/vagrant/embedded/gems/gems/vagrant-1.8.5/plugins/guests/linux/cap/public_key.rb as following:
   
    ```
      if test -f ~/.ssh/authorized_keys; then
               grep -v -x -f '#{remote_path}' ~/.ssh/authorized_keys > ~/.ssh/authorized_keys.tmp
               mv ~/.ssh/authorized_keys.tmp ~/.ssh/authorized_keys
               chmod 0600 ~/.ssh/authorized_keys
      fi
   ```  
   For further details, please go [here](https://github.com/mitchellh/vagrant/issues/7610#issuecomment-234019846)
 * Start up:
 
   You can start up the virtual machine using:
   
    ```
   $vagrant up
	Bringing machine 'default' up with 'virtualbox' provider...
	==> default: Importing base box 'centos/7'...
	==> default: Matching MAC address for NAT networking...
	==> default: Checking if box 'centos/7' is up to date...
	==> default: A newer version of the box 'centos/7' is available! You currently
	==> default: have version '1608.02'. The latest is version '1609.01'. Run
	==> default: `vagrant box update` to update.
	==> default: Setting the name of the VM: vagrant_example_default_1476074625803_59797
	==> default: Clearing any previously set network interfaces...
	==> default: Preparing network interfaces based on configuration...
	    default: Adapter 1: nat
	==> default: Forwarding ports...
	    default: 22 (guest) => 2222 (host) (adapter 1)
	==> default: Booting VM...
	==> default: Waiting for machine to boot. This may take a few minutes...
	    default: SSH address: 127.0.0.1:2222
	    default: SSH username: vagrant
	    default: SSH auth method: private key
	    default: Warning: Remote connection disconnect. Retrying...
	    default: 
	    default: Vagrant insecure key detected. Vagrant will automatically replace
	    default: this with a newly generated keypair for better security.
	    default: 
	    default: Inserting generated public key within guest...
	    default: Removing insecure key from the guest if it's present...
	    default: Key inserted! Disconnecting and reconnecting using new SSH key...
	==> default: Machine booted and ready!
    ```

3. Features:

    + Vagrant runs the virtual machine without a UI. You can SSH into the machine:
   
       ```
       $vagrant ssh
       [vagrant@localhost ~]$ 
       ```   
     Show the state of the machine:
   
     ```
     $vagrant status
     Current machine states:
     default                   running (virtualbox)
     The VM is running. To stop this VM, you can run `vagrant halt` to
     shut it down forcefully, or you can run `vagrant suspend` to simply
     suspend the virtual machine. In either case, to restart it again,
     simply run `vagrant up`.
     ```
     Record a point-in-time state of a guest machine. You can then quickly restore to this environment:
   
      ```
      $vagrant snapshot
      ```   
   Synced folders enable Vagrant to sync a folder on the host machine to the guest machine. Synced folders are configured within your Vagrantfile using the config.vm.synced_folder method.
   
      ```ruby
      Vagrant.configure("2") do |config|
        config.vm.box = "centos/7"
        config.vm.synced_folder "./synced_folder", "/synced_folder", type: "rsync"
      ```
   Synced folders are automatically setup during vagrant up and vagrant reload:
   
      ```
      $vagrant reload
      ==> default: Attempting graceful shutdown of VM...
      ==> default: Checking if box 'centos/7' is up to date...
      ==> default: A newer version of the box 'centos/7' is available! You currently
      ==> default: have version '1608.02'. The latest is version '1609.01'. Run
      ==> default: `vagrant box update` to update.
      ==> default: Clearing any previously set forwarded ports...
      ```
   Vagrant can use rsync as a mechanism to sync a folder to the guest machine. This command forces a re-sync of any rsync synced folders. 
   
      ```
      $vagrant rsync
      ```
   Shut down the running machine:
   
      ```
      $vagrant halt
      ```
   Stop the running machine and destroy all resources that were created:
     
      ```
      $vagrant destroy
      ```

   
      
    
    





