# Komga

This repository covers my personal [Komga](https://komga.org/) instance. It provides eBooks and comics in my private network.

The following chapters cover setup, upgrade and backup. I publish it because it might help anybody to setup a similar solution.

# Drawbacks

There are some drawbacks in this scenario which I would like to point out:

* In this scenario, Komga is running on HTTP without any transport layer encryption. Run this only in secure environments.
* docker-compose.yml is referencing to the latest images available in Docker Registry. In a enterprise environment, this is often seen as an anti pattern because it could lead to unintended updates and broken systems. However, in my specific scenario, I want to refer to the latest available image. If something fails unintended, I need to fix it.

# Requirements

* Ansible 2.7 or higher
* Docker runtime environment with docker compose
* A Linux user which is used by Docker to run the container and has access to your eBooks/comic files

# Setup

First of all, please instantiate Ansible var samples:

```
cd /YOUR/REPO/CHECKOUT/PATH/ansible
cp roles/install-komga/vars/main-sample.yml roles/install-komga/vars/main.yml
```

Open `roles/install-komga/vars/main.yml` in your prefered editor and adapt settings.

To execute this Ansible playbook, you can use following command:

```
ansible-playbook -i /PATH/TO/INVENTORY /YOUR/REPO/CHECKOUT/PATH/ansible/ansible_playbook.yml
```

## Start

After setup, all containers needs to get started executing

```
docker compose up -d
```

in setup directory.

## Upgrade

Ansible role is creating a cronjob which updates base images every night.

# Backup

The role creates a cron job which executes a [backup script](/ansible/roles/install-komga/templates/backup_komga.sh.j2) on a daily basis. To copy backup data to a backup server, rsync is used. To enable backup, please ensure there is a `rsync.pwd` in your installation path which contains rsync password.