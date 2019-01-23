---
description: A deployment is a declarative way to manage the state of pods and replicas.
---

# Deployments

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML Deployment file_
* _Learn how to manage a Deployment_
* _Learn how to manage the lifecycle of an application_

## Create

A Deployment controller provides declarative updates for Pods and ReplicaSets.

Describe a desired state in a Deployment object, and the Deployment controller changes the actual state to the desired state at a controlled rate. A Deployment object can create, update or delete resources or parameters.

The _create_ command can directly ask the API resource to create a Deployment in command line or create a Deployment object based on a yaml file definition.

#### Exercise n°1

Create a Deployment object in command line named myNginxDeploymentCli to deploy an Nginx Pod.

```bash
kubectl create deployment myNginxDeploymet02 --image=nginx
```

#### Exercise n°2

Deploy an Nginx Pod in a declarative way with a Deployment object and scale it to 3.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myNginxDeploymentYaml
  labels:
    app: myNginxDeploymentYaml
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myNginxDeploymentYaml
  template:
    metadata:
      labels:
        app: myNginxDeploymentYaml
    spec:
      containers:
      - name: nginx
        image: nginx
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some json search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each services :

* Desired : Display the number of desired running Pods, similar to a ReplicaSet output
* Current : Display the number of current running Pods, similar to a ReplicaSet output
* Up-to-date : Display the number of up to date Pods when a Deployment is updated
* Available : Display the number of production ready Pods, similar to a ReplicaSet output

#### Exercise n°1

Get a list of the existing deployment object in the default namespace.

```bash
kubectl get deployment
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the Deployment\(s\) \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the Deployment\(s\) and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe a deployment in the default namespace.

```bash
kubectl describe deployment DEPLOYMENT_NAME
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain deployment.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Update

Users expect applications to be available all the time and developers are expected to deploy new versions of them several times a day. In Kubernetes this is done with rolling updates. **Rolling updates** allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones. The new Pods will be scheduled on Nodes with available resources.

Similar to application Scaling, if a Deployment is exposed publicly, the Service will load-balance the traffic only to available Pods during the update. An available Pod is an instance that is available to the users of the application.

### Rolling updates

The updates can be done based on the yaml file update or in command line depending on the kind of update needed. Remember, the command line modification are temporary and can be overwritten by an automated lifecycle manage like a Continuous Deployment tool. In production, it is highly recommended to versioned the yaml file in a git repository and ensure that each modification are tested on multiple environment before the production deployment.

The Kubernetes basic Rolling Updates architecture can be schematized like this :

![Kubernetes Rolling Updates architecture](../.gitbook/assets/rolling_update_architecture.gif)

The _rollout_ command allows to directly ask the API resource via the command line tools to manage the life cycle of a Deployment like displaying the older release with the _history_ parameter.

{% hint style="info" %}
The rolling update method does not apply only to the _Deployment_ object but to the _DaemonSet_ and the _StatefulSet_ too.
{% endhint %}

#### Exercise n°1

1. Check the history of the previous Nginx deployment
2. Describe the deployment to note the current version
3. In command line, update the Nginx version to the 1.9.1 in the previous deployment
4. Describe the deployment to ensure the version has changed
5. Check the history of the deployment to note the new entry

```bash
kubectl rollout history deployment DEPLOYMENT_NAME
kubectl describe deployment DEPLOYMENT_NAME | grep image
kubectl set image deployment/DEPLOYMENT_NAME nginx=nginx:1.9.1
kubectl describe deployment DEPLOYMENT_NAME | grep image
kubectl rollout history deployment DEPLOYMENT_NAME
```

#### Exercise n°2

Do the same operation to update Nginx in the 1.12.3 version but with a yaml file and add the `--record` parameter to the command. Get the deployment history to understand the difference between an update with or without the `--record` parameter.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mynginxdeployment
  labels:
    app: mynginxdeployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mynginxdeployment
  template:
    metadata:
      labels:
        app: mynginxdeployment
    spec:
      containers:
      - name: nginx
        image: nginx:1.12.3
```

### Rollback

In a Kubernetes cluster, a Rollback mean scaling up the corresponding ReplicaSet and scaled down the current one.

This can be done easily in command line by specifying the revision to rollback the deployment.

The _rollout_ command allows to directly ask the API resource via the command line tools to manage the life cycle of a Deployment like revert to an older version with the _undo_ parameter.

{% hint style="info" %}
The rolling update method does not apply only to the _Deployment_ object but to the _DaemonSet_ and the _StatefulSet_ too.
{% endhint %}

#### Exercise n°1

Rollback the Nginx deployment to the previous revision and check that the version of Nginx is back to 1.9.1.

```bash
kubectl rollout undo deployment DEPLOYMENT_NAME --to-revision=3
```

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

Deleting a Deployment will automatically delete each resources associated like Pods and ReplicaSet.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous deployment in command line.

```bash
kubectl delete deployment DEPLOYMENT_NAME
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to manage deployment object and their relation with Pods and ReplicaSet.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/03_deployments`

{% tabs %}
{% tab title="Exercise" %}
1. Develop the Deployment yaml file to deploy tha Voting App containers in Pods.
2. Ensure the app is up and running
{% endtab %}

{% tab title="Resolution" %}
```bash

```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
* Kubernetes official documentation on [rolling updates](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/)



