# ilias-corona-client


# linux-ansible

This repostory includes the ansible playbooks for a basic ILIAS setup, it's tested on Ubuntu 18.04 LTS

## Requirements:

- Python
- ansible
- ssh access to managed hosts

Ansible needs a user who is allowed to execute commands with ``` sudo ```. You can check it with followed command:
```bash
$ groups
dirk adm cdrom sudo dip plugdev lpadmin sambashare
```

If this is not the case, this can be changed:

```
usermod -a -G sudo <USER>
```

### install ansible

#### Update/upgrade
Before we install Ansible, make sure that your server is updated and upgraded. Do note, should your kernel be upgraded, you'll need to reboot the server. Because of this, make sure to run the update/upgrade at a time when a reboot is possible (unless you have live patching installed, at which point you can run the task any time). To update and upgrade, log into the server to host Ansible and issue the following commands:

```bash
sudo apt-get update
sudo apt-get upgrade -y
```

Once that process completes, reboot the server (if necessary). You're now ready to install.

#### Installing Ansible
Next, install Ansible. Here are the steps to make that happen:

- Log into the Ubuntu Server that will host Ansible
- Install the necessary repository with the command 

```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
```
Install Ansible

```bash
sudo apt-get install ansible -y
```


## Playbooks and roles

### encrypt and decrypt files

#### encrypt file
```
➜ ansible-vault encrypt roles/web-ilias/defaults/main.yml
New Vault password: a-very-secure-password
Confirm New Vault password: a-very-secure-password
Encryption successful
```

#### decrypt files
````
➜ ansible-vault decrypt roles/web-ilias/defaults/main.yml                                        
Vault password: a-very-secure-password 
Decryption successful
````

#### roles/web-ilias

Before you run ansible you have to modify the ```roles/web-ilias/defaults/main.yml  ``` file, the is a template at the same folder.
```yaml
mysql:
  root_db_password: a-secure-root-password-from your Linux instance 
  ilias_db_password: a-secure-ilias-password-for-mysql-user
  hosts:
    - "{{ ansible_hostname }}"
    - 127.0.0.1
    - localhost
```


### Hosts






### Howto Play

Ansible needs only to install at your management host, all other servers has as requirement only python.

Lets start with a dry run:
```
➜ ansible-playbook -C playbooks/generic.yml -i inventories/hosts.yml --limit=ilias  --ask-vault-pass
```

If you don't see any error, so you can deploy your changes:
```
➜ ansible-playbook playbooks/generic.yml -i inventories/hosts.yml --limit=ilias  --ask-vault-pass
```

With ```bash --ask-vault-pass ``` you say Ansible, that the user asked for the ansible-vault password.
You need this because passwords are stored encrypted there.

