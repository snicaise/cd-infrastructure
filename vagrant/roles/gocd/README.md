# GoCD
[![Build Status](https://travis-ci.org/snicaise/gocd.svg?branch=master)](https://travis-ci.org/snicaise/gocd)

Installs and configures [Go CD](http://www.go.cd) on Ubuntu.

## Requirements
- This role requires Ansible 1.9 or higher.
- SSH keys, primarily for git SSH access.

Generate a new SSH key pair for git :
- [github deploy keys](https://developer.github.com/guides/managing-deploy-keys/#deploy-keys)
- [bitbucket deployment keys](https://confluence.atlassian.com/bitbucket/use-deployment-keys-294486051.html)
- [gitlab deploy keys](http://doc.gitlab.com/ce/ssh/README.html).

## Role Variables

Name                      | Default                               | Description
------------------------- | ------------------------------------- | ----------------------------------------------------------------------
gocd_version              | 15.2.0-2248                           | Version of Go CD to install
gocd_server_host          | localhost                             | Used by go-agent to connect to go-server
gocd_server_port          | 8153                                  | Which port the go-server binds to, and accepts HTTP connections from.
gocd_agent_count          | 1                                     | Install multiple agents on the same machine
gocd_admin_user           | admin                                 | Default admin user
gocd_admin_password       | password                              | Default admin password
gocd_server_password_file | /etc/go/passwd                        | Password file path
gocd_java_home            | /usr/lib/jvm/java-7-openjdk-amd64/jre | JAVA_HOME used by go-server and go-agent
gocd_ssh_private_key      | ~/.ssh/id_gocd                        | GIT SSH private key
gocd_ssh_public_key       | ~/.ssh/id_gocd.pub                    | GIT SSH public key
gocd_ssh_know_hosts       | github.com, bitbucket.org             | Domain to import as a known host.

## Dependencies
None.

## Example Playbook

Install Go CD server and agent on same host :

```
- hosts: all
  roles:
    - { role: gocd, gocd_server: true, gocd_agent: true }
```

Install 2 Go CD agents on a different host, specifying go-server address :

```
- hosts: goserver
  roles:
    - { role: gocd, gocd_server: true, gocd_agent: false }

- hosts: goagents-java
  roles:
    - { role: gocd, gocd_server: false, gocd_agent: true, gocd_server_host: "{{ hostvars['goserver']['ansible_eth0']['ipv4']['address'] }}", gocd_agent_count: 2 }
```

## License
MIT
