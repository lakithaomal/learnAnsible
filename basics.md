# Basics of Ansible
Ansibles main goals is automating cluster management. Lets take a simple expample on working on a set of interconnected servers. 
- For a powerdown
  - Power down web servers
  - Power down DB servers
 - For power back up
   - Power up DB servers
   - Power up webservers
To simply automate this kind of service we use an ansible playbook.


## Ansible Configuration Files

The ansible config file typically woould be at `/etc/ansible/ansible.cfg` and would like this.

``` cfg
[defaults]
inventory = /etc/ansible/hosts
log_path = /var/log/ansible.log
library = /usr/share/my_modules/
roles_path = /etc/ansible/roles
action_plugins = /usr/share/ansible/plugins/action
gathering = implicit
# SSH timeout
timeout = 10
forks = 5

[inventory]
[privilege_escalation]
[paramiko_connection]
[ssh_connection]
[persistent_conn
```

However when you have multiple servers with multiple configerations,  you would need multiple configaration files. For example they would be at these locations. 

- `/etc/ansible/ansible.cfg` 
- `/opt/web-playbooks/ansible.cfg `
- `/opt/db-playbooks/ansible.cfg`
- `/opt/network-playbooks/ansible.cfg`

Also you can specify the location of a playbook using an environmental variable (`$ANSIBLE_CONFIG=/opt/ansible-web.cfg`). In fact this playbook will get priority to any other one. The proirity preference is listed below. 

- Environmental Variable : `$ANSIBLE_CONFIG=/opt/ansible-web.cfg`
- Playbook contained in the current directory which we run from.
- Playbook contained in the current users home directory.
- The playbook at the default ansible config file location (`/etc/ansible/ansible.cfg`). 

You can also change varaibles on the ansible playbook. The persistance of that change is breifly introduded below.

- Change Persisting for the current run:
``` bash 
ANSIBLE_GATHERING=explicit
ansible-playbook playbook.yml
```
- Change Persisting for the current session:
``` bash 
$ export ANSIBLE_GATHERING=explicit
$ ansible-playbook playbook.yml
```
- Change Persisting for multiple terminals/sessions:
At `/opt/web-playbooks/ansible.cfg` add the line  `gathering = explicit playbook.yml`









