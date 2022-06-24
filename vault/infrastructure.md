---
id: ACmIaoniWX1pK5upZqwF4
title: Infrastructure
desc: ''
updated: 1655746961835
created: 1641149787317
---

This is the root of my infrastructure documentation.

## HQ

I have one server that acts as a control node for my whole infrastructure.
On this server is installed 

## Entrypoint

To manage my infrastructure, I have an [Ansible collection](https://github.com/jonsible/iac) that holds several roles and playbooks.

I also have an [inventory repository](https://github.com/jonsible/inventory) which is private, that holds :
- Bare machines, meaning bare-metal hosts and virtual machines that I pay for
- VMs, meaning virtual machines that I provisioned in my KVM infrastructure.

## Bootstrap

Usually, the first role to run when creating a new server is the [bootstrap role](https://github.com/jonsible/iac/tree/master/roles/bootstrap).

- It creates the default user (configured by the `default_user` variable)
- Installs a public key in the user's `authorized_keys` by retrieving it from `github` (configured by the `github_user` variable)
- Sets up passwordless sudo
- Installs required components

## Hardening

The [hardening role](https://github.com/jonsible/iac/tree/master/roles/hardening) will perform multiple steps to secure the server.

- Remove `ufw`
- Install iptables
- Install and configure `fail2ban`
- 