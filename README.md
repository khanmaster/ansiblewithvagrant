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
### Simplicity - Ansible functions by connecting via SSH to the clients
### Agentless – There is no need to install a server and an agent that will pull the changes from it.
###Automation
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
- So right click on PowerShell to open it as an admin and run below command
- Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
## Next:
Type Microsoft Store on your search bar next to windows button on bottom left
Search for Ubuntu 18.04 LTS– below snap is taken after it was installed

