# Ansible

> ⚠️ Trang còn sơ sài, cần cập nhật sang tiếng Việt

Ansible is an open source automation tool that can describe your IT infrastructure with simple declarative code. 

Most importantly it uses declarative push-based YAML code that allows devops teams to easily automate their Linux servers. Increasing efficiency while reducing the likelihood of human error.

Ansible works by making at least one machine to control node, it then connects to multiple manage nodes and sends them an ansible module over SSH which can configure that machine's dependencies, update network settings, provision databases.

> ***automate any job you do more than once***
> - wise devops guy says

Developers write playbooks that contain a series of jobs called plays each play is a set of instructions (tasks) that can be executed on one or more target hosts in order against all machines in parallel.

In addition, a playbook is item potent which means it won't actually make any changes unless it has to.

Ansible also has a massive ecosystem of prebuilt playbooks that can be accessed on Ansible Galaxy.

Ansible Vault to store sensitive information like a password securely.

```yml
- name: Create Linode Instance
  hosts: localhost 
  vars:
	  hello: hi mom!
  vars_files:
	  - ./group_vars/vars.yml
  tasks:
	  - name: create a linode instance
	    linode-cloud.instance:
	    api_token: "{{ api_token }}"
	    label: my-ansible-linode
	    type: g6-nanode-1
	    region: us-east
	    image: linode/ubuntu22.04
	    root_pass: "{{ password }}"
	    state: present
```

```sh
ansible-playbook deploy.yml
```
