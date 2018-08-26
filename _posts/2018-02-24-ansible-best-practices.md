---
layout: post
title:  "Ansible best practices"
date:   2018-02-24 07:55:09 +0530
---
"Ansible is a Swiss Army Knife for DevOps" - Tim Appnel, Principal Product Manager, Ansible. In this article we review some best practices to consider while writing the Ansible playbooks. This should be particularly useful for anyone starting with Ansible. While writing the playbooks in YAML, consider them as the documentation of the automation that you are bringing into the environment. Anyone with knowledge of Ansible should be able to read the playbooks and easily be able to decipher what those are expected to do.

![Ansible is a Swiss Army Knife for DevOps]({{ "/assets/ansible-swiss-army-knife.png"}}){:height="50%" width="50%"}

1. **Version Control.** Treat the playbooks just like other software development code. It is necessary to keep them in a repository like Git and use the code development approach to version control them and release them to production. Use pull requests and code reviews to progress a code through the development, test, production stages. Code reviews may not be a typical process for sysadmin teams, however this is one of the practices of software development that is essential to be adopted, especially before using the playbooks in production.
2. **Start simple and then build on it.** While starting to write a play book it would be easiest to document the thought process in a single play and then fine tune and break it up from there. Consider breaking the playbooks into multiple logical playbooks and roles and then stitching them together from a main playbook.
3. **Treat the code as documentation.** A well written playbook is the documentation of the automation workflow. Strive to keep the workflow simple and easily readable. Anyone reading this playbook should easily understand what is going on in the workflow. This should be part of the acceptance criteria when accepting pull requests. Few simple guidelines are:
* Use self explanatory name of playbooks and roles
* Include meaningful `name` to the tasks
* Give meaningful and unique names to the `variables`. Consider grouping the variables together in the playbook rather than having them all over the playbook.
* Include meaningful `debug` statements. Can someone new to the environment understand what exactly is going on when a playbook is executed? Also consider if a debug message is really necessary rather than overusing them. The key messages should be covered. Also consider the right verbosity level.
* Use meaningful hostnames rather than IP addresses and cryptic hostnames.
4. **Modules first.** Always use modules over shell commands. Check the available modules with `ansible --list-modules`.
5. **Roles.** Use `ansible-galaxy init` command to create the role directory structure. Remove the unused directories from the role directory structure to simplify and improve readability.
6. **No point reinventing the wheel.** Before writing a playbook from scratch research what may already be available in the repositories. A similar code may already have been written by someone in another part of the organisation. Use that as a starting point and fine tune it for your environment.

I would recommend you to listen to the [talk by Timothy Appnel, Principal Product Manger of Ansible](https://www.ansible.com/videos-ansible-automates-ansible-best-practices-the-essentials-nov-2017).