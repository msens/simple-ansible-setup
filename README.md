# Prerequisites
1. Make sure you have Ansible installed on your host.
2. Make sure to have Virtualbox and Vagrant installed on your host.

# Simple-ansible-setup

This ansible setup provisions some virtual boxes with nginx . tomcat etc.
Up the virtual boxes and test ssh into the boxes to add fingerprints to known_hosts file.
```
vagrant up
ssh vagrant@10.100.199.201
ssh vagrant@10.100.199.202
ssh vagrant@10.100.199.203
```

In case of
```
fatal: [10.100.199.201] => SSH Error: Host key verification failed.
```
or
```
fatal: [10.100.199.201] => {'msg': "FAILED: ('10.100.199.201', <paramiko.rsakey.RSAKey object at 0x1085649d0>, <paramiko.rsakey.RSAKey object at 0x10857e450>)", 'failed': True}
```

..... remove fingerprint for this host from your ~/.ssh/known_hosts file.
```
vim ~/.ssh/known_hosts
```

# Test connections to the vagrant box

To test connection to your virtual box execute lines below. Pass is 'vagrant':
```
ansible all --ask-pass -i ./ansible/hosts/local -m ping
```

Run Ansible playbooks on a per playbook basis against the boxes just upped. 
```
ansible-playbook --ask-pass ./ansible/nginx.yml -i ./ansible/hosts/local
ansible-playbook --ask-pass ./ansible/tomcat.yml -i ./ansible/hosts/local
```

Or run Ansible playbooks on per tag. 
```
ansible-playbook --ask-pass ./ansible/nginx.yml -i ./ansible/hosts/local -t section 6
ansible-playbook --ask-pass ./ansible/tomcat.yml -i ./ansible/hosts/local
```


Or run all at once. 
```
ansible-playbook --ask-pass ./ansible/all.yml -i ./ansible/hosts/local
```

# urls
- Access nginx when installed on vm1: http://10.100.199.201:80/
- Access any of the two backend tomcats through loadbalancer: http://10.100.199.201:81/
- Access any of the two backend tomcat petclinics through loadbalancer: http://10.100.199.201:81/petclinic
- Access tomcat installed on vm2: http://10.100.199.202:8080/
- Access tomcat installed on vm3: http://10.100.199.203:8080/
- Access petclinic installed on vm2: http://10.100.199.202:8080/petclinic
- Access petclinic installed on vm3: http://10.100.199.202:8080/petclinic


# Ansible structure
From ansible-root:
- ./xxx.yml is the playbook for each type of host that is defined. For eacht type, it describes what roles should be applied and how it should be done.
- ./hosts/local is the inventory file and describes which host-types should be applied to what ip adresses/hosts. It describes the target landscape.
- ./group_vars/all. Contains variable that are valid throughout all Ansible modules
- ./host_vars/<ip>. Contains variable specifi to a host with specific IP.
- ./roles is the location in which each role described. One folder per role. It describes what is required for a system to fulfill a  role (jenkins, docker, nginx, apache, mysql)

Per role-folder (./role):
 - defaults/main.yml describe all 'variables' which are used within tasks. In example to iterate over directories, file and templates or assign ports and volumes ...
 - tasks/main.yml describe all tasks/activities that need to be performed, which use defaults as descibed in previous step.
 - files/xxx.xx is one or more files that need to be copied over.
 - templates/xxx.conf.js is one or more jinja2 template files in which variable are automatically populated by Ansible (i.e with items set in defaults or vars from host_vars).


# Some notes

## In case of issues 
```
fatal: [10.100.199.200] => SSH Error: Host key verification failed.
```
remove fingerprint for this host from your ~/.ssh/known_hosts file.
```
vim ~/.ssh/known_hosts
```

## Tomcat directory structure
Note that on Ubuntu, tomcat is installed over following directories:
- /etc/tomcat7 - properties and policies
- /usr/share/tomcat7 - scripts, binaries
- /var/cache/tomcat7
- /var/lib/tomcat7 - webapps
- /var/log/tomcat7 - logs
- /etc/default

## nginx
Use the following to see status / log of nginx on node1:
```
sudo nginx
```

Kill all processes running on port 80:
```
sudo fuser -k 80/tcp
```

## cis
In cis module, the following polocies have been disabled for now.
For more info on cis, please see the following vid: https://www.youtube.com/watch?v=8h_Y-L1Q8xI

 - name: 1.5.5 Disable Interactive Boot (Scored)
 - name: 3.1 Set Daemon umask (Scored)
 - name: 3.2 Remove X Windows (runlevel 3) (Scored)
 - name: 5.1.4 Create and Set Permissions on rsyslog Log Files (Scored)
 - All off leverl 5.2!
 - All off 6.3
 - 9.2.1
 - 9.2.5.

