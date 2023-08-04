# Le Potato Cluster using LINBIT Software

This repository includes Ansible playbooks and configuration templates for installing
and configuring software from [LINBIT®](https://linbit.com/) onto aarch64 based
[Le Potato SBCs](https://libre.computer/products/aml-s905x-cc/).

Each playbook in this repo will configure a different stack of LINBIT's software.
Currently, you may use this repository to configure the Potatoes with software from
[LINBIT's Ubuntu PPA](https://launchpad.net/~linbit/+archive/ubuntu/linbit-drbd9-stack),
and optionally configuring the
[a demo DRBD Reactor cluster](https://www.youtube.com/watch?v=m2YT2BfwJcQ). Or, you
can configure the Potatoes with k3s and [LINBIT SDS](https://linbit.com/kubernetes/)
for Kubernetes.

**NOTE:** LINBIT SDS for Kubernetes requires an evaluation or customer account with
LINBIT. [Contact LINBIT](https://linbit.com/contact-us/) for such evaluation access,
or consider modifying the playbook to deploy
[Piraeus](https://github.com/piraeusdatastore/piraeus-operator), LINBIT SDS's
upstream project also from the developers at LINBIT.

## How to Use

1. Install the latest Ubuntu LTS distribution onto 3 or more Le Potatoes.

1. Configure passwordless SSH to the `ubuntu` user (or some non-root user) on each 
Le Potato from the Ansible control host.

1. Attach "clean" USB storage devices to 2 or more of the Le Potatoes - `wipefs -af`
on each USB storage device to "clean".

1. Adjust hosts and variables in `hosts.ini` accordingly for your network. DNS names
should resolve. Add `/etc/hosts` entry if proper DNS configuration isn't possible.

1. Run on of the playbooks within the Git checkout:
  * To configure the Potatoes with software from LINBIT's Ubuntu PPA, run the
    `linbit-ppa.yaml` playbook from within the Git checkout:
    `ansible-playbook linbit-ppa.yaml`
  * Optionally, you may then run the DRBD Reactor demo playbook:
    `ansible-playbook www-reactor-demo.yaml`
  * To conifgure the Potatoes with k3s and LINBIT SDS, run the `k3s-linstor_v2.yaml`
    playbook from within the Git checkout:
    `ansible-playbook -e lb_user=username -e lb_pass=password k3s-linstor_v2.yaml`
    Where "username" and "password" are your credentials for your
    [LINBIT Portal](https://my.linbit.com/) account.

The `linbit-ppa.yaml` playbook will update the Le Potatoes to the latest packages
and install everything from LINBIT's PPA while also initializing the SBCs in a
LINSTOR cluster. This will not configure any storage pools or make any
"hard to revert" configurations in the cluster. From here you can either get your
hands dirty following
[LINBIT's documentation](https://linbit.com/user-guides-and-product-documentation/)
or run one of the `*-demo.yaml` playbooks to configure a cluster, e.g.:

```
ansible-playbook www-reactor-demo.yaml
```

The `k3s-linstor_v2.yaml` will configure the Potatoes as an HA k3S cluster, deploy
LINBIT SDS for Kubernetes into the cluster using the LINSTOR Operator, as well as
setting up some generic LINSTOR storage classes within Kubernetes.
