---
layout: post
title:  "Ansible - getting started"
description: "An introduction to using Ansible for automation. What is Ansible, what can be done with Ansible and some interesting points about Ansible" 
categories: [Ansible]
tags: [cloud, automation]
date:   2017-11-12 07:55:09 +0530
---
This article is for anyone who is evaluating how Ansible may help in their automation journey. While DevOps and Infrastructure as Code are becoming the norm from the startups to the enterprises, I had quite a few conversations on what Ansible is and how could Ansible help in managing the IT environments. This article is written considering those conversations I had in the last few weeks and hope this would give a brief about Ansible and its capabilities.

![]({{ "/assets/ansible_logo.png"}}){:height="25%" width="25%"}

## What is Ansible
The best place to get started with Ansible is to spend about 15 minutes looking at this [Ansible Quick Start video](https://www.ansible.com/quick-start-video).
* Ansible is an **open-source automation language** which helps to write set of tasks, called Ansible Playbooks, to automate various requirements in the IT application infrastructure landscape.
* Ansible is **agentless** i.e. there is no need to install any software and run any daemon on the client machines.
* Ansible needs a core server / virtual machine, called Ansible Control node, which is the Ansible automation engine and helps to run the Ansible Playbooks. The playbooks help execute the tasks on the remote machines.
* Ansible not only works on servers but on applications, databases and networking equipments also.
* The Ansible control server communicates with the clients via
	* SSH for Unix/Linux clients
	* WinRM for Windows clients
	* APIs for Cloud providers
	* Control server needs Python 2.6+ or Python 3.5+ and the clients need Python 2.6+.
* So essentially in an environment some network routes may need to be defined and appropriate ports need to be opened in the firewalls which may sit between the Ansible Control node and the clients.
* Ansible Playbooks are written in YAML to code the use cases. These Playbooks turn up to be the documentation of the environment in which it operates.

## What can Ansible do
* Multi-tier orchestration from infrastructure to applications
* Application deployments (rolling deployments etc.)
* Configuration management
* Continuous delivery
* Automate DevOps processes
* Provisioning in the Clouds (AWS, OpenStack, DigitalOcean etc.)
* Helps to maintain security and compliance
* and many more ...

## Some more points that could be of interest

* Ansible is a open source project sponsored by Red Hat.
* Ansible Tower is a commercial offering from Red Hat to manage complex environments with Ansible and brings in a level of control and ease of management. However, Ansible core can be used independently without implementing Ansible Tower in the environment.
* Ansible itself is written in Python.
* Power of Ansible lies in the Ansible modules. The core modules are written in Python. The modules can be extended by writing in any language that can take JSON as input and output or return key=value pairs.
* The playbooks are written in YAML so the codes are very clean and readable. They can be read and reasonably understood by people who even do not know Ansible or any code and still understand what it intends to do.
* These being YAML files should be kept in a source control repository like Git and hence version controlled. So it would not be too difficult to figure out if something goes wrong and also enable collaboration and sharing amongst the teams. Also a successfully tested code in test environment can be rolled out to the production environment.
* Ansible can address a selected set of inventory - chosen statically or dynamically (especially required in Cloud environments).
* Has a strong open-source community and growing number of modules.

If you have 45 minutes, check out this [talk at PyCon 2014 by Michael DeHaan](https://www.youtube.com/watch?v=Qi0AhK7PMCI), project founder of Ansible. Excellent talk to give you an idea on what Ansible can do for you.

Automation is a journey and Ansible should be making this journey simple and less bumpy. Hope you enjoy this journey and happy automating with Ansible!