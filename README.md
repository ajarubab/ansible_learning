Important Ansible Commands : 

After initalising remote connection

- ansible (Double tab)
- ansible --version
- ansible-config init --disabled > ansible.cfg
- ansible host_pattern -m module -a 'module-argument' -i inventory (SYNTAX)
- ansible-navigator doc ping
- ansible-navigator doc file
- ansible-navigator doc ping -m stdout
- podman images
- ansible-navigator inventory --list
- ansible all -m ping
- ansible all -m ping -a 'data=success'
- ansible all -m ping -k -vvv
- ansible-inventory --list
- ansible-inventory --graph
- ansible -i ../inventory all -m ping
- ansible nodeA -m command -a 'ls -ld /tmp/dirByAnsible/'

# Run as ansible-playbook
    ansible-playbook  '/workspaces/ansible-learning/playbooks/create_dir.yml'

# Run as ansible-navigator
    ansible-navigator run  '/workspaces/ansible-learning/playbooks/create_dir.yml' --ee true --pae false --ce podman --eei 'registry.redhat.io/ansible-automation-platform-25/ee-supported-rhel8:latest'

# Run as ansible-navigator with verbosity( -v,-vv,-vvv,-vvvv,-vvvvv,-vvvvvv)
    ansible-navigator run  '/workspaces/ansible-learning/playbooks/create_dir.yml' --ee true --pae false --ce podman --eei 'registry.redhat.io/ansible-automation-platform-25/ee-supported-rhel8:latest' -vvv

# Run as ansible-navigator with verbosity( -v,-vv,-vvv,-vvvv,-vvvvv,-vvvvvv)
    ansible-navigator run  '/workspaces/ansible-learning/playbooks/create_dir.yml' --ee true --pae false --ce podman --eei 'registry.redhat.io/ansible-automation-platform-25/ee-supported-rhel8:latest' -vv

* --ee true : Execution Environment enabled
* --pae false : Playbook Artifact Enable set false here 
* --ce podman : container Engine is podman
* --eei : Execution Environment Image


# (SMOKE TEST, DRY RUN) [Not actual run] - Recommended Before every Actual Run
- ansible-navigator run playbooks/usr_n_grp_mng.yml --check

# (SYNTAX CHECK BEFORE RUN)
- ansible-navigator run playbooks/usr_n_grp_mng.yml --syntax-check

# (Modify indentation or wrong keywords using vi editor)
- vi playbooks/usr_n_grp_mng.yml

# (Running an artifact.json file to check its record of last playbook run output)
- ansible-navigator replay playbooks/usr_n_grp_mng-artifact-2026-10-22T11:34:41.152451+00.00.json

# (Check the package firewalld in Integrated terminal of ansible on all nodes)
- ansible nodeA,nodeB,nodeC -m command -a 'rpmquery firewalld'

# (Install the firewalld service using adhoc command on all 3 hosts)
- ansible nodeA,nodeB,nodeC -m dnf -a "name=firewalld state=present" -b
- ansible nodeA,nodeB,nodeC -m dnf -a "name=firewalld state=started enabled=yes" -b

# (Uninstall the firewalld service using adhoc command on all 3 hosts)
- ansible nodeA,nodeB,nodeC -m dnf -a "name=firewalld state=absent" -b

# (Ansible- vault commands)

- ansible-valut encrypt playbooks/pwd_file
New Vault password:
Confirm New Vault password:
Encryption successful

- ansible-vault view playbooks/pwd_file
Vault password:

- ansible-vault edit playbooks/pwd_file
Vault password:
(press i -> Edit the content in automatically opened VI editor -> Esc -> :wq Enter)

- ansible-vault decrypt playbooks/pwd_file --output=dup_pwd_file
Vault password:
Decryption successful

- ansible-vault decrypt playbooks/pwd_file
Vault password:
Decryption successful

(Creation of a vault file )
- ansible-vault create playbooks/vars/secret.yml --vault-id myvault@prompt
New vault password (myvault):
Confirm new vault password (myvault):

(creating a file having password of myvault password)
- echo redhat123 > playbook/safeguard

* (Fetching the secrect.yml file content without putting vault password in prompt and giving vault password by safeguard file)
- ansible-vault view playbooks/vars/secret.yml --vault-password-file=playbooks/safeguard

* if we change the password text of safeguard file from redhat123 to Linux123 
- echo Linux123 > playbooks/safeguard

* In that case to access the content of out secret.yml valut file we have to change (REKEY) the vault password by:-

- ansible-vault rekey playbooks/vars/secret.yml
Vault password: [type redhat123]
New Vault password: [type Linux123]
Confirm New Vault password: [Again type Linux123]
Rekey successful

* In this case the secret key(content of of the secret.yml file) also got changed automatically

# FACTS GATHERING and RETRIEVAL
* "gather_facts" keyword value is true by default.

# (by Adhoc commands)

* To get All avaibale Facts of nodeA and nodeC hosts
- ansibale nodeA,nodeC -m setup

* To get only memory related info from FACTS for all hosts
- ansible all -m setup | grep mem

# Importing Custom Facts of any host

To import the custom facts of any server/host machine, those info should be kept in the path 
"/etc/ansible/facts.d" with any filename but should have an extention of ".fact"
Only iin that case the modeule "ansible_local" can fetch the info into the terminal of vscode ide under dev container.
And for it we do not need the root login permission [ become: true] of the host/server.
