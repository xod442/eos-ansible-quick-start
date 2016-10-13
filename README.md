# Ansible + Arista Quick Start - Hackup from the mad scientist 
A simple Ansible setup to get you up and running faster!

## Intro
This repo includes automated playbooks that will help you run some basic playbooks against your Arista EOS switch. This setup is for a 4 spine and 6 leaf data center network.

You can use this with only 2 spines and 2 leafs...just edit the host file in the directory to remove the devices you do not have in your deployment

The all.yaml file in the group_vars directory hold all the variables to build out the data center with a base configuration (also readies the switch for import into cloud vision portal), ospf and bgp.


## Prereqs
* Docker
* A CLI user created on your switch
* An Arista EOS switch with eAPI enabled or SSH access
* Set the switches ip address in the /etc/host file of the docker container.

Bare metal switches or vEOS will work.

## Getting Started

### First clone this repo

```
git clone https://github.com/xod442/eos-ansible-quick-start.git
cd eos-ansible-quick-start
```

### Create the Docker Image ----if you want to skip this step then  "docker pull xod442/ansible" goto #2
```
1- docker build -t ansible .
```
Note: If you want to run Ansible from Source, run:
```
1-A docker build -t ansible-dev -f ./Dockerfile-dev .
```

### Run the Container
```
2- docker run -i -t -v $(pwd):/ansible-sample ansible OR docker run -i -t -v $(pwd):/ansible-sample xod442/ansible
```

Note: The ``-v`` will mount the files in this quickstart repo into the root
of the docker container. You will find this in ``/ansible-sample``. If you
created the development version, substitute ``ansible`` with ``ansible-dev``


### Run the Setup Playbooks

```
cd /ansible-sample
ansible-playbook baseconfiguration.yaml
ospf.yaml
bgp.yaml
```

### Run your first EOS playbook

``ansible-playbook -i hosts base_configuration.yaml -v``

You should get something like:

```
PLAY [all] *********************************************************************

TASK [Arista EOS Base Configuration] *******************************************
changed: [172.16.130.201] => {"changed": true, "responses": [{}, {}, {}, {}], "updates": ["ip name-server vrf default 8.8.8.8", "ip name-server vrf default 2.2.2.2", "hostname arista.makes.the.best.switches", "ip name-server vrf default 1.1.1.1"]}

PLAY RECAP *********************************************************************
172.16.130.201             : ok=1    changed=1    unreachable=0    failed=0
```

#### How it works

Let's break down the playbook command:

``ansible-playbook -i hosts base_configuration.yaml -v``

This says:

* Use the ansible-playbook command to run a playbook
* We use the ``-i hosts`` to specify our local ``hosts`` file instead of the default one in ``/etc/ansible/hosts``
* We want to run the ``base_configuration.yaml`` playbook
* We want to see some extra logging with ``-v`` verbosity

