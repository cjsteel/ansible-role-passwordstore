
ansible-role-{{ short-role-name }}
=======================

An Ansible role to install and manage **{{ short-role-name }}**

## Version

0.0.0

Description
-----------

Unusable at this time

## Notes

You may want to remove / uninstall older versions before installing newer ones.


Resources
---------

-  https://{{ short-role-name }}.com/

Requirements
------------

Controller
----------

### Controller preparation

#### Setup ssh agent

```shell
eval `/usr/bin/ssh-agent -s`
/usr/bin/ssh-add
```

#### test connectivity

#### remove any stale keys

```shell
ssh-keygen -f "$HOME/.ssh/known_hosts" -R workstation-001
ssh-keygen -f "$HOME/.ssh/known_hosts" -R 192.168.11.22
```

#### Confirm connectivity

### Ansible command example

```shell
ansible-playbook systems.yml -i inventory/dev --ask-vault-pass -v --limit workstation-001
```

## Options

## Issues

## Role Variables


### roles/{{ short-role-name }}/defaults/main.yml

```yaml
---
# file: roles/{{ short-role-name }}/defaults/main.yml
#

# Requirements
#
# set the value of project_deployment_user_name in your projects group_vars/all/defaults.yml
#
# ---
# file: group_vars/all/defaults.yml
#
# project_deployment_user_name: 'deployment_user_name'

{{ short-role-name }}_controller_home   : '{{ fact_controler_home }}'
{{ short-role-name }}_remote_user       : '{{ project_deployment_user_name }}'
{{ short-role-name }}_remote_users_home : '/home/users/{{ {{ short-role-name }}_remote_user }}'

# probably set in this or a dependent role

{{ short-role-name }}_state             : 'absent' # 'present' # 'absent'
#{{ short-role-name }}_installation_type : 'local' # 'url'
{{ short-role-name }}_app_name          : '{{ short-role-name }}'
{{ short-role-name }}_package_name      : '{{ short-role-name }}-desktop'
{{ short-role-name }}_ver               : '2.0.3' #'2.6.2' # 2.0.3
{{ short-role-name }}_arch              : 'amd64'
{{ short-role-name }}_package_type      : 'deb'

# calculated vars

# example below builds "{{ short-role-name }}-desktop-2.6.2-amd64.deb"
{{ short-role-name }}_package_filename  : '{{ {{ short-role-name }}_package_name }}-{{ {{ short-role-name }}_ver }}-{{ {{ short-role-name }}_arch }}.{{ {{ short-role-name }}_package_type }}'
{{ short-role-name }}_controller_package_path : '{{ fact_controller_home }}/src/Ubuntu/16.04/{{ short-role-name }}/{{ {{ short-role-name }}_ver }}/{{ {{ short-role-name }}_package_filename }}'

{{ short-role-name }}_taget_node_package_dir  : '{{ {{ short-role-name }}_remote_users_home }}/src/Ubuntu/16.04/{{ short-role-name }}/{{ {{ short-role-name }}_ver }}'
{{ short-role-name }}_taget_node_package_path : '{{ {{ short-role-name }}_taget_node_package_dir }}/{{ {{ short-role-name }}_package_filename }}'
```

### roles/{{ short-role-name }}/tests/vagrant.yml

```shell
---
# file: roles/{{ short_role_name }}/tests/vagrant.yml

- hosts: all
  remote_user: ubuntu
  become: false # or local directory creation will fail
  pre_tasks:

    - set_fact: fact_controller_user="{{ lookup('env','USER') }}"
    - debug: var=fact_controller_user

    - set_fact: fact_controller_home="{{ lookup('env','HOME') }}"
    - debug: var=fact_controller_home

  vars:

    - {{ short-role-name }}_controller_home   : '{{ fact_controler_home }}'
    - {{ short-role-name }}_remote_user       : 'ubuntu'
    - {{ short-role-name }}_remote_users_home : '/home/ubuntu'

# probably set in this or a dependent role

    - {{ short-role-name }}_state                   : 'absent' # 'present' # 'absent'
#    - {{ short-role-name }}_installation_type       : 'local' # 'url'
    - {{ short-role-name }}_app_name                : '{{ short-role-name }}'
    - {{ short-role-name }}_package_name            : '{{ short-role-name }}-desktop'
    - {{ short-role-name }}_ver                     : '2.0.3' #'2.6.2' # 2.0.3
    - {{ short-role-name }}_arch                    : 'amd64'
    - {{ short-role-name }}_package_type            : 'deb'
    # example below builds "{{ short-role-name }}-desktop-2.6.2-amd64.deb"
    - {{ short-role-name }}_package_filename        : '{{ {{ short-role-name }}_package_name }}-{{ {{ short-role-name }}_ver }}-{{ {{ short-role-name }}_arch }}.{{ {{ short-role-name }}_package_type }}'

    - {{ short-role-name }}_controller_package_path : '{{ fact_controller_home }}/src/Ubuntu/16.04/{{ short-role-name }}/{{ {{ short-role-name }}_ver }}/{{ {{ short-role-name }}_package_filename }}'

    - {{ short-role-name }}_taget_node_package_dir  : '{{ {{ short-role-name }}_remote_users_home }}/src/Ubuntu/16.04/{{ short-role-name }}/{{ {{ short-role-name }}_ver }}'
    - {{ short-role-name }}_taget_node_package_path : '{{ {{ short-role-name }}_taget_node_package_dir }}/{{ {{ short-role-name }}_package_filename }}'

  roles:
    - ../../
```



