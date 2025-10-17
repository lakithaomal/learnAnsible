# Ansible Basics: Automating Cluster Management 🚀

The primary function of Ansible is **automating infrastructure and cluster management**. It streamlines complex, multi-step processes by executing instructions on multiple servers simultaneously.

Consider the complexity of managing a multi-tier application (like a web server cluster and its database servers). A simple service restart requires a coordinated sequence:

| Action | Phase | Sequence |
| :--- | :--- | :--- |
| **Power Down** | Shutdown | 1. Power down web servers |
| | | 2. Power down DB servers |
| **Power Up** | Startup | 1. Power up DB servers |
| | | 2. Power up web servers |

To automate such precise, sequenced operations, Ansible uses **Playbooks** (`.yaml` files), which define the exact tasks and their order.

***

## Ansible Configuration Files (`ansible.cfg`) ⚙️

The **Ansible configuration file** (`ansible.cfg`) controls the behavior of the Ansible environment, defining paths, timeouts, parallelism, and more.

### Default Structure

The default configuration file is typically located at: `/etc/ansible/ansible.cfg`. It is structured into sections (e.g., `[defaults]`, `[ssh_connection]`).

```cfg
[defaults]
inventory = /etc/ansible/hosts      # Path to the list of target servers
log_path = /var/log/ansible.log
roles_path = /etc/ansible/roles
gathering = implicit                # How Ansible collects server facts
timeout = 10                        # SSH connection timeout in seconds
forks = 5                           # Default number of parallel processes

[inventory]
[privilege_escalation]
# ... other sections
```

### Configuration Priority and Locations

Ansible is flexible and can use multiple configuration files depending on the execution context. When deciding which settings to use, Ansible follows a strict order of precedence:

1.  **Environment Variable** (`$ANSIBLE_CONFIG`): The absolute highest priority. If the environment variable `$ANSIBLE_CONFIG` is set, Ansible uses only the file specified there.
    * **Example:** `$ANSIBLE_CONFIG=/opt/ansible-web.cfg`
2.  **Local Directory:** A file named `ansible.cfg` in the **current directory** from which the `ansible-playbook` command is run.
3.  **Home Directory:** A file named `.ansible.cfg` (note the leading dot) in the **current user's home directory** (`~/.ansible.cfg`).
4.  **System Default:** The default configuration file located at **`/etc/ansible/ansible.cfg`**.

***

## Changing Configuration Variables

You can override configuration settings temporarily or permanently using environment variables. These changes affect the priority order described above.

| Persistence Level | Method | Example |
| :--- | :--- | :--- |
| **Current Run** | Direct command prefix. | `ANSIBLE_GATHERING=explicit ansible-playbook playbook.yml` |
| **Current Session** | Exporting the variable. | `export ANSIBLE_GATHERING=explicit` |
| **Multiple Sessions/Users** | Placing the variable into an active **`ansible.cfg`** file. | In `/opt/web-playbooks/ansible.cfg`, add the line: `gathering = explicit` |

***

## Inspecting the Configuration State

Ansible provides three utility commands to inspect the live configuration state, each offering a different view:

| Command | What it Shows | Purpose |
| :--- | :--- | :--- |
| **`ansible-config list`** | **All possible settings** with their current value, default value, and source. | **Discovery:** Use to see every option available and where the current value is coming from. |
| **`ansible-config view`** | The raw text of the **active `ansible.cfg` file**. | **Verification:** Use to see the exact content of the loaded configuration file. |
| **`ansible-config dump`** | All **active settings that differ from their default value**. | **Portability:** Use to generate a minimal configuration file containing only the customized settings. |

### Example using `ansible-config dump`

This command demonstrates how an environment variable impacts the configuration dump:

```bash
$export ANSIBLE_GATHERING=explicit$ ansible-config dump | grep GATHERING
```

The output confirms that the environment variable (`ANSIBLE_GATHERING`) successfully set the `DEFAULT_GATHERING` configuration parameter:

```
DEFAULT_GATHERING(env: ANSIBLE_GATHERING) = explicit
```
