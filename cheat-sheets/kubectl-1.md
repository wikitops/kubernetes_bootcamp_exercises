---
description: >-
  Kubectl is a command line interface for running commands against Kubernetes
  clusters.
---

# Kubectl

## Installing

The kubectl version has to be within one minor version difference of the Kubernetes cluster. For example, a v1.2 client should work with v1.1, v1.2, and v1.3 master.

Kubectl can be installed on Ubuntu, Debian, CentOS, RedHat operating systems.

{% tabs %}
{% tab title="Ubuntu / Debian" %}
```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
{% endtab %}

{% tab title="CentOS / RedHat" %}
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
yum install -y kubectl
```
{% endtab %}
{% endtabs %}

For further information about kubectl installation method, please refer to the [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

## Syntax

Kubectl is a powerful tool to manage each object on a Kubernetes cluster. The command has a simple and unique syntax to manage everything :

```bash
kubectl [command] [TYPE] [NAME] [flags]
```

## External documentation

To go further in the management of Kubectl, please refer to these documentations :

* Official [Kubernetes documentation](https://kubernetes.io/docs/reference/kubectl/overview/) on Kubectl command line
* Official[ Kubernetes documentation ](https://kubernetes.io/docs/tasks/tools/install-kubectl/)to install Kubectl command line
* Official [Kubectl commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) details



