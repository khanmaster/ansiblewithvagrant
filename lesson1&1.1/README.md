# Infra Structure as Code - Ansible Configuration Management

 ## Understanding Infra Structure as Code (Iac) concepts with Ansible Provisioning

- Pre-requisites
- Virtual Box on windows
- Vagrant Installed

Timings 45-60 Minutes

## This Lesson Includes

- What is Infra structure As Code (IaC)
- What is Ansible and why should we use it
- Create a Git Hub Repository
- Multi server environment
- Installing Ubuntu 18.04 LTS app on Windows 
- Creating Multiple servers/VMs using vagrant on Windows 

## What is “Infrastructure As Code” (IaC)?
Infrastructure as code (IaC) is the way of defining computing and network infrastructure through source code, the same way you do for applications. Rather than manually configuring your infrastructure or using a one-off isolated script, IaC gives you the power to write code, using a high-level language, to decide how infrastructure should be configured and deployed.

## Why Infra Sturcture as Code (IaC) 
The guiding principle behind IaC is to enforce consistency among DevOps team members by representing the desired state of their infrastructure via code. Moreover, the code can be kept in source control, which means it can be audited, tested on, and used to create reproducible builds with continuous delivery.

Let us walk through the architecture of our exercise 
![](https://github.com/spartaglobal/Ansible/blob/master/IAC_Architecture.png)

# What is Ansible
## Simple - Agentless - IT Automation Tool
Ansible is a universal language, unravelling the mystery of how work gets done. Turn tough tasks into repeatable playbooks. Roll out enterprise-wide protocols with the push of a button.

# How does it work
## Simplicity - Ansible functions by connecting via SSH to the clients

## Agentless – There is no need to install a server and an agent that will pull the changes from it.

## Automation
Since it uses SSH, it can very easily connect to clients using SSH-Keys, simplifying through the whole process. Client details, like hostnames or IP addresses and SSH ports, are stored in files called inventory files. 
Its actions/tasks are defined in YAML files called Playbooks

# Why Ansible?
## It is Vital to know why should we use Ansible

With its simplicity, ease-of-use, broad compatibility with most major cloud, database, network, storage, and identity providers amongst other categories.
Ansible has been a popular choice of Engineering teams for configuration-management since 2012. 
Ansible can be used as a source control tool.

- Best thing about Ansible is that you do not need to have advance scripting knowledge.

## How does it fit into DevOps culture and practices
- The growing predominance of multi-cloud and hybrid cloud architectures
- Ansible provides a common platform for enabling mature DevOps and infrastructure as code practices.
- Ansible is easily integrated with higher-level orchestration systems, such as AWS Code-Build, Jenkins, or Red Hat.

## Best Practices -  Create a new Git Hub Repository for Ansible
- Create a cheat-sheet/go to guide for adhoc-commands, playbooks etc. this will build your Git Hub profile and history


