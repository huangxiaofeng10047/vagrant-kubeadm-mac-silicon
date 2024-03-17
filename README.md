# Vagrant Kubeadm Cluster MAC Silicon 

Fully Automated Kubernetes setup on MAC Silicon M1/M2 laptops using Vagrant and VMWare Fusion.

It can be used as CKA, CKAD, and CKS practice lab.

Here is the high level workflow.

<p align="center">
  <img src="https://github.com/techiescamp/vagrant-kubeadm-mac-silicon/assets/106984297/8410a3d1-0d78-4f67-b9ae-01115d06b9ca" width="65%" />
</p>


## Prerequisites

A working Vagrant setup using VMware Fusion on MAC.

To install Vagrant & VMWare Fusion please refere this detailed Document [Vagrant + Vmware Fusion Setup](https://devopscube.com/build-vms-mac-silicon-with-vagrant/)

> Important Note: Please restart the system after the installation.


## Bring Up the Cluster

To provision the cluster, execute the following commands.

> Important Note: You have to use sudo with all the vagrant commands.

```shell
git clone https://github.com/techiescamp/vagrant-kubeadm-mac-silicon.git
cd vagrant-kubeadm-mac-silicon
sudo vagrant up
```
## Set Kubeconfig file variable

You can connect to the Vagrant cluster from your local mac terminal by configuring the following.

```shell
cd vagrant-kubeadm-mac-silicon
cd configs
sudo chmod +r config 
export KUBECONFIG=$(pwd)/config
```

or you can copy the config file to .kube directory.

```shell
cp config ~/.kube/
```

Validate the cluster access

```shell
kubectl get po -n kube-system
```

## Install Kubernetes Dashboard

The dashboard is automatically installed by default, but it can be skipped by commenting out the dashboard version in _settings.yaml_ before running `vagrant up`.

If you skip the dashboard installation, you can deploy it later by enabling it in _settings.yaml_ and running the following:
```shell
vagrant ssh -c "/vagrant/scripts/dashboard.sh" controlplane
```

## Kubernetes Dashboard Access

To get the login token, copy it from _config/token_ or run the following command:
```shell
kubectl -n kubernetes-dashboard get secret/admin-user -o go-template="{{.data.token | base64decode}}"
```

Make the dashboard accessible:
```shell
kubectl proxy
```

Open the site in your browser:
```shell
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
```

## To shutdown the cluster,

```shell
sudo vagrant halt
```

## To restart the cluster,

```shell
sudo vagrant up
```

## To destroy the cluster,

```shell
sudo vagrant destroy -f
```
