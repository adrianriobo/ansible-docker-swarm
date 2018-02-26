# ansible-docker-swarm

Ansible playbooks for configure docker swarm cluster. This project is intendeed for Centos distribution
docker version will be fixed throught vars file.

Also the approach for using this project is local execution (i.e. inside remote-exec provisioner of terraform)

## Playbooks

- **dockerswarminit.yml**: First initialization. Install docker and swarm init, add firewall rules. Export tokens in $HOME as manager_token and worker_token.


- **dockerswarmaddnode.yml**: Add node to the cluster. It communicates throught scp to get tokens. It uses vars to check which role will be used as cluster node.

## Sample invocation

First manager node:

ansible-playbook dockerswarminit.yml --extra-vars "ansible_sudo_pass=${LOCAL_ADMIN_PASS} swarm_master_ip=S{PRIVATE_FIXED_IP}"

From any node (manager) in the cluster:

ansible-playbook dockerswarmaddnode.yml --extra-vars "ansible_sudo_pass=${LOCAL_ADMIN_PASS} \
                 swarm_master_ip=S{MASTER_PRIVATE_FIXED_IP} swarm_master_pass=${MASTER_USER_PASS} \ 
                 swarm_master_user=${MASTER_USER} node_type=**manager**"

From any node (worker) in the cluster:

ansible-playbook dockerswarmaddnode.yml --extra-vars "ansible_sudo_pass=${LOCAL_ADMIN_PASS} \
                 swarm_master_ip=S{MASTER_PRIVATE_FIXED_IP} swarm_master_pass=${MASTER_USER_PASS} \
                 swarm_master_user=${MASTER_USER} node_type=**worker**"



