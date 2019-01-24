---
description: >-
  ConfigMaps allow you to decouple configuration artifacts from image content to
  keep containerized applications portable.
---

# ConfigMaps

## Module Overview

At the end of this module, you will :

* _Learn the format of a YAML ConfigMaps file_
* _Learn how to manage a ConfigMaps_
* _Learn the composition of a ConfigMaps_

## Create

ConfigMap can be created from directories, files, or literal values thanks to the Kubectl client.

Independently from the deployment method, the command line to create a ConfigMaps should look like this :

```bash
kubectl create configmap CONFIGMAP_NAME DATA_SOURCE
```

* CONFIGMAP\_NAME is the name to assign to the ConfigMap 
* DATA\_SOURCE is the directory, file, or literal value to draw the data from

The data source corresponds to a key-value pair in the ConfigMap, where

* key is the file name or the key provided on the command line
* value is the file contents or the literal value provided on the command line

The _create_ command can directly ask the API resource to create a Service in command line or create a ConfigMap object based on a yaml file\(s\) definition.

### From literal values

Multiple key-value pairs can be passed to the Kubectl client to create a single ConfigMap. Each pair provided on the command line is represented as a separate entry in the data section of the ConfigMap.

In this particular method, the key / value are explicitly defined in the command line and each key / value will generate a new entry in the ConfigMap.

The command line for this kind of ConfigMaps creation should look like this :

```bash
kubectl create configmap CONFIGMAP_NAME --from-literal=KEY_NAME=VALUE
```

#### Exercise n°1

Create a ConfigMap named MySimpleConfigMap with those key / value pairs :

* course=kubernetes
* subject=configmap
* subject.title=MySimpleConfigMap

```bash
kubectl create configmap MySimpleConfigMap --from-literal=course=kubernetes --from-literal=subject=configmap --from-literal=subject.title=MySimpleConfigMap
```

{% hint style="info" %}
This method is not recommended in production. It is recommended to version the ConfigMaps files in a sources repository to manage it with a CI/CD tool.
{% endhint %}

### From a directory

Like any other object in Kubernetes, multiple ConfigMap can be created based on files stocked in a directory. The Kubectl client take care to parse each file in the directory given to ensure that each content files are integrated as ConfigMaps.

In this particular method, the key of the ConfigMap will be the name of the file and the value will be the entire content of the file.

The command line for this kind of ConfigMaps creation should look like this :

```bash
kubectl create configmap CONFIGMAP_NAME --from-directory=PATH_TO_DIRECTORY
```

#### Exercise n°1

1. Create a directory
2. Create a properties file in this directory
3. Copy / paste the content below in the new file
4. Create a ConfigMap based on the directory

```bash
course=kubernetes
subject=configmaps
subject.title=My first ConfigMap
```

### From a file

Like any other object in Kubernetes, multiple ConfigMap can be created based on multiple files stocked in a different directories. The Kubectl client take care to parse each file given to the command line to ensure that each content files are integrated as ConfigMaps.

In this particular method, the key of the ConfigMap will be the name of the file and the value will be the entire content of the file. In this method, it is possible to specify a key at the ConfigMap creation to avoid using the file name.

The command line for this kind of ConfigMaps creation should look like this :

```bash
# Without key specification
kubectl create configmap CONFIGMAP_NAME --from-file=PATH_TO_FILENAME

# With key specification
kubectl create configmap CONFIGMAP_NAME --from-file=KEY_NAME=PATH_TO_FILENAME
```

#### Exercise n°1

1. Create two files
2. Put some properties in each one
3. Create a ConfigMap based on the two property files created

```bash
kubectl create configmap CONFIGMAP_NAME --from-file=PATH_FILE1 --from-file=PATH_FILE2
```

## Get

The _get_ command list the object asked. It could be a single object or a list of multiple objects comma separated. This command is useful to get the status of each object. The output can be formatted to only display some information based on some JSON search or external tools like `tr`, `sort`, `uniq`.

The default output display some useful information about each ConfigMaps :

* Name : Name of the ConfigMap, needed to attach it to a Pod
* Data : The count of keys in the ConfigMap
* Age : Duration from the object creation

#### Exercise n°1

Get a list of ConfigMaps deployed in the default namespace.

```bash
kubectl get configmaps
```

## Describe

Once an object is running, it is inevitably a need to debug problems or check the configuration deployed.

The _describe_ command display a lot of configuration information about the ConfigMaps \(labels, resource requirements, etc.\) or any other Kubernetes objects, as well as status information about the ConfigMaps and Pod \(state, readiness, restart count, events, etc.\).

This command is really useful to introspect and debug an object deployed in a cluster.

#### Exercise n°1

Describe one of the ConfigMaps deployed in the default namespace.

```bash
kubectl describe configmaps CONFIGMAPS_NAME
```

## Attach

Once a ConfigMap is created, it has to be attach to a pod to be able to read the data within it.

The data can be attach as :

* Environment variable : The application in the container has to be able to read data from those environment variables.
* File : The application in the container has to be able to read the content in the specific files and find the needed keys to get the right value.

Multiple ConfigMaps can be defined in a Pod, this is useful when the configuration has to be managed by different teams.

### In environment variable

There is two method to define the content of a ConfigMap in a Pod :

* Define each variable individually
* Define each key / value pairs in a ConfigMap as an environment variable automatically

Depending of the ConfigMap content, defining each variable individually can be much complex and more secure than giving access to all the content of a ConfigMap.

