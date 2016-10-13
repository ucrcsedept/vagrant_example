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
    - Show the state of the machine:
   
     ```
     $vagrant status
     Current machine states:
     default                   running (virtualbox)
     The VM is running. To stop this VM, you can run `vagrant halt` to
     shut it down forcefully, or you can run `vagrant suspend` to simply
     suspend the virtual machine. In either case, to restart it again,
     simply run `vagrant up`.
     ```
    - Record a point-in-time state of a guest machine. You can then quickly restore to this environment. The command has some subcommands: save, restore, list, delete and more.
   
      ```
      # current state of the guest machine:
      $[vagrant@localhost ~]$ ls -l
	total 0
	-rw-rw-r--. 1 vagrant vagrant 0 Oct 13 15:59 snapshotfile
      
      # save current state: 
      $ vagrant snapshot save test_snapshot
	==> default: Snapshotting the machine as 'test_snapshot'...
	==> default: Snapshot saved! You can restore the snapshot at any time by
	==> default: using `vagrant snapshot restore`. You can delete it using
	==> default: `vagrant snapshot delete`
      
      # make some changes in the guest machine:
      [vagrant@localhost ~]$ rm snapshotfile 
      [vagrant@localhost ~]$ ls -l
      total 0

      #restore the snapshot:
      $ vagrant snapshot restore test_snapshot
	==> default: Restoring the snapshot 'test_snapshot'...
	==> default: Checking if box 'centos/7' is up to date...
	==> default: A newer version of the box 'centos/7' is available! You currently
	==> default: have version '1608.02'. The latest is version '1609.01'. Run
	==> default: `vagrant box update` to update.
	==> default: Resuming suspended VM...
	==> default: Booting VM...
	==> default: Waiting for machine to boot. This may take a few minutes...
        default: SSH address: 127.0.0.1:2222
        default: SSH username: vagrant
        default: SSH auth method: private key
        default: Warning: Connection refused. Retrying...
	==> default: Machine booted and ready!
	==> default: Running provisioner: shell...
        default: Running: /var/folders/m6/xrjv0s1d4lvdj6f3wnswrqhc0000gp/T/vagrant-shell20161013-45182-bg5c81.sh
	==> default: Loaded plugins: fastestmirror
	==> default: Loading mirror speeds from cached hostfile
	==> default:  * base: ftpmirror.your.org
	==> default:  * extras: lug.mtu.edu
	==> default:  * updates: repos.dfw.quadranet.com
	==> default: Package httpd-2.4.6-40.el7.centos.4.x86_64 already installed and latest version
	==> default: Nothing to do
      $ vagrant ssh
      [vagrant@localhost ~]$ ls -l
       total 0
      -rw-rw-r--. 1 vagrant vagrant 0 Oct 13 15:59 snapshotfile
      
      #list all the snapshots taken:
      $ vagrant snapshot list
	test_snapshot

      #delete a snapshot:
      $vagrant snapshot delete test_snapshot
      ==> default: Deleting the snapshot 'test_snapshot'...
      ==> default: Snapshot deleted!
      
      ```   
    - Synced folders enable Vagrant to sync a folder on the host machine to the guest machine. Synced folders are configured within your Vagrantfile using the config.vm.synced_folder method.
   
      ```ruby
      Vagrant.configure("2") do |config|
        config.vm.box = "centos/7"
	# Share an additional folder to the guest VM. The first argument is
        # the path on the host to the actual folder. The second argument is
        # the path on the guest to mount the folder. And the optional third
        # argument is a set of non-required options.
        config.vm.synced_folder "./synced_folder", "/synced_folder", type: "rsync"
      ```
   - Synced folders are automatically setup during vagrant up and vagrant reload:
   
      ```
      $vagrant reload
      ==> default: Attempting graceful shutdown of VM...
      ==> default: Checking if box 'centos/7' is up to date...
      ==> default: A newer version of the box 'centos/7' is available! You currently
      ==> default: have version '1608.02'. The latest is version '1609.01'. Run
      ==> default: `vagrant box update` to update.
      ==> default: Clearing any previously set forwarded ports...
      ==> default: Preparing network interfaces based on configuration...
      ==> default: Adapter 1: nat
      ==> default: Forwarding ports...
      default: 22 (guest) => 2222 (host) (adapter 1)
     ==> default: Booting VM...
     ==> default: Waiting for machine to boot. This may take a few minutes...
         default: SSH address: 127.0.0.1:2222
    	 default: SSH username: vagrant
    	 default: SSH auth method: private key
    	 default: Warning: Remote connection disconnect. Retrying...
     ==> default: Machine booted and ready!
	[default] GuestAdditions 4.3.24 running --- OK.
     ==> default: Checking for guest additions in VM...
     ==> default: Rsyncing folder: /Users/qcheng/vagrant/fork/vagrant_example/ => /vagrant
     ==> default: Rsyncing folder: /Users/qcheng/vagrant/fork/vagrant_example/synced_folder/ => /synced_folder
     ==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
     ==> default: flag to force provisioning. Provisioners marked to run always will still run.
      ```
   - Vagrant can use rsync as a mechanism to sync a folder to the guest machine. This command forces a re-sync of any rsync synced folders. We create a test file in the host machine and rsync to the guest machine.
   
      ```
      $ cd synced_folder/
      $ touch test
      $ vagrant rsync
      ==> default: Rsyncing folder: /Users/qcheng/vagrant/fork/vagrant_example/ => /vagrant
      ==> default: Rsyncing folder: /Users/qcheng/vagrant/fork/vagrant_example/synced_folder/ => /synced_folder
      $ vagrant ssh
      $ cd /synced_folder/
      [vagrant@localhost synced_folder]$ ls
      test
      ```
   - All networks are configured within your Vagrantfile using the config.vm.network method call. You can access the service on port(80) of the guest machine via the port(8080) of the host machine.
   
      ```ruby
 	 Vagrant.configure("2") do |config|
      	   config.vm.box = "centos/7"
	   config.vm.synced_folder "./synced_folder", "/synced_folder", type: "rsync"
  	   # Create a forwarded port mapping which allows access to a specific port
  	   # within the machine from a port on the host machine. In the example below,
  	   # accessing "localhost:8080" will access port 80 on the guest machine.
  	   config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
      ```
   - Provisioners in Vagrant allow you to automatically install software, alter configurations, and more on the machine as part of the vagrant up process. All provisioner are configured within your Vagrantfile using the config.vm.provision method call. The provisioner can use file, shell script, ansible and more.
   
      ```ruby
      Vagrant.configure("2") do |config|
      	   config.vm.box = "centos/7"
	   config.vm.synced_folder "./synced_folder", "/synced_folder", type: "rsync"
	   config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
	   config.vm.provision :shell, path: "bootstrap.sh"      
      ```
      Vagrant will use the shell provisioner to setup the machine, with the bootstrap.sh file, which will install Apache.
      
      ```
      ==> default: Checking for guest additions in VM...
      ==> default: Rsyncing folder: /Users/qcheng/vagrant/fork/vagrant_example/ => /vagrant
      ==> default: Rsyncing folder: /Users/qcheng/vagrant/fork/vagrant_example/synced_folder/ => /synced_folder
      ==> default: Running provisioner: shell...
          default: Running: /var/folders/m6/xrjv0s1d4lvdj6f3wnswrqhc0000gp/T/vagrant-shell20161013-54351-1m062b1.sh
      ........
      
      # bootstrap.sh file as following:
      #!/usr/bin/env bash
      yum -y install httpd
      ```	
      After vagrant starts up or you can run "vagrant reload --provision" instead, you can start the httpd service and check the status:
      
      ```
      $sudo systemctl start httpd.service
      $systemctl is-active httpd.service
	active
      ```    
   - Shut down the running machine:
   
      ```
      $vagrant halt      
      ==> default: Attempting graceful shutdown of VM...
      $vagrant status
      Current machine states:
	default                   poweroff (virtualbox)
      The VM is powered off. To restart the VM, simply run `vagrant up`
      ```
   - Stop the running machine and destroy all resources that were created:
     
      ```
      $vagrant destroy     
      default: Are you sure you want to destroy the 'default' VM? [y/N] y
	==> default: Forcing shutdown of VM...
	==> default: Destroying VM and associated drives...
      ```

   
      
    
    