NOT IN USE AT THIS TIME - Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles. Examples:

* [ ansible-role-acemenu ]( https://github.com/cjsteel/ansible-role-acemenu )
* [ ansible-role-ensure_dirs ]( https://github.com/csteel/ansible-role-ensure_dirs )
* [ ansible-role-skel ]( https://github.com/csteel/ansible-role-skel )

### NOT IN USE AT THIS TIME  - ensure_dirs

#### NOT IN USE AT THIS TIME - roles/ansible-role-{{ short-role-name }}/meta/main.yml

```yaml
---
# file: roles/ansible-role-{{ short-role-name }}/meta/main.yml in dependant role
dependencies:

- { role: ensure_dirs, 
        ensure_dirs_on_remote: "{{ {{ short-role-name }}_remote_directories_description }}",
        ensure_dirs_on_local : "{{ {{ short-role-name }}_local_directories_description }}"
  }
```

#### NOT IN USE AT THIS TIME 

#### roles/ansible-role-{{ short-role-name }}/defaults/main.yml example

```yaml
{{ short-role-name }}_remote_directories_description:

  {{ short-role-name }}_installation_resources_dir:

    state       : "present"					# absent
    path        : "sys/sw"					# relative to Ansible users home
    owner       : "{{ ansible_ssh_user }}"
    group       : "{{ ansible_ssh_user }}"
    mode        : "0644"

{{ short-role-name }}_local_directories_description:

  {{ short-role-name }}_installation_resources_dir:

    state       : "present"					# absent
    path        : "sys/sw/" 				# relative to Ansible users home dir
    owner       : "{{ ansible_ssh_user }}"
    group       : "{{ ansible_ssh_user }}"
    mode        : "0644"
```

## role playbook example

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

### project_name/{{ short-role-name }}.yml

```yaml
---
# file: project_name/ants.yml

- hosts: {{ short-role-name }}
  become: false
  gather_facts: true
  pre_tasks:

    - set_fact: fact_controller_user="{{ lookup('env','USER') }}"
    - debug: var=fact_controller_user

    - set_fact: fact_controller_home="{{ lookup('env','HOME') }}"
    - debug: var=fact_controller_home

  roles:

    - { role: {{ short-role-name }}, {{ short-role-name }}_state: 'absent', {{ short-role-name }}_ver: '2.0.3' }
    - { role: {{ short-role-name }}, {{ short-role-name }}_state: 'present', {{ short-role-name }}_ver: '2.6.2' }

#    - { role: cjsteel.ansible-role-{{ short-role-name }}, {{ short-role-name }}_state: 'absent', {{ short-role-name }}_ver: '2.0.3' }
#    - { role: cjsteel.ansible-role-{{ short-role-name }}, {{ short-role-name }}_state: 'present', {{ short-role-name }}_ver: '2.6.2' }
```

## main playbook example

### project_name/systems.yml

```yaml
---
- hosts: all
  become: false

- include: deployment_user.yml

- include: shorewall.yml

- include: ldap.yml

- include: workstation.yml

- include: {{ short-role-name }}.yml
...
```

## Ansible command examples

### without sudo

```shell
ansible-playbook -i inventory/dev systems.yml --limit ace-ws-77
```

### with sudo

```shell
ansible-playbook -i inventory/dev systems.yml --limit ace-ws-77 --ask-become-pass
```



## License

MIT

## Author Information

Christopher Steel on behalf of ACElabs  
Systems Administrator  
McGill Centre for Integrative Neuroscience  
Montreal Neurological Institute  
E-mail: christopherDOTsteel@mcgill.ca  
[mcin-cnim.ca](http://mcin-cnim.ca/)    
[theneuro.ca](http://www.mcgill.ca/neuro/)   

## Open Science

The Montreal Neurological Institute has adopted the principles of Open Science. We are inspired by the likes of the Allen Institute for Brain Science, the National Institutes of Health's Human Connectome project, and the Human Genome project. For additional information please see [open science at the neuro]( https://www.mcgill.ca/neuro/open-science-0).



![MCIN](imgs/mcin-logo-brain-140x116.png)          ![neuro](imgs/neuro-logo-160x116.png)  
