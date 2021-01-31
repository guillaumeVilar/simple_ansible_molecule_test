# simple_ansible_molecule_test
Repository that shows an example of an Ansible playbook that deploy an Apache HTTP server, and how to test the playbook using Molecule.

Based on Chapter 13 of "Ansible for DevOps" book. 


## Install: 
```
python3 -m venv .venv
source .venv/bin/activate

pip3 install "molecule molecule-docker"
pip3 install yamllint ansible-lint
```

## Run the Ansible playbook manually
Modify the inventory file to point to your server, then run the playbook using: 
```
ansible-playbook -i inventory.yml apache_deploy.yml
```

## Run the Molecule test: 
```
molecule test
```


## Additional Molecule commands: 
```
molecule init scenario     
molecule converge
```

## Ansbile-lint command:
```
ansible-lint apache_deploy.yml
```



## Result of the molecule test: 
```
$ molecule test 
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_ef
fect, verify, cleanup, destroy
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
COMMAND: set -e
yamllint apache_deploy.yml
ansible-lint

INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) deletion to complete] *******************************
ok: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '666265232385.30650', 'results_file': '/Users/guillaumevi
lar/.ansible_async/666265232385.30650', 'changed': True, 'failed': False, 'item': {'command': '', 'image': 'geerlingguy/docker-ubu
ntu2004-ansible:latest', 'name': 'instance', 'pre_build_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgro
up:ro']}, 'ansible_loop_var': 'item'})

TASK [Delete docker network(s)] ************************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /Users/guillaumevilar/Documents/ansible/simple_ansible_molecule_test/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item={'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_b
uild_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']})

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_build_i
mage': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_b
uild_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_build_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/geerlingguy/docker-ubuntu2004-ansible:latest) 

TASK [Create docker network(s)] ************************************************

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_build_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '656060991736.30744', 'results_file': '/Users/guillaumevilar/.ansible_async/656060991736.30744', 'changed': True, 'failed': False, 'item': {'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_build_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Update apt cache (on Debian).] *******************************************
changed: [instance]

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Ensure Apache is installed.] *********************************************
changed: [instance]

TASK [Copy a web page.] ********************************************************
changed: [instance]

TASK [Ensure Apache is running and starts at boot.] ****************************
changed: [instance]

RUNNING HANDLER [restart apache] ***********************************************
changed: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=7    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Update apt cache (on Debian).] *******************************************
ok: [instance]

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Ensure Apache is installed.] *********************************************
ok: [instance]

TASK [Copy a web page.] ********************************************************
ok: [instance]

TASK [Ensure Apache is running and starts at boot.] ****************************
ok: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance]

TASK [Verify Apache is serving web requests.] **********************************
ok: [instance]

PLAY RECAP *********************************************************************
instance                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '894559680993.32087', 'results_file': '/Users/guillaumevilar/.ansible_async/894559680993.32087', 'changed': True, 'failed': False, 'item': {'command': '', 'image': 'geerlingguy/docker-ubuntu2004-ansible:latest', 'name': 'instance', 'pre_build_image': True, 'privileged': True, 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup:ro']}, 'ansible_loop_var': 'item'})

TASK [Delete docker network(s)] ************************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```