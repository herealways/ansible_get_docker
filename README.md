## ansible-get-docker

This is an ansible role that installs and configures Docker on CentOS7, CentOS8 and Fedora 30.  
By default, it will install:
* latest version of docker-ce (For CentOS8 it may not be the case since it use dnf localinstall)
* latest version of docker-compose through pip3
* latest version of python3.6 when the platform is CentOS7 and CentOS8
* latest version of docker and selinux module through pip3

It can also:
* Add users to docker group.
* Use custom custom setting files.

**Note:** For CentOS8, it will run something like `firewall-cmd --zone=public --add-masquerade --permanent` to [fix broken DNS in container](https://serverfault.com/questions/987686/no-network-connectivity-to-from-docker-ce-container-on-centos-8).

## Example
```yml
---

# get-docker.yml
- hosts: example
  become: true
  tasks:
    - import_role:
        name: herealways.get-docker
```

Run: ansible-playbook get-docker.yml

## How-to
### Installing a specific version of Docker

Uncomment the "docker_upstream_repo_*" variable in defaults/main.yml and enter a version number, or provide this variable in other ways to overwrite default value.  
(like -e option in ansible-playbook or "vars:" in your playbook)  
**Note:** For CentOS8, it will download related rpms and install them using dnf localinstall. You need to change the url defined in defaults/main.yml if you want to install a specific version of docker on CentOS8.

### Adding users to docker group
Uncomment the "docker_users" variable and in defaults/main.yml and enter users,
or provide this variable in other ways to overwrite default value.

### Templating daemon.json file

Firstly, set the "docker_set_daemon_file" variable to true in defaults/main.yml,
or change this variable in other ways to overwrite default value.  
Secondly, edit the templates/etc/docker/daemon.json.j2 file to what you want.  

### Using (self-signed) credentials for docker registry

Firstly, set the "docker_use_certificates" and docker_set_login_credentials variables to true in defaults/main.yml, or change this variable in other ways to overwrite default value.  
Secondly, put your certificates under templates/etc/docker/certs.d/  
Thirdly, edit the "docker_registry" variable in defaults/main.yml.  

### Logging in a docker registry on the host
Firstly, set the docker_set_login_credentials variables to true in defaults/main.yml,
or change this variable in other ways to overwrite default value.  
Secondly, edit the "docker_registry" variable in defaults/main.yml.