#### Exercise n°1

Based on the previous ConfigMaps created, create a Pod using the busybox image to display some part of the ConfigMap in single environment variables.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myFirstConfigMapEnv1
spec:
  containers:
    - name: busybox
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      env:
        - name: CM_COURSE
          valueFrom:
            configMapKeyRef:
              name: MySimpleConfigMap
              key: course
        - name: CM_SUBJECT_TITLE
          valueFrom:
            configMapKeyRef:
              name: MySimpleConfigMap
              key: subject.title
  restartPolicy: Never
```

#### Exercise n°2

Based on the previous ConfigMaps created, create a Pod using the busybox image to display the entire ConfigMap in environment variables automatically.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myFirstConfigMapEnv2
spec:
  containers:
    - name: busybox
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "env" ]
      envFrom:
      - configMapRef:
          name: MySimpleConfigMap
  restartPolicy: Never
```

### In file path

With this method, the data of a ConfigMap is directly connected as a Volume to a Pod in a specific path in the container to be readable by the application deployed.

As the previous method, the content of a ConfigMap can entirely be copied in the specific file or only some specific keys can be defined to be available in the specific path.

{% hint style="info" %}
Be careful in the path used to mount the volumes, if a file already exist, it will be erased to mount the ConfigMap Volume.
{% endhint %}

#### Exercise n°1

Based on the previous ConfigMaps created, create a Pod using the busybox image to display some keys of the ConfigMap mounted as Volumes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myFirstConfigMapFile01
spec:
  containers:
    - name: busybox
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh","-c","cat /tmp/myconfigmap/mycourse" ]
      volumeMounts:
      - name: myConfigMap
        mountPath: /tmp/myconfigmap
  volumes:
    - name: myConfigMap
      configMap:
        name: MySimpleConfigMap
        items:
        - key: course
          path: mycourse
  restartPolicy: Never
```

#### Exercise n°2

Based on the previous ConfigMaps created, create a Pod using the busybox image to display the entire content of a ConfigMap mounted as Volumes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myFirstConfigMapFile02
spec:
  containers:
    - name: busybox
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh","-c","cat /tmp/myconfigmap" ]
      volumeMounts:
      - name: myConfigMap
        mountPath: /tmp/myconfigmap
  volumes:
    - name: myConfigMap
      configMap:
        name: MySimpleConfigMap
  restartPolicy: Never
```

## Explain

Kubernetes come with a lot of documentation about his objects and the available options in each one. Those information can be fin easily in command line or in the official Kubernetes documentation.

The _explain_ command allows to directly ask the API resource via the command line tools to display information about each Kubernetes objects and their architecture.

#### Exercise n°1

Get the documentation of a specific field of a resource.

```bash
kubectl explain configmaps.spec
```

Add the --recursive flag to display all of the fields at once without descriptions.

## Delete

The _delete_ command delete resources by filenames, stdin, resources and names, or by resources and label selector.

A ConfigMaps can only be deleted when it is not attached to a Pod.

Note that the delete command does NOT do resource version checks, so if someone submits an update to a resource right when you submit a delete, their update will be lost along with the rest of the resource.

#### Exercise n°1

Delete the previous ConfigMaps created.

```bash
kubectl delete configmaps CONFIGMAPS_NAME
```

## Module exercise

The purpose of this section is to manage each steps of the lifecycle of an application to better understand each concepts of the Kubernetes course.

The main objective in this module is to understand how to externalized some configuration part of an application to simplify the development and the lifecycle of both application and configuration.

For more information about the application used all along the course, please refer to the _Exercise App &gt; Voting App_ link in the left panel.

Based on the principles explain in this module, try by your own to handle this steps. The development of a yaml file is recommended.

The file developed has to be stored in this directory : `/data/votingapp/05_configmaps`

{% tabs %}
{% tab title="Exercise" %}
1. Create a ConfigMaps resource to externalize some part of the vote Pods :
   1. Named the ConfigMaps _vote_
   2. The ConfigMaps must manage those data : 
      1. `option_a: "KUBERNETES"`
      2. `option_b: "SWARM"`
2. Update the Deployment of the vote Pods to attach the ConfigMaps as environment variables :
   1. The name of the `option_a` environment variable has to be OPTION\_A
   2. The name of the `option_b` environment variable has to be OPTION\_B
{% endtab %}

{% tab title="Solution" %}
A command example to create the Secrets in command line.

```bash
kubectl create configmap vote -n voting-app --from-literal=option_a=KUBERNETES --from-literal=option_b=SWARM
```

An example of yaml definition file to update the Deployments of the vote Pods.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vote
  namespace: voting-app-prd
spec:
  replicas: 1
  selector:
    matchLabels:
      name: vote
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        name: vote
    spec:
      containers:
        - env:
            - name: "OPTION_A"
              valueFrom:
                configMapKeyRef:
                  name: vote
                  key: option_a
            - name: "OPTION_B"
              valueFrom:
                configMapKeyRef:
                  name: vote
                  key: option_b
          image: wikitops/examplevotingapp-vote:1.0
          imagePullPolicy: IfNotPresent
          name: vote
          ports:
            - containerPort: 8080
              name: vote
              protocol: TCP
```

Update the current Deployments resources.

```bash
kubectl apply -f /data/votingapp/05_configmaps/deployments.yaml
```
{% endtab %}
{% endtabs %}

## External documentation

Those documentations can help you to go further in this topic :

* Kubernetes official documentation on [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

