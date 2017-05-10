Replicated
==========

This role helps install Replicated and provides install automation for applications.

Requirements
------------

This role requires Ansible 2.3 or higher. The platform platform requirements are found in the metadata file.

Required playbooks are documented in `requirements.yml` and can be installed via

```
ansible-galaxy install -r /etc/ansible/roles/replicated.ansible/requirements.yml
```

Role Variables
--------------

The following variables can be passed to the role:

```
  #
  # Replicated or Replicated Operator install
  #
  replicated_version: 2.7                       # optional replicated version
  replicated_docker_version: 1.12.3             # optional docker version
  replicated_install: replicated                # optional what to install, either replicated (the default) or operator
  replicated_private_address: 10.0.7.20         # server private address, if left blank defaults to ansible_default_ipv4.address
  replicated_public_addres: 10.0.7.20           # server public address, if left blank defaults to ansible_default_ipv4.address
  replicated_daemon_token: JSWSOzcOmiaeN        # optional daemon_token, if left blank one will be generated
  replicated_daemon_address: 10.0.7.20         # required if performing an operator only install

  #
  # Automation support to install both Replicated and your application and
  # settings by providing the replicated configuration file, settings file
  # any any extra files such as keys/certs and a license.
  # 
  # To learn more about Replicated automation see
  # https://www.replicated.com/docs/kb/developer-resources/automate-install
  #
  replicated_automation_settings_file: settings.conf  # written to /etc/settings.conf
  replicated_automation_conf_file: replicated.conf    # written to /etc/replicated.conf
  replicated_automation_extra_files:                  # extra files needed for automation
  - { src: '/tmp/cert', dest: '/tmp/http-cert' }
  - { src: '/tmp/key', dest: '/tmp/http-key' }
  - { src: '/tmp/license.rli', dest: '/tmp/license.rli' }
```
  
Examples
--------

1) Install the latest Replicated.

```
- name: Install latest Replicated
  hosts: replicated
  roles:
  - replicated
```

2) Install a Replicated Operator node pointing back to a previously setup Replicated host

```
- name: Install Replicated
  hosts: replicated-operator
  vars:
    replicated_install: operator
    replicated_daemon_address: 10.0.7.3
    replicated_daemon_token: JSWSOzcOmiaeNdt3Bb1Xm7B7Kakj
  roles:
  - replicated
```

3) Install Replicated with an existing application

Automation support files are put into place prior to Replicated installing and on the install used to configure and start the application. To use the Replicated automation start by copying the sample replicated.conf and settings.conf from the templates folder and customize for your application.

Settings can be gathered from a running instance via the [Replicated CLI](https://www.replicated.com/docs/reference/replicated-cli) by running `replicated app <appid> settings`.

```
- name: Install Replicated
  hosts: replicated
  vars:
    replicated_daemon_token: JSWSOzcOmiaeNdt3Bb1Xm7B7Kakj
    replicated_automation_settings_file: /tmp/settings.conf
    replicated_automation_conf_file: /tmp/replicated.conf
    replicated_automation_extra_files:
    - { src: '/tmp/key', dest: '/tmp/key' }
    - { src: '/tmp/cert', dest: '/tmp/cert' }
    - { src: '/tmp/license.rli', dest: '/tmp/license.rli' }
  roles:
  - replicated
```


Dependencies
------------

None
