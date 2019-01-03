# Kubeadm

## Installing

Kubeadm can be installed on each Linux operating systems.

{% tabs %}
{% tab title="Ubuntu / Debian" %}
Some prerequisites has to be respected before running these commands.

```bash
apt-get update && apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```
{% endtab %}

{% tab title="CentOS / RedHat" %}
Some prerequisites has to be respected before running these commands.

```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable kubelet && systemctl start kubelet
```
{% endtab %}
{% endtabs %}

## Initiating

**First command to use** to initiate a Kubernetes cluster on a master node.

```bash
kubeadm init
```

{% hint style="info" %}
Take care of the return of this command line, it will contain the command to join other nodes.
{% endhint %}

### Specific address

Define the specific address on which the API server has to listen on.

```bash
kubeadm init -apiserver-advertise-address <IP_ADDRESS>
```

### Specific port

Define the specific port on which the API server listen has to listen on.

```bash
kubeadm init -apiserver-bind-port <PORT_NUMBER>
```

The default port used is 6443.

### Ignore preflights

Ignoring all the list of checks whose errors will be shown as warnings.

```bash
kubeadm init --ignore-preflight-errors all
```

### Node name

Give a name to the node

```text
kubeadm init --node-name <NODE_NAME>
```

By default, the host name of the server is passed to the command line.

## Joining

The previous parameters define in the initiating section are also available when joining a new node to an existing cluster but some new one can be used.

### Secure

Joining a new node to an existing cluster in secure mode thanks to an encoded discovery token.

```bash
kubeadm join --token <TOKEN> <MASTER_IP>:<MASTER_PORT> --discovery-token-ca-cert-hash sha256:<HASH>
```

## Resetting

Revert any changes made to a host wih kubeadm init command or kubeadm join command.

```bash
kubeadm reset
```

## Tokening

The token can be generated ahead of time and shared with the master and worker nodes, which can then bootstrap in parallel without coordination. Use a shared token along with the IP address of the API server to initiate a cluster and join new nodes.

### List

List each created token in the Kubernetes cluster.

```bash
kubeadm token list
```

### Create

Create bootstrap tokens on the server and print the join command line to easily reuse it.

```bash
kubeadm token create --print-join-command
```

### Generate

Generate and print a bootstrap token, but do not create it on the server.

```bash
kubeadm token generate
```

### Delete

Delete bootstrap tokens on the server.

```bash
kubeadm token delete <TOKEN>
```

## Configuring



## Upgrading

## Getting

Get some information about Kubeadm.

### Version

Print the version of Kubeadm.

```text
Kubeadm version
```

## External documentation

To go further in the management of Kubeadm, please refer to these documentations :

* Official [Kubernetes documentation](https://kubernetes.io/docs/setup/independent/install-kubeadm/) on installing Kubeadm
* Official [Kubernetes documentation](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) on initiating a Kubernetes cluster with Kubeadm
* Official [Kubernetes documentation](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/) on Kubeadm command



