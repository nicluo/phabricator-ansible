# Phabricator on Ubuntu with Ansible

This ansible script demonstrates the minimum needed to setup Phabricator using Ansible.

This makes it easy for anyone to get started with Phabricator quickly and easily.

Where possible, roles are taken from Ansible Galaxy, so that any extra configuration or scripting is specifically for Phabricator.

Basic structure adapted from: https://gist.github.com/sparrc/b4eff48a3e7af8411fc1

## Vagrant Provisioning

Using a VagrantBox to provision a local install makes it easy to debug and modify the playbook.

`vagrant-hostmanager` sets a hostname for the local Phabricator install. This allows us to use a convenient url like `phabricator.dev` and have Vagrant automatically manage our `/etc/hosts` file.

First, install Ansible Galaxy requirements by running `ansible-galaxy install -r requirements.yml`.


Use `vagrant up` to boot, `vagrant provision` to re-run the playbook.
