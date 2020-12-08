## [![Nabla](https://debops.org/images/debops-small.png)](https://github.com/AlbanAndrieu) roles/alban_andrieu_jenkins_slave

This file was generated by Ansigenome. Do not edit this file directly but instead have a look at the files in the ./meta/ directory.

[![License](http://img.shields.io/:license-apache-blue.svg?style=flat-square)](http://www.apache.org/licenses/LICENSE-2.0.html)
[![Travis CI](https://img.shields.io/travis/AlbanAndrieu/ansible-jenkins-slave.svg?style=flat)](https://travis-ci.org/AlbanAndrieu/ansible-jenkins-slave)
[![Branch](http://img.shields.io/github/tag/AlbanAndrieu/ansible-jenkins-slave.svg?style=flat-square)](https://github.com/AlbanAndrieu/ansible-jenkins-slave/tree/master)
[![Donate](https://img.shields.io/gratipay/AlbanAndrieu.svg?style=flat)](https://www.gratipay.com/~AlbanAndrieu)
<!--[![Ansible Galaxy](https://img.shields.io/badge/galaxy-alban.andrieu.jenkins--slave-660198.svg?style=flat)](https://galaxy.ansible.com/alban.andrieu/jenkins-slave/)-->
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-alban.andrieu.jenkins--slave-660198.svg?style=flat)](https://galaxy.ansible.com/alban.andrieu/jenkins-slave)
[![Platforms](http://img.shields.io/badge/platforms-ubuntu-lightgrey.svg?style=flat)](#)
[![Docker Hub](https://dockerbuildbadges.quelltext.eu/status.svg?organization=nabla&repository=ansible-jenkins-slave-docker)](https://hub.docker.com/r/nabla/ansible-jenkins-slave-docker/)

Ensures that the basic requirements are properly installed (using `apt`) and configured for a jenkins slave to build, test package and deploy your project

This playbook is be used by [Docker Hub](https://hub.docker.com) to create a [Docker](http://docker.io) image.

See role [ansible-jenkins-slave-docker](https://github.com/AlbanAndrieu/ansible-jenkins-slave-docker) and image [ansible-jenkins-slave-docker](https://hub.docker.com/r/nabla/ansible-jenkins-slave-docker/)

# Table of contents

<!-- toc -->

  * [Requirements](#requirements)
- [Quality tools](#quality-tools)
  * [Installation](#installation)
  * [Role dependencies](#role-dependencies)
  * [Documentation](#documentation)
  * [Role variables](#role-variables)
  * [Detailed usage guide](#detailed-usage-guide)
  * [Testing](#testing)
- [Update README.md Table of Contents](#update-readmemd-table-of-contents)
  * [Contributing](#contributing)
  * [Authors and license](#authors-and-license)
- [License](#license)
  * [Feedback, bug-reports, requests, ...](#feedback-bug-reports-requests-)

<!-- tocstop -->

### Requirements

Tools which might be needed by [Eclipse](https://www.eclipse.org), like jdk, maven...
See available playbook on [GitHub](https://github.com/search?p=3&q=user%3AAlbanAndrieu+ansible%2A&type=Repositories)

## Quality tools

See [pre-commit](http://pre-commit.com/)
Run `pre-commit install`

Run `pre-commit run --all-files`

### Installation

This role requires at least Ansible `v2.3.1.0`. To install it, run:

Using `ansible-galaxy`:
```shell
$ ansible-galaxy install alban.andrieu.jenkins-slave
```

Using `arm` ([Ansible Role Manager](https://github.com/mirskytech/ansible-role-manager/)):
```shell
$ arm install alban.andrieu.jenkins-slave
```

Using `git`:
```shell
$ git clone https://github.com/AlbanAndrieu/ansible-jenkins-slave.git
```

### Role dependencies

- `alban.andrieu.common`
- `geerlingguy.git`
- `alban.andrieu.cpp`
- `alban.andrieu.xvbf`
- `alban.andrieu.selenium`
- `maven`
- `chrome`
- `alban.andrieu.tomcat`
- `alban.andrieu.jboss`
### Documentation

More information about `alban.andrieu.jenkins-slave` can be found in the
TODO [official alban.andrieu.jenkins-slave documentation](https://docs.debops.org/en/latest/ansible/roles/ansible-jenkins-slave/docs/).


### Role variables

List of default variables available in the inventory:

```YAML
---

jenkins_name: jenkins
jenkins_user: jenkins
# python -c "from passlib.hash import sha512_crypt; import getpass; print sha512_crypt.encrypt(getpass.getpass())"
#jenkins1234
#http://docs.ansible.com/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module
jenkins_password: "$6$rounds=656000$ITswjqU77/RgGDEv$Vbv6Pw5UJEQQGXwZ4JiR0WXsZVSNAHY7NuWgid.yGLIxro27nZt7CIMwQrh4encLm9Db1RDscEjC1T9ldCgx61"
jenkins_group: jenkins
#jenkins_shell: "/bin/false"
jenkins_shell: "/bin/bash"
jenkins_home: /var/lib/jenkins              # Jenkins home location
#NIS issue jenkins_home: /home/jenkins
#jenkins_home: "/home/jenkins"
jenkins_slave_home: "{{ jenkins_home }}"
jenkins_slave_directory: "{{ jenkins_slave_home }}/jenkins-slave"

jenkins_jdk7_enable: no
jenkins_jdk8_enable: no
jdk_dir_tmp: "/tmp" # or override with "{{ tempdir.stdout }} in order to have be sure to download the file"
jdk_owner: "root"
jdk_group: "{{ jdk_owner }}"

jenkins_http_host: 127.0.0.1                # Set HTTP host
jenkins_http_port: 8000                     # Set HTTP port
jenkins_prefix: "/"
jenkins_url: "http://{{ jenkins_http_host }}:{{ jenkins_http_port }}{{jenkins_prefix}}"
jenkins_slave_name: swarm-{{ ansible_hostname }}

# Package states: present or installed or latest
jenkins_pkg_state: present
# Repository states: present or absent
jenkins_repository_state: present

jenkins_ssh_key_file: "~/.ssh/id_rsa"        # Set private ssh key for Jenkins user (path to local file)
jenkins_ssh_authorized_keys_fingerprints: [] # Set known authorized keys for ssh
# Alban Andrieu
#  - "ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAIEAio3SOQ9yeK6QfKqSFNKyTasuzjStxWevG1Vz1wgJIxPF+KB0XoMAPD081J+Bzj2LCDRSWisNv2L4xv2jbFxW/Pl7NEakoX47eNx3U+Dxaf+szeWBTryYcDUGkduLV7G8Qncm0luIFd+HDIe/Qir1E2f56Qu2uuBNE6Tz5TFt1vc= Alban"
#ssh-keygen -F $IP
#ssh-keyscan -H $IP
jenkins_ssh_fingerprints:                   # Set known hosts for ssh
  - "bitbucket.org,131.103.20.167 ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAubiN81eDcafrgMeLzaFPsw2kNvEcqTKl/VqLat/MaB33pZy0y3rJZtnqwR2qOOvbwKZYKiEO1O6VqNEBxKvJJelCq0dTXWT5pbO2gDXC6h6QDXCaHo6pOHGPUy+YBaGQRGuSusMEASYiWunYN0vCAI8QaXnWMXNMdFP3jHAJH0eDsoiGnLPBlBp4TNm6rYI74nMzgz3B9IikW4WVK+dc8KZJZWYjAuORU3jc1c/NPskD2ASinf8v3xnfXeukU0sJ5N6m5E8VLjObPEO+mN2t/FZTMZLiFqPWc/ALSqnMnnhwrNi2rbfg/rd/IpL8Le3pSBne8+seeFVBoGqzHM9yXw=="
  - "github.com,204.232.175.90 ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ=="

jenkins_remote_thinbackup: "/backup/thinBackup"

home_url: "http://localhost"
nexus_url: "{{ home_url }}:8081"
npm_nexus_npm_url: "{{ nexus_url }}/nexus/content/npm/registry.npmjs.org/"
#npm_phantomjs_cdnurl: "{{ home_url }}:7070/download/phantomjs"
npm_strict_ssl: "false"
#npm_prefix: "/usr/local"
#maven_repository: "{{ jenkins_slave_home }}/repository"
maven_repository: "/usr/share/maven-repo"
#TODO sudo chmod 755 -R /usr/share/maven-repo

bower_directory: "bower_components"
bower_analytics: "false"
bower_timeout: 120000
bower_url: "{{ home_url }}:5678"
bower_registry_url: "{{ bower_url }}"
bower_register_url: "{{ bower_url }}"
bower_publish_url: "{{ bower_url }}"

shell_git_configure_enabled: yes         # Enable git configuration
shell_git: []
  # Additional properties: 'shell_git_machine, shell_git_login, shell_git_email, shell_git_password, shell_git_name, shell_git_path, shell_git_ssl, shell_git_meld_enabled, shell_git_editor'
  # - shell_git_machine: 'localhost',
  #   shell_git_login: 'jenkins',
  #   shell_git_email: 'jenkins@localhost.com',
  #   shell_git_password: 'todo',
  #   # Optional.
  #   shell_git_name: '{{ shell_git_login }}',
  #   shell_git_path: '/usr/bin',
  #   shell_git_ssl: false,
  #   shell_git_meld_enabled: yes,
  #   shell_git_editor: "gedit",

nodejs_version: "6.4.0"
nodejs_enabled: true
epel_repo_enabled: false

proxy_repo_enabled: false
proxy_repo_server: "localhost"
proxy_repo_url: "http://{{ proxy_repo_server }}:3142"

docker_py_version: "1.9.0"
docker_users: "{{ jenkins_user }}"

docker_files_generated_directory: "./"
docker_files_enable: no
docker_volume_directory: "{{ jenkins_home }}"
docker_working_directory: "/tmp/ansible"
#docker_working_directory: "{{ docker_volume_directory }}"
docker_image_name: "nabla/ansible-jenkins-slave"
```

List of internal variables used by the role:

    cpp_version
    jdk_dir
    cpp_dir
    mvn_dir
### Detailed usage guide

Describe how to use in more detail...

### Testing
```shell
$ ansible-galaxy install alban.andrieu.jenkins-slave
$ vagrant up
```

Update README.md Table of Contents
----------------------------------------------


  * [github-markdown-toc](https://github.com/jonschlinkert/markdown-toc)
  * With [github-markdown-toc](https://github.com/Lucas-C/pre-commit-hooks-nodejs)

`
npm install --save markdown-toc
`

### Contributing

The [issue tracker](https://github.com/AlbanAndrieu/ansible-jenkins-slave/issues) is the preferred channel for bug reports, features requests and submitting pull requests.

For pull requests, editor preferences are available in the [editor config](.editorconfig) for easy use in common text editors. Read more and download plugins at <http://editorconfig.org>.

In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests and examples for any new or changed functionality.

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

### Authors and license

`roles/alban_andrieu_jenkins_slave` role was written by:

- [Alban Andrieu](fr.linkedin.com/in/nabla/) | [e-mail](mailto:alban.andrieu@free.fr) | [Twitter](https://twitter.com/AlbanAndrieu) | [GitHub](https://github.com/AlbanAndrieu)

License
-------

- License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)

### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/AlbanAndrieu/ansible-jenkins-slave/issues)!

***

This role is part of the [Nabla](https://github.com/AlbanAndrieu) project.
README generated by [Ansigenome](https://github.com/nickjj/ansigenome/).

***

Alban Andrieu

[linkedin](fr.linkedin.com/in/nabla/)
