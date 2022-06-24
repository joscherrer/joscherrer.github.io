---
id: 52mva7i06ojuu04lf5x5xeh
title: Ansible
desc: ''
updated: 1656078342891
created: 1655753362795
---

## Requirements

- Setup your ssh key
- Install the collection
```bash
mkdir -p "$HOME/dev/ansible_collections"
ansible-galaxy collection install -p "$HOME/dev" git@github.com:jonsible/iac.git
```
- Clone the inventory
```
git clone git@github.com:joscherrer/inventory.git ~/dev/inventory
```
- Go into the 

## Bootstrap

```bash
ansible-playbook -i inventory/bare.yml jonsible.iac.bootstrap
```


## Hardening

```bash
ansible-playbook -i inventory/bare.yml jonsible.iac.hardening
```

## nginx

```bash
ansible-playbook -i inventory/bare.yml
```