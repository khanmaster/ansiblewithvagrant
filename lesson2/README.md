# Infra Structure as Code - Ansible Configuration Management

### Understanding concepts with Ansible Provisioning

Timings 90-120 Minutes

## This Lesson Includes

- Multi server environment
- Creating Multiple servers/VMs using vagrant on Windows 
- Ansible Installation
- Setting up SSH keys to access servers/VMs
- Networking
- Vagrant VMs touble shooting



## Moving onto creating Multi Server/VMs on Windows using Vagrant:

You have practiced creating 1 VM and now lets take it to next level and use Ansible as provisioning and configuration management tool in multi server environment  
- We will now create 3 Servers/VMs, web , db and aws using vagrant on windows
- We will use our vm called web as an ansible controller
- we will connect with VMs called db and aws from our ansible controller 

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
- Save the code in vagrantfile and run below command
- ```vagrant up```
- It will take few minutes to create 3 VMs
- check the stastus with below command 
- ```vagrant status``` 
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/vagrant_status.jpg)
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/VB_status.jpg)

## Setup SSH keys/password in the host file.
There are few different ways to set up connection and we will use the fast and easy set up to get going with actual Ansible provisioning.
- Bit of a theory about SSH to have clear concept 
- ```ssh-keygen``` command will generate rsa key pair
- copy the key to server with below command
- ```ssh-copy-id``` root@host
- ```ssh-copy-id``` uses the SSH protocol to connect to the target host and upload the SSH user key.
The command edits the authorised keys file on the server. It creates the .ssh directory if it doesn't exist. It creates the authorised keys file if it doesn't exist. Effectively, ssh key copied to server.

- SSH into our VMs 
- ssh vagrant@ip – ``` ssh vagrant@192.168.33.10 or ssh vagrant@192.168.33.11 ```
- ``` ssh vagrant@db ```  ``` ssh varant@web ```
- password: ```vagrant```
![](images/vagrant_ssh.jpg)
- ```ssh vagrant@aws```
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/ssh_aws.png)

- WOW! We have successfully created multi-server/client environment!

# Trouble-Shooting Tips

# Let us touch base on some common trouble shooting issues before we jump back to Ansible
### Vagrant VM rebooting failure or unable to find private key etc.
- Run this command  and reboot the VM ```echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null```
### You can also add following code to your vagrantfile to resolve the issue.
``` 
 config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
end
```
# Installing Ansible on Ubuntu
## Now Lets start from scratch 
- ``` sudo apt-get update ```
- ``` sudo apt-get install software-properties-common ```
- ``` sudo apt-add-repository ppa:ansible/ansible ```
- ``` sudo apt-get update ```
- ``` sudo apt-get install ansible ```
- press Y (yes) when prompted
- Complete the installation and check with below command
- ``` ansible –version ```
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/Ansible_version.jpg)

## Above screen shows it has been installed successfully! 

- install tree with ```sudo apt install tree ```

- Now go to ``` cd /etc/ansible$```
- Type ```tree``` to see the file tree of ansible directory
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/tree.jpg)

- Ansible Inventory file - In the Ansible terminology, an inventory is a file where the target hosts of our actions are specified.
- Its default path is ```/etc/ansible/hosts```
- Open hosts file and add following entry 
``` bash 
[web]
192.168.33.10 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
[db]
192.168.33.11 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
[aws]
192.168.33.12 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
 ```
- In the Ansible hosts file we have three servers web, db and aws
- The IPs must match the hardcoded IPs defined in our vagrantfile to connect via SSH
- ``` ansible_connection=ssh ``` enables ansible to connect to the server with ssh key or password
- ``` ansible_ssh_user=vagrant``` ```ansible_ssh_pass=vagrant``` enable you to connect to VMs with user ID vagrant and password vagrant
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/ansible_hosts.jpg)

## Now let us check if we can ping our servers
- Use this Command: ``` ping 192.168.33.10 ```
- Use this Command: ``` ping 192.168.33.11 ```

![](/https://github.com/spartaglobal/Ansible/blob/lesson1/images/ping_command.jpg)
- Great! We have working multi-server env ready to play with Ansible in the next lacture!

Summary: You have learned 

- Multi server environment
- Creating Multiple servers/VMs using vagrant on Windows 
- Ansible Installation
- Setting up SSH keys to access servers/VMs
- Networking
- Vagrant VMs touble shooting
