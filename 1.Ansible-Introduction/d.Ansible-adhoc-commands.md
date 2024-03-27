# Ansible Commands Reference

This file provides a reference for commonly used Ansible commands.

## Check Connection to Clients

```bash
# 1. to check the connnection to the clients

ansible <clientname/groupname> -m ping -i /etc/ansible/hosts

# 2. to check the all connection

ansible -m ping all

# 3. to execute/implement the ansible playbook
 
ansible-playbook  <playbook-name> -i /etc/ansible/hosts 

# 4. to check the ansbile syntax befor execution

ansible-playbook <playbook-name> -i /etc/ansible/hosts --syntax-check
```

## Ansible Adhoc Commands

Adhoc commands are used for performing simple and quick tasks directly from the command line. They are not reusable and are typically used for one-time operations.

## Overview

Adhoc commands are handy for tasks such as restarting servers or retrieving OS information without the need for writing complex playbooks.

## Example Usage

### Restarting a Server

```bash
ansible <groupname> -m command -a "/sbin/reboot"


## Checking Connection to Clients


ansible <clientname/groupname> -m ping -i /etc/ansible/hosts

# Ansible Adhoc Commands

Adhoc commands are used for performing simple and quick tasks directly from the command line.

## Ping Command

# To check the connection to all hosts:

ansible all -m ping

# To check the connection to hosts in the 'db' group that are also part of the 'web_server' group:

ansible db:web_server -m ping

## Shell Command

#To check uptime on hosts in the 'db' and 'web_server' groups:

ansible db:web_server -m shell -a "uptime"

# To check memory usage on hosts in the 'db' and 'web_server' groups:

ansible db:web_server -m shell -a "free -m"

# To check uptime and memory usage using a custom inventory file 'prod_inv':

ansible -i prod_inv -m shell -a "uptime"
ansible -i prod_inv -m shell -a "free -m"

## Adhoc Command Syntax

# The syntax for running adhoc commands is as follows:

ansible [-i prod_inv] server_name:group1:group2 -m module [-a argument_value]

# For example, to reboot all servers in the 'abc' group in 12 parallel forks:

ansible abc -a "/sbin/reboot" -f 12

# To reboot servers in the 'db' group:

ansible db -a "/sbin/reboot"


# Transfering files using copy module

ansible abc -m copy -a "src=/etc/yum.conf dest=/tmp/yum.conf"

ansible db -m copy -a "src=source/file/path dest=dest/location"

ansible db -m copy -a "src=.hosts/ dest=/tmp/inventory" # (idempotent means if there is no change, Ansible will not do the task)

ansible db -m copy -a "content='hello world' dest=/tmp/hello.txt"

ansible db -m copy -a "content=' world' dest=/tmp/hello.txt backup=yes"

ansible db -m copy -a "src=/root/project/file.yml dest=/root/my_folders/file.yml"


# Downloading a file from nodes using Ansible adhoc command


# Downloading a file using the fetch module


# Syntax:

ansible db_servers -m fetch -a "src=/source/file/path dest=/dest/location"

ansible db -m fetch -a "src=/root/test1.txt dest=./demo/" # (it creates with the entire path of the file)

ansible db -m fetch -a "src=/root/test1.txt dest=./newdemo/" flat=yes # (flat=yes it will not create dir structure)

ansible db -m fetch -a "src=/root/test1.txt dest=./demo/{{inventory_hostname}}_test1.txt flat=yes"


# Create or delete file or directory on managed nodes using file module


# For directory:


ansible db -m file -a "dest=/root/file state=directory"

ansible db -m file -a "dest=/root/file1 mode=755 owner=root group=root state=directory"

ansible db -m file -a "dest=/root/file state=absent"

# For files:


ansible db -m file -a "dest=/root/file.txt state=touch"

ansible db -m file -a "dest=/root/file1.txt mode=755 owner=root group=root state=touch"

ansible db -m file -a "dest=/root/file1.txt state=absent"


# Install a package like git, httpd, mysql on Linux


# Linux-CentOS/Fedora/Red Hat: yum
# Ubuntu: apt

ansible db -m yum -a "name=git state=present"
ansible db -m yum -a "name=git state=absent"
ansible db -m yum -a "name=git state=latest"
ansible db -m yum -a "name=git state=present" -b # (-b option used if it is not root user/has root privileges)


# Command Module


# Used to execute binary commands.
# It is the default module.
# With the Command module, the command will be executed without being proceeded through a shell. As a consequence, some variables like $HOME are not available. Also, stream operations like <, >, |, and & will not work.
# The command module is more secure because it will not be affected by the user’s environment.

# Example:

ansible all -m command -a "uptime" # It will use the command module by default

#If managed nodes are not installed with Python, use the raw module:

ansible db -m raw -a "uptime"


# Service Module


ansible db -m service -a "name=httpd state=restarted"
ansible db -m service -a "name=httpd state=started"
ansible db -m service -a "name=httpd state=stopped"


# Shell Module


ansible db -m shell -a "mkdir test"
```
