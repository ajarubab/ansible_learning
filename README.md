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

-- Run as ansible-playbook
    ansible-playbook  '/workspaces/ansible-learning/playbooks/create_dir.yml'

-- Run as ansible-navigator
    ansible-navigator run  '/workspaces/ansible-learning/playbooks/create_dir.yml' --ee true --pae false --ce podman --eei 'registry.redhat.io/ansible-automation-platform-25/ee-supported-rhel8:latest'

--ee true : Execution Environment enabled
-- pae false : Playbook Artifact Enable set false here 
-- ce podman : container Engine is podman
--eei : Execution Environment Image