<h1 align="center">Vagrantfile Gerencianet - PHP</h1>

![Curso de integração Gerencianet](https://media-exp1.licdn.com/dms/image/C4D1BAQH9taNIaZyh_Q/company-background_10000/0/1603126623964?e=2159024400&v=beta&t=coQC_AK70vTYL3NdvbeIaeYts8nKumNHjvvIGCmq5XA)

<p align="center">
  <a href="https://github.com/gerencianet/gn-vagrant-php/">Portuguese</a> |
  <span><b>English</b></span>
</p>

---

This repository provides a Vagrantfile template to facilitate creating a virtual machine using a hypervisor. Think of it something like XAMPP/WAMP that doesn't depend on your operating system. After completing the configuration, you will have a virtual machine with Ubuntu, ready to run PHP projects.

Jump To:
* [About Vagrant](#about-vagrant)
* [How it works?](#how-it-works)
* [Simplified setup](#simplified-setup)
* [Folder sharing and port forwarding](#folder-sharing-and-port-forwarding)
* [VM Setup and tools](#vm-setup-and-tools)
* [Useful commands](#useful-commands)

---

## About Vagrant

The [Vagrant](https://www.vagrantup.com/) is a great tool for creating and configuring lightweight, reproducible and portable virtual machine environments.

Development environments managed by Vagrant can run on local virtualized platforms (providers) such as VirtualBox, VMware and others. Here we detail how to install using VirtualBox.

Vagrant provides the configuration framework and format to create and manage complete development environments. These development environments run on your desktop or in the cloud and are portable across Windows, Mac OS X and Linux.


## How it works?
When we run the command for Vagrant to upload a VM, it reads the [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile) file. It contains all the configurations and settings for the VM in question. Vagrant starts a box in the provider, defined in the configuration file. As there are more express instructions, it executes them, leaving a fully configured machine available for development.


## Simplified setup
1. Install dependencies according to your operating system
    
* Go to the [VirtualBox](https://www.virtualbox.org/) [download page](https://www.virtualbox.org/wiki/Downloads), choose your operating system, download and install the software.
  * Virtualization support must be enabled in your motherboard BIOS (setting may vary depending on your motherboard model).
* Go to the [Vagrant](https://www.vagrantup.com/downloads) [download page](https://www.vagrantup.com/downloads), choose your operating system, download and install the software.

2. Clone this project and access the directory

    Create a directory for your VM (Ex: ``C:\Users\%userprofile%\vagrant-boxes``), access it and run the following commands:
    
    ```
    $ git clone https://github.com/gerencianet/gn-vagrant-php.git
    $ cd gn-vagrant-php
    ```

3. Startup

    To start your machine, run the following command in the root directory of Vagrantfile:

    ```
    # Calls vagrant to download the Ubuntu image (if necessary) and restart an instance.
    $ vagrant up
    ```
    
    **What about VirtualBox?**

    It looks like we don't use VirtualBox for anything. But by opening the VirtualBox app you will see that the VM is running.

    As you can see, VirtualBox is letting us know that Ubuntu is working correctly.
    
    ![VM in VirtualBox](https://gnetbr.com/Bkl9PuH7pO)


 4. SSH access  

    We now have this operating system set up on our computer, but how do we access it? Just as you would access any remote Linux server via the command line, you will do the same with Vagrant. Run the following command from the root directory of Vagrantfile to safely enter the virtual machine.
   
    ```
    # Connects you to the virtual machine. The configuration is stored in the directory so you can always return to this machine.
    $ vagrant ssh
    ```

## Folder sharing and port forwarding

There is shared folder configuration defined in Vagrantfile, creating in vagrant directory the folder ``www_gn``. This directory, which maps to the Apache root folder, allows you to deploy your project in this folder.

Vagrant's port forwarding allows you to access a port on your host machine and have all data forwarded to a port on the guest machine, either by TCP or UDP. With this setting, you can then open your browser http://localhost:8080 and browse your project.
``` 
config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.synced_folder ".", "/vagrant", type: "virtualbox", owner: "www-data", group: "www-data"
```

## VM Setup and tools

The following setup and tools will be automatically configured when starting the VM.
* Ubuntu v20.04
* 1024mb of RAM memory
* Apache
* PHP v8.0
* Nano
* Git
* Composer

## Useful commands
To run the commands on the VM, you must be in the directory where the Vagrantfile in question is located.

```
# Restart the virtual machine.
# Equivalent to a reboot. Useful especially when there are changes in Vagrantfile.
$ vagrant reload

# Shut down the virtual machine.
$ vagrant halt

# Suspends virtual machine execution saving its state.
# Ideal for day-to-day when you're developing, and you're going to take a break.
$ vagrant suspend

# Resume a previously suspended virtual machine.
$ vagrant resume

# Destroy the virtual machine.
# Use when you want to start from scratch.
$ vagrant destroy
```
