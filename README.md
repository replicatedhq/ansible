Replicated
==========

This role helps install Replicated and provides install automation for applications.

Requirements
------------

This role requires Ansible 1.9 or higher. The platform platform requirements are found
in the metadata file.

Role Variables
--------------

The following variables can be passed to the role:

```
  #
  # Replicated or Replicated Operator install
  #
  private_address: 10.0.7.20         # server private address, if left blank defaults to ansible_default_ipv4.address
  public_addres: 10.0.7.20           # server public address, if left blank defaults to ansible_default_ipv4.address
  proxy: http://10.0.7.2:3128        # optional proxy http address
  docker_version: 1.12.3             # optional docker version, if left blank uses latest from install script
  install: replicated                # optional what to install, either replicated (the default) or operator
  install_url: <url>                 # optional install_url script

  #
  # Automation support to install both Replicated and your application and
  # settings by providing the replicated configuration file, settings file
  # any any extra files such as keys/certs and a license.
  # 
  # To learn more about Replicated automation see
  # https://www.replicated.com/docs/kb/developer-resources/automate-install
  #
  automation_replicated_setting: settings.conf   # written to /etc/settings.conf
  automation_replicated_conf: replicated.conf    # written to /etc/replicated.conf
  automation_extra_files:                        # extra files needed for automation
  - { src: '/tmp/cert', dest: '/tmp/http-cert' }
  - { src: '/tmp/key', dest: '/tmp/http-key' }
  - { src: '/tmp/license.rli', dest: '/tmp/license.rli' }
```
  
Examples
--------

1) Install the latest Replicated

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
    install: operator
    daemon_endpoint: 10.0.7.3
    daemon_token: JSWSOzcOmiaeNdt3Bb1Xm7B7Kakj
  roles:
  - replicated
```

3) Install Replicated with an existing application

Automation support files are put into place prior to Replicated installing and on the install used to configure and start the application.

```
- name: Install Replicated
  hosts: replicated
  vars:
    daemon_token: JSWSOzcOmiaeNdt3Bb1Xm7B7Kakj
    automation_replicated_setting: /tmp/settings.conf
    automation_replicated_conf: /tmp/replicated.conf
    automation_extra_files:
    - { src: '/tmp/key', dest: '/tmp/key' }
    - { src: '/tmp/cert', dest: '/tmp/cert' }
    - { src: '/tmp/license.rli', dest: '/tmp/license.rli' }
  roles:
  - replicated
```


Dependencies
------------

None
