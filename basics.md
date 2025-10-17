# Ansible Basics 
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

To check on the current configurations you can try out the following commands. 
  
- `ansible - config list`: This command lists all possible configuration settings that Ansible recognizes, along with their: 
  - Current Value: The value Ansible is actually using (which might come from the default, an environment variable, or a config file).
  - Default Value: The hardcoded default value for that setting.
  - Source: Where the current value came from (e.g., "default," "ansible.cfg," or an environment variable).
  - Description: A brief explanation of what the setting does.
- `ansible - config view`: active ansible.cfg configuration file that Ansible is currently using, in its raw file format.
  - Content: Only shows the settings defined in the specific ansible.cfg file.
  - Format: The raw text of the configuration file, typically organized by sections (e.g., [defaults], [privilege_escalation]).
- `ansible - config dump`:This command outputs all active, non-default configuration settings that Ansible is currently using, presenting them in a structured .ini format.
  - Content: Lists only settings whose current value differs from the default. It excludes any options that are still at their factory setting.
  - Format: Presents the output as a clean, complete ansible.cfg file.







