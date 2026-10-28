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

