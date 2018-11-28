# Minikube

## Installing

Minikube can be installed on Linux, Windows and Mac.

{% tabs %}
{% tab title="Linux" %}
On Linux, download the binary from the repository and add the execution right to it.

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
{% endtab %}

{% tab title="Windows" %}
[Chocolatey](https://chocolatey.org/) is a command line application installer for Windows. It adds, updates, and uninstalls programs in the background requiring very little user interaction.

To install Minikube on Windows with Chocolatey, run these commands as administrator.

```bash
choco install minikube
choco install kubernetes-cli
```
{% endtab %}

{% tab title="Mac" %}
[Homebrew](https://brew.sh/) is a package manager for macOS that can be used to install Minikube by following these steps.

```bash
brew cask install minikube
brew install kubernetes-cli
```
{% endtab %}
{% endtabs %}

Once Minikube is installed, each configuration files can be found in the home directory : 

```bash
%HOMEPATH%\.minikube
```

## Starting

Simply start Minikube with all the default options or restart a stopped Minikube cluster.

```text
minikube start
```

This command can be customized with external parameters explain in the next sections.

### VM driver

Depending on the operating system, the provider used to start the Kubernetes virtual machine can be changed in command line at the **first** starting boot. When it's done, a virtual machine will be created locally on the desired provider.

{% tabs %}
{% tab title="Virtualbox" %}
[VirtualBox](https://www.virtualbox.org/) is a powerful x86 and AMD64/Intel64 virtualization product for enterprise as well as home use. It can be installed on Linux, Windows and Mac operating system.

When it's done, just run these comand to run Minikube locally.

```bash
minikube start --vm-driver virtualbox
```
{% endtab %}

{% tab title="Hyper-V" %}
Microsoft Hyper-V \(formerly known as Windows Server Virtualization\), is a native hypervisor; it can create virtual machines on x86-64 systems running Windows. It has to enable on Windows operating system to run the Minikube instance locally.

[Here](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) is an example to enable Hyper-V on a Windows 10 operating system.

When it's done, just start Minikube by specifiying the desired VM driver.

```bash
minikube start --vm-driver hyperv
```
{% endtab %}
{% endtabs %}

### Kubernetes version

It can be useful for some reasons to deploy a specific Kubernetes version. This can be done at the **first** boot time of the Minikube cluster in command line.

```bash
minikube start --kubernetes-version="v1.12.0"
```

The Kubernetes versions available can be displayed with a getting command explain below.

### Flavor

The default Minikube flavor can be updated at the **first** boot by giving arguments at the command line.

```bash
minikube start --memory 5120 --cpus=4
```

## Stopping

Stopping Minikube will not destroy the virtual machine created, it just gonna stop it.

```bash
minikube stop
```

## Deleting

Deleting Minikube will gonna destroyed the virtual machine created at the first boot of the cluster. This will not affect the local configuration file of Kubectl.

```bash
minikube delete
```

## Getting

Once Minikube cluster is up and running, it can be useful to get information about the Kubernetes node.

### Minikube version

Getting the Minikube version.

```bash
minikube version
```

### Kubernetes version

Getting the Kubernetes versions available.

```bash
minikube get-k8s-versions
```

### Cluster status

Getting the status of the Minikube cluster.

```bash
minikube status
```

### IP address

Getting the public IP of the cluster to be able to work with the deployed resources.

```bash
minikube ip
```

### Service URL

Getting the Minikube URL of a service already deployed in the cluster.

```bash
minikube service --url=true SERVICE_NAME
```

## Connecting

Once the Minikube cluster is up and running, it can be useful to connect to the node with SSH to manage some Kubernetes resources.

```bash
minikube ssh
```

## Dashboard

Minikube automatically deployed the Kubernetes dashboard when a cluster is bootstraped. The minikube command line can automatically open it in the default Web browser on port 8001.

```bash
minikube dashboard
```

The default Minikube dashboard url should be : [http://localhost:8001/ui](http://localhost:8001/ui)

## Updating

Once the Minikube cluster is up and running, it may be updated in command line.

```bash
minikube update-context
```

## Logging

For debug purpose, it can be interesting to read the Minikube logs.

```bash
minikube logs
```

## Customizing

Some add-ons exist to automatically deploy some useful principles in a Kubernetes cluster to easily managed it.

### Install

The Minikube add-ons installation has to be made in command line.

```bash
minikube addons enable ADDONS_NAME
```

### List

Getting a exhaustive list of the available add-ons in Minikube.

```bash
minikube addons list
```

## External documentation

To go further in the management of Minikube, please refer to these documentations :

* Official [Minikube Github repository](https://github.com/kubernetes/minikube)
* Official [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-minikube/) of Minikube
* Official [Kubernetes documentation](https://kubernetes.io/docs/setup/minikube/) for running Minikube



