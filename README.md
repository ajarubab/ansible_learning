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


# Selecting Hosts by Using Host Patterns

* When playbook tasks are applied on all the managed host mentioned in inventory file 
- hosts: all
- hosts: '*'

* When playbook tasks are applied on nodeA only 
- hosts: nodeA

* When playbook tasks are applied on nodeB only 
- hosts: nodeB

* When playbook tasks are applied on nodeA and nodeC only 
- hosts: nodeA,nodeC

* When playbook tasks are applied on ip address 192.178.92.130 only 
- hosts: 192.178.92.130

* When playbook tasks are applied on nodeB and ip address 192.178.92.130 only 
- hosts: nodeB, 192.178.92.130

* When playbook tasks are applied on group Prod only 
- hosts: Prod

* When playbook tasks are applied on group Dev and Stag only 
- hosts: Dev,Stag

* When playbook tasks are applied on group Prod and ip address 192.178.92.130 only 
- hosts: Prod,192.178.92.130

* [test_nodes:children]
dev
stag

here test_nodes will be known as Group of groups or Nested groupand ":childeren" is mandatory for declaring a nested group. hence

* When playbook tasks are applied on nested test_nodes only 
- hosts: test_nodes

which eventually means
- hosts: dev,stag

* When playbook tasks are applied on ip address 192.178.92.10 to 192.178.92.100 (on 90 servers/nodes) 
- hosts: '192.178.92.[10:100]'

* When playbook tasks are applied on ip address 192.178.01.* to 192.178.50.*
- hosts: '192.178.[01:50].*'

* When playbook tasks are applied on ip address 190.*.*.* to 192.*.*.*
- hosts: '[190:192].*.*.*'

# cat /etc/resolv.conf (Stores ip address of DNS server)

# To implement ansible task on the localuser(student) of any node/server
- make an entry in inventory file like

[student_nodeA]
nodeA ansible_host=172.16.107.131 ansible_user=student

then we can just put 
- hosts: student_nodeA

# If we do not have any ip address (let say 192.168.92.140) registered in our /etc/host file but still if we require it we can 
just make an entry in the inventory file like:
- serverd ansible_host=192.168.92.140
- serverd.example.com ansible_host=192.168.92.140

and then we are good to go with
hosts: serverd
or
hosts: 192.168.92.140
or
hosts: serverd.example.com

# to use the server-nodes which are not in any group in the inventory file automatically comes under "ungrouped" group
so we can use the ungrouped managed host all at once as:
- hosts: ungrouped

# to exclude any or some of the hosts or group from a group of servers/managed host, we use !

- hosts: groupname,excluded_node_name/ip
- hosts: prod,!nodeC        [means all servers of group prod but not on nodeC]
- hosts: testing_servers,!stag        [means all servers of the nested group testing_servers but not on servers of group stag]

# To implement the play or task on the common hosts of two or more groups we use &
 - hosts: dev,&stag     [fetches the common server from both the groups and then execute the play on that common server]
