# Le Potato Cluster using LINBIT Software

This repository includes ansible playbooks and configuration templates for installing
and configuring software from
[LINBIT's Ubuntu PPA](https://launchpad.net/~linbit/+archive/ubuntu/linbit-drbd9-stack)
onto aarch64 based [Le Potato SBCs](https://libre.computer/products/aml-s905x-cc/).

## How to Use

1. Install the latest Ubuntu LTS distribution onto 3 or more Le Potatos.

1. Configure passwordless SSH to the `ubuntu` user (or some non-root user) on each 
Le Potato from the ansible control host.

1. Attach "clean" USB storage devices to 2 or more of the Le Potatos - `wipefs -af`
on each USB storage device to "clean".

1. Adjust hosts and variables in `hosts.ini` accordingly for your network. DNS names
should resolve. Add `/etc/hosts` entry if proper DNS configuration isn't possible.

1. Run the `linbit-ppa.yaml` playbook from within the Git checkout:

```
ansible-playbook linbit-ppa.yaml
```

That will update the Le Potatos to the latest packages and install everything from
LINBIT's PPA while also initializing the SBCs in a LINSTOR cluster. This will not
configure any storage pools or make any "hard to revert" configurations in the cluster.

From here you can either get your hands dirty following 
[LINBIT's documentation](https://linbit.com/user-guides-and-product-documentation/) or
run one of the `*-demo.yaml` playbooks to configure a cluster, e.g.:

```
ansible-playbook www-reactor-demo.yaml
```
