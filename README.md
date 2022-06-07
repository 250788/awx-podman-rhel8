# Ansible role: AWX

Install and configures AWX using podman on Red Hat Enterprise Linux 8.

## Requirements

* Red Hat Enterprise Linux 8
* Python Pip
* Ansible
* Podman/Podman-compose


## Role Variables

awx_task_hostname=awx  
awx_web_hostname=awxweb  
postgres_data_dir=```"~/.awx/pgdocker"```   
host_port=8085  
host_port_ssl=443  
docker_compose_dir=```"~/.awx/awxcompose"```  
pg_username=awx  
pg_password=<pg_password>  
pg_database=awx  
pg_port=5432  
admin_user=admin  
admin_password=<awx_admin_password>  
create_preload_data=False  
secret_key=<awx_secret_key>  
awx_venv_dir="$HOME/.awx"  

## Example playbook  
```yaml
---
- name: Build and deploy AWX
  hosts: all
  roles:
    - {role: check_vars}
    - {role: image_build, when: "dockerhub_base is not defined"}
    - {role: image_push, when: "docker_registry is defined and dockerhub_base is not defined"}
    - {role: local_docker, when: "openshift_host is not defined and kubernetes_context is not defined"}
```

Execute AWX install playbook
```bash
ansible-playbook -i inventory playbooks/install.yml
```
