# simple-ansible-setup

Up the virtual box and test ssh into box
```
vagrant up
ssh vagrant@10.100.199.200
```

# Test connections to the vagrant box

Up the four VMs, as specified in your inventory file. Pass is 'vagrant':
```
ansible all --ask-pass -i ./ansible/hosts/local -m ping
```

Run Ansible playbook for nginx against the box just upped. 
```
ansible-playbook --ask-pass ./ansible/nginx.yml -i ./ansible/hosts/local
```

# Ansible structure
From root:
- ./xxx.yml is the playbook. It describes what roles should be applied to what hosts.
- ./group_vars/all. Contains variable that are valid throughout all Ansible modules
- ./host_vars/<ip>. Contains variable specifi to a host with specific IP.
- ./hosts/local is the inventory file and describes which host has which ip adresses. It describes the target landscape.
- ./roles is the location in which each role described. One folder per role. It describes what is required for a system to fulfill a  role (jenkins, docker, nginx, apache, mysql)

Per role-folder (./role):
 - defaults/main.yml describe all 'variables' which are used within tasks. In example to iterate over directories, file and templates or assign ports and volumes ...
 - tasks/main.yml describe all tasks/activities that need to be performed, which use defaults as descibed in previous step.
 - files/xxx.xx is one or more files that need to be copied over.
 - templates/xxx.conf.js is one or more jinja2 template files in which variable are automatically populated by Ansible (i.e with items set in defaults or vars from host_vars).


# Some notes

In case of
```
fatal: [10.100.199.200] => SSH Error: Host key verification failed.
```
remove fingerprint for this host from your ~/.ssh/known_hosts file.
```
vim ~/.ssh/known_hosts file
```
