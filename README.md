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

-- Run as ansible-navigator with verbosity( -v,-vv,-vvv,-vvvv,-vvvvv,-vvvvvv)
    ansible-navigator run  '/workspaces/ansible-learning/playbooks/create_dir.yml' --ee true --pae false --ce podman --eei 'registry.redhat.io/ansible-automation-platform-25/ee-supported-rhel8:latest' -vvv

-- Run as ansible-navigator with verbosity( -v,-vv,-vvv,-vvvv,-vvvvv,-vvvvvv)
    ansible-navigator run  '/workspaces/ansible-learning/playbooks/create_dir.yml' --ee true --pae false --ce podman --eei 'registry.redhat.io/ansible-automation-platform-25/ee-supported-rhel8:latest' -vv

--ee true : Execution Environment enabled
-- pae false : Playbook Artifact Enable set false here 
-- ce podman : container Engine is podman
--eei : Execution Environment Image


(SMOKE TEST, DRY RUN) [Not actual run] - Recommended Before every Actual Run
- ansible-navigator run playbooks/usr_n_grp_mng.yml --check

(SYNTAX CHECK BEFORE RUN)
- ansible-navigator run playbooks/usr_n_grp_mng.yml --syntax-check

(Modify indentation or wrong keywords using vi editor)
- vi playbooks/usr_n_grp_mng.yml

(Running an artifact.json file to check its record of last playbook run output)
- ansible-navigator replay playbooks/usr_n_grp_mng-artifact-2026-10-22T11:34:41.152451+00.00.json

(Check the package firewalld in Integrated terminal of ansible on all nodes)
- ansible nodeA,nodeB,nodeC -m command -a 'rpmquery firewalld'

(Install the firewalld service using adhoc command on all 3 hosts)
- ansible nodeA,nodeB,nodeC -m dnf -a "name=firewalld state=present" -b
- ansible nodeA,nodeB,nodeC -m dnf -a "name=firewalld state=started enabled=yes" -b

(Uninstall the firewalld service using adhoc command on all 3 hosts)
- ansible nodeA,nodeB,nodeC -m dnf -a "name=firewalld state=absent" -b