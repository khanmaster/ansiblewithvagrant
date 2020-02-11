# Infra Structure as Code - Ansible
 ## Understanding Infra Structure as Code (Iac) concepts with Ansible Provisioning

Timings 90-120 Minutes

## This Lesson Includes
- Pre-requisites
- None

- What is Infra structure As Code (IaC)
- Create a Git Hub Repository
- Multi server environment
- Installing Ubuntu 18.04 LTS app on Windows 
- Creating Multiple servers/VMs using vagrant on Windows 
- What is Ansible
- How does it work - Important features and functions 
- Why should we use it
- Ansible Installation
- Setting up SSH keys to access servers/VMs
- Networking
- Ansible Ad-hoc Commands 
- Understanding Ansible file Directory/file Tree
- Ansible Inventory file, Playbook, tasks and Module 
- Basic overview of YAML YET ANOTHER MARKUP LANGUAGE syntax and intendation  
- Creating and running a playbook - Infra structure as code (IAC) example
- Installing MySQL and Nginx to multi servers using Playbooks

## What is “Infrastructure As Code” (IaC)?
Infrastructure as code (IaC) is the way of defining computing and network infrastructure through source code, the same way you do for applications. Rather than manually configuring your infrastructure or using a one-off isolated script, IaC gives you the power to write code, using a high-level language, to decide how infrastructure should be configured and deployed.

## Why Infra Sturcture as Code (IaC) 
The guiding principle behind IaC is to enforce consistency among DevOps team members by representing the desired state of their infrastructure via code. Moreover, the code can be kept in source control, which means it can be audited, tested on, and used to create reproducible builds with continuous delivery.

Let us walk through the architecture of our exercise 
![](https://github.com/spartaglobal/Ansible/blob/master/IAC_Architecture.png)

# What is Ansible
##Simple - Agentless - IT Automation Tool
Ansible is a universal language, unravelling the mystery of how work gets done. Turn tough tasks into repeatable playbooks. Roll out enterprise-wide protocols with the push of a button.
# How does it work
## Simplicity - Ansible functions by connecting via SSH to the clients
## Agentless – There is no need to install a server and an agent that will pull the changes from it.
### Automation
Since it uses SSH, it can very easily connect to clients using SSH-Keys, simplifying through the whole process. Client details, like hostnames or IP addresses and SSH ports, are stored in files called inventory files. 
Its actions/tasks are defined in YAML files called Playbooks
# Why Ansible?
With its simplicity, ease-of-use, broad compatibility with most major cloud, database, network, storage, and identity providers amongst other categories.
Ansible has been a popular choice of Engineering teams for configuration-management since 2012. 
Ansible can be used as a source control tool. - No need to have scripting knowledge.
The growing predominance of multi-cloud and hybrid cloud architectures, Ansible provides a common platform for enabling mature DevOps and infrastructure as code practices.
Ansible is easily integrated with higher-level orchestration systems, such as AWS Code-Build, Jenkins, or Red Hat.
## Best Practices -  Create a new Git hub repo for ansible
Create a cheat-sheet/go to guide for adhoc-commands, playbooks etc.

Now we will install Ubuntu 18.04 LTS app on Windows
Before we do that we need to enable some option on windows 
- Right click on PowerShell to open it as an admin and run below command
- Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
## Next:
Type Microsoft Store on your search bar next to windows button on bottom left
Search for Ubuntu 18.04 LTS– below snap is taken after it was installed
![](Ubuntu.jpg)
## Follow installation steps
Restart and login to Ubuntu

## Now Lets start from scratch by installing Ansible on Ubuntu distribution of Linux

- sudo apt-get update
- sudo apt-get install software-properties-common
- sudo apt-add-repository ppa:ansible/ansible
- sudo apt-get update
- sudo apt-get install ansible
press Y (yes) when prompted
Complete the installation and check with below command
- ansible –version
![](Ansible_version.jpg)

## Above screen shows it has been installed successfully! 

- We will come back to Ansible once we have our Multi VMs environment ready.

## Moving onto creating Multi server/VMs on Windows using Vagrant:

You have practiced creating 1 VM and now lets take it to next level and use Ansible as provisioning tool in multi server environment  
- We will now create 3 Servers/VMs, web , db and aws using vagrant on windows
- We will use Ubuntu as an ansible controller

## Open Git Bash and run it as an administrator
In the same vagrantfile that you used to create 1 VM,  add the following code and comment out the previous code
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what

# MULTI SERVER/VMs environment 
#
Vagrant.configure("2") do |config|

# creating first VM called web  
  config.vm.define "web" do |web|
    
    web.vm.box = "bento/ubuntu-18.04"
   # downloading ubuntu 18.04 image

    web.vm.hostname = 'web'
    # assigning host name to the VM
    
    web.vm.network :private_network, ip: "192.168.33.10"
    #   assigning private IP
    
    config.hostsupdater.aliases = ["development.web"]
    # creating a link called development.web so we can access web page with this link instread of an IP   
        
    end
  
# creating second VM called db
  config.vm.define "db" do |db|
    
    db.vm.box = "bento/ubuntu-18.04"
    
    db.vm.hostname = 'db'
    
    db.vm.network :private_network, ip: "192.168.33.11"
    
    config.hostsupdater.aliases = ["development.db"]     
    end

# creating third VM called aws 
  config.vm.define "aws" do |aws|
    
    aws.vm.box = "bento/ubuntu-18.04"
    
    aws.vm.hostname = 'aws'
    
    aws.vm.network :private_network, ip: "192.168.33.12"
    
    config.hostsupdater.aliases = ["development.aws"] 
   end
end
```
- Save the and run below command
- vagrant up
- It will take few minutes to create 3 VMs
- check the stastus with below command 
- vagrant status 
![](vagrant_status.jpg)
![](VB_status.jpg)

## Setup SSH keys/password in the host file.
There are few different ways to set up connection and we will use the fast and easy set up to get going with actual Ansible provisioning.
- Bit of a theory about SSH to have clear concept 
- ssh-keygen command will generate rsa key pair
- copy the key to server with below command
- ssh-copy-id root@host
- ssh-copy-id uses the SSH protocol to connect to the target host and upload the SSH user key.
The command edits the authorised keys file on the server. It creates the .ssh directory if it doesn't exist. It creates the authorized keys file if it doesn't exist. Effectively, ssh key copied to server.

- SSH into our VMs 
- ssh vagrant@ip – ssh vagrant@192.168.33.10 or ssh vagrant@192.168.33.11
- ssh vagrant@db ssh varant@web
- password: vagrant
![](vagrant_ssh.jpg)
- ssh vagrant@aws
![](ssh_aws.jpg)
